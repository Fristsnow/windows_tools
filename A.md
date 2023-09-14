# A

## Client

#### 1.Send a Package

api.php

```php

Route::apiResource('/package', \App\Http\Controllers\PackageController::class);
```

PackageController

```php
public function store(StorePackageRequest $request)
    {
        //
        $data = $request->post();
        $data['sender_id'] = Auth::guard('client')->id();
        $couriers = Campus::find($data['from_campus_id'])->couriers;
//        dd($couriers);
        if (!count($couriers))
            throw new BaseException(409, 'no available couriers in the campus');
        $data['courier_id'] = $couriers[rand(0, count($couriers) - 1)]->id;
        $package = Package::create($data);
        $package->tracking_number = 'CE'
            . substr($package->from_campus, strlen($package->from_campus) - 1, 1)
            . substr($package->to_campus, strlen($package->to_campus) - 1, 1)
            . date('Ymd')
            . fake()->numberBetween(100, 999);
        $package->save();

        Progress::create([
            'status' => 'Pending pickup',
            'package_id' => $package->id,
            'returning' => 0,
            'courier_id' => $data['courier_id']
        ]);
        return $this->success201([
            'package_id' => $package->id,
            'courier_name' => $package->courier->name,
            'courier_phone_number' => $package->courier->phone_number
        ]);
    }
```

Campus:

```php
public function couriers()
    {
        return $this->hasMany(Staff::class, 'affiliated_campus_id', 'id')->where([
            ['role', '=' ,'courier'],
            ['online','=', true]
        ]);
    }
```

Package:

```php

    protected $appends = [
        'from_campus',
        'to_campus',
    ];

    public function courier()
    {
        return $this->belongsTo(Staff::class, 'courier_id');
    }

    public function get_from_campus()
    {
        return $this->belongsTo(Campus::class, 'from_campus_id');
    }

    public function get_to_campus()
    {
        return $this->belongsTo(Campus::class, 'to_campus_id');
    }

    public function getFromCampusAttribute()
    {
        return $this->get_from_campus()->first()->name;
    }

    public function getToCampusAttribute()
    {
        return $this->get_to_campus()->first()->name;
    }

```

Staff

```php
    public function getNameAttribute()
    {
        return $this->firstname . ' ' . $this->lastname;
    }
```

#### 2.Get Package

Package

```php
protected $appends = [
        'recipient'
];
 public function getRecipientAttribute()
    {
        return [
            'name' => $this->recipient_name,
            'phone_number' => $this->recipient_phone_number
        ];
    }
```

PackageController

```php
  public function index()
    {
        //
        $client = Auth::guard('client')->user();
        return $this->success200($client->packages->map(function ($item) {
            return collect($item->toArray())->except('delivered_time');
        }));
    }
```

