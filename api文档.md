------

# api文档

接口地址：https://php.kfirstsnowlucky.cn

------

## Php_laravel_api

### 1，login

请求方式：POST

请求格式：/api/v1/login

示例：

```json
{
    "email":"admin@lucky.com",
    "password":"123456"
}
```

返回结果：

```json
{
    "msg": "success",
    "data": {
        "id": 1,
        "email": "admin@lucky.com",
        "full_name": "admin",
        "token": "c845c87fd24faf971f722d6367958d97",
        "create_time": "2023-08-11 00:00:00"
    }
}
```

2，login

请求方式：POST

请求格式：/api/v1/logout?token=你的登录返回的token

示例：/api/v1/logout?token=c845c87fd24faf971f722d6367958d97

返回结果：

```json
{
    "msg": "success"
}
```

没有token或token失效时返回的结果：

```json
{
    "msg": "token失效"
}
```

