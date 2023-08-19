# 前后端数据交互(cct项目)：

------

[^注：基于String boot，该项目基于String boot + vue + ElementUI + axios]: 

## cct String boot

### main

#### Java

##### common

###### CorsConfig

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    
	@Override
	public void addCorsMappings(CorsRegistry registry){
		registry.addMapping("/**")
            	//是否发送Cookie
            	.allowCredentials(true)
            	//放行哪些原始域
            	.allowedOriginPatterns("*")
            	.allowedMethods(new String[]{"GET","POST","PUT","DELETE"})
            	.allowedHeaders("*")
				.exposedHeaders("*");
	}
}
```

###### Result

```java
@Data
public class Result {
    private int code;
    private String msg;
    private Long total;
    private Object data;

    public static Result result(int code,String msg,Long total,Object data){
        Result res = new Result();
        res.setCode(code);
        res.setMsg(msg);
        res.setTotal(total);
        res.setData(data);
        return res;
    }
    public static Result fail(){
        return result(400,"失败",0L,null);
    }
    public static Result suc(){
        return result(200,"成功",0L,null);
    }
    public static Result suc(Object data){
        return result(200,"成功",0L,data);
    }
    public static Result suc(Long total,Object data) {
        return result(200,"成功",total,data);

    }
}
```



##### controller

###### CctuserController

```JAVA
@RestController
@RequestMapping("/table")
public class CctuserController {

    @Autowired
    private CctuserServiceEntity cctuserServiceEntity;

//    查询
    @GetMapping("/cctuser")
    public List<Cctuser> list(){
        return cctuserServiceEntity.listAll();
    }

    //    修改        ====>       必须有id
//    @GetMapping("/updata")
////                    .eq(Cctuser::getId,cctuser.getId())
//    public Result mod(@RequestBody Cctuser cctuser){
////        List list=cctuserServiceEntity.lambdaQuery()
////                .eq(Cctuser::getName,cctuser.getName())
////                .eq(Cctuser::getAddress,cctuser.getAddress())
////                .eq(Cctuser::getGonglv,cctuser.getGonglv())
////                .eq(Cctuser::getVvs,cctuser.getVvs())
////                .eq(Cctuser::getZhangtai,cctuser.getZhangtai())
////                .eq(Cctuser::getTime,cctuser.getTime())
////                .eq(Cctuser::getBeizhu,cctuser.getBeizhu()).list();
//        return cctuserServiceEntity.save(cctuser)?Result.suc():Result.fail();
//    }
//    @PostMapping("/login")
//    public Result login(@RequestBody Users users){
//        List list=userService.lambdaQuery()
//                .eq(Users::getUsername,users.getUsername())
//                .eq(Users::getPassword,users.getPassword()).list();
//        return list.size()>0?Result.suc(list.get(0)):Result.fail();
//    }

//    新增
    @PostMapping("/new")
    public Result save(@RequestBody Cctuser cctuser){
        return cctuserServiceEntity.save(cctuser)?Result.suc():Result.fail();
    }
    //    新增或者修改
    @PostMapping("/neworupdata")
    public boolean saveOrMod(@RequestBody Cctuser cctuser){
        return cctuserServiceEntity.saveOrUpdate(cctuser);
    }
    //      删除
    @GetMapping("/delete")
    public boolean delete(Integer id){
        return cctuserServiceEntity.removeById(id);
    }
}

```



##### entity

###### Cctuser

```java
package com.example.cct.entity;


import lombok.Data;

import java.sql.Date;

@Data
public class Cctuser {
    private int id;
    private String name;
    private String address;
    private String gonglv;
    private String vvs;
    private String zhangtai;
    private String time;
    private String beizhu;
}

```

##### mapper

###### CctuserMapper

```java
package com.example.cct.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.cct.entity.Cctuser;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

@Mapper
public interface CctuserMapper extends BaseMapper<Cctuser> {
    List<Cctuser> listAll();
}

```



##### service

###### CctuserService

```
package com.example.cct.service.entitymapper;

import com.baomidou.mybatisplus.extension.service.IService;
import com.example.cct.entity.Cctuser;

import java.util.List;

public interface CctuserService extends IService<Cctuser> {
    List<Cctuser> listAll();
}

```

###### CctuserServiceEntity

```java
package com.example.cct.service;

import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.example.cct.entity.Cctuser;
import com.example.cct.mapper.CctuserMapper;
import com.example.cct.service.entitymapper.CctuserService;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;
import java.util.List;

@Service
public class CctuserServiceEntity extends ServiceImpl<CctuserMapper, Cctuser> implements CctuserService {

    @Resource
    private CctuserMapper cctuserMapper;

    @Override
    public List<Cctuser> listAll() {
        return cctuserMapper.listAll();
    }
}

```



#### resources

##### mapper

###### *.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.cct.mapper.CctuserMapper">
    <select id="listAll" resultType="com.example.cct.entity.Cctuser">
        select * from cctuser
    </select>
</mapper>
```

##### *.yml

```yml
server:
  port: 8085

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cct?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=GMT%2B8
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: WANGXINYU1119



```

### pom.xml

```java
	<dependency>
			<groupId>com.baomidou</groupId>
			<artifactId>mybatis-plus-boot-starter</artifactId>
			<version>3.2.0</version>
	</dependency>
```



## cctweb

### src：

#### components：

##### Tables.vue：

```vue
<template>
    <div class="home1">
        <div>
            <div style="font-weight: bold;font-size: 20px">
                <p>固定碳排放</p>
            </div>
            <!--        头部搜索框-->
            <div style="display: flex;margin-bottom: 20px">
                <div>
                    <el-input style="width: 800px" v-model="search" placeholder="请输入关键字查找"></el-input>
                </div>
                <div style="margin-left: 7px">
                    <el-button type="success">搜索</el-button>
                </div>
                <div style="margin-left: 7px">
                    <el-button type="primary" @click="newbtn">新增</el-button>
                </div>
            </div>
            <!--        内容区域-->
            <el-table :data="table.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
                      height="550px" border style="width: 100%"
                      :header-cell-style="{background:'#cccccc',color:'#000','text-align':'center'}"
                      :row-style="{height:'37px'}"
                      :cell-style="{padding: '0','text-align':'center'}">
                <el-table-column prop="id" label="编号" width="50"></el-table-column>
                <el-table-column prop="name" label="检测点名称" width="100px"></el-table-column>
                <el-table-column prop="address" label="检测点地点" width="100px"></el-table-column>
                <el-table-column prop="gonglv" label="实时功率" width="100px"></el-table-column>
                <el-table-column prop="vvs" label="实时电压" width="100px"></el-table-column>
                <el-table-column prop="zhangtai" label="状态" width="80px"></el-table-column>
                <el-table-column prop="time" label="上报时间" width="120px"></el-table-column>
                <el-table-column prop="beizhu" label="备注" width="130px"></el-table-column>
                <el-table-column prop="" :label="this.deletes" width="200px">
                    <template slot="header">
                        <span>操作</span>
                    </template>
                    <template slot-scope="scope">
                        <el-button size="mini" type="primary" @click="updatebtn">{{xiugai}}</el-button>
                        <el-button size="mini" type="danger" @click.native.prevent="deleteRow(scope.$index, table)">
                            {{deletes}}
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <div>
            <!--            智造双碳数据修改-->
            <el-dialog title="智造双碳数据修改" :visible.sync="dialog" width="50%">
                <el-form ref="form" :model="from">
                    <el-row>
                        <el-form-item label="编号:">
                            <el-input v-model="from.cctid"/>
                            <br>
                        </el-form-item>
                        <el-form-item label="检测点名称:">
                            <el-input v-model="from.cttname"/>
                            <br>
                        </el-form-item>
                        <el-form-item label="检测点地点:">
                            <el-input v-model="from.address"/>
                            <br>
                        </el-form-item>
                        <el-form-item label="实时功率:">
                            <el-input v-model="from.gonglv"/>
                            <br>
                        </el-form-item>
                        <el-form-item label="实时电压:">
                            <el-input v-model="from.vvs"/>
                            <br>
                        </el-form-item>
                        <el-form-item label="状态:">
                            <el-input v-model="from.zhangtai"/>
                            <br>
                        </el-form-item>
                        <el-form-item label="备注:">
                            <el-input v-model="from.beizhu"/>
                            <br>
                        </el-form-item>
                    </el-row>
                </el-form>
                <div slot="footer">
                    <el-button type="primary" @click="btnn(from)">确定</el-button>
                </div>
            </el-dialog>
            <!--            智造双碳数据新增-->
            <el-dialog title="智造双碳数据新增" :visible.sync="dialog1" width="50%">
                <el-form ref="form" :rules="rules" :model="from" label-width="100px">
                    <el-form-item label="检测点名称:" prop="name">
                        <el-input v-model="from.name" autocomplete="off"/>
                    </el-form-item>
                    <el-form-item label="检测点地点:" prop="address">
                        <el-input v-model="from.address" autocomplete="off"/>
                    </el-form-item>
                    <el-form-item label="实时功率:" prop="gonglv">
                        <el-input v-model="from.gonglv" autocomplete="off"/>
                    </el-form-item>
                    <el-form-item label="实时电压:" prop="vvs">
                        <el-input v-model="from.vvs" autocomplete="off"/>
                    </el-form-item>
                    <el-form-item label="状态:" prop="zhangtai">
                        <el-input v-model="from.zhangtai" autocomplete="off"/>
                    </el-form-item>
                    <el-form-item label="备注:" prop="备注">
                        <el-input v-model="from.beizhu" autocomplete="off"/>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="btn()">确定</el-button>
                    </el-form-item>
                </el-form>
            </el-dialog>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Tables',
        data() {
            return {
                xin: '新增',
                xiugai: '修改',
                deletes: '删除',
                table: [],
                dialog: false,
                dialog1: false,
                input: '',
                search: '',
                from: {
                    name: '',
                    address: '',
                    gonglv: '',
                    vvs: '',
                    zhangtai: '',
                    time:'2005-06-09',
                    beizhu: ''
                },
                rules: {
                    name: [
                        {required: true, message: '请输入检测点名称', trigger: 'blur'},
                        {min: 3, max: 8, message: '长度在 3 到 8 个字符', trigger: 'blur'}
                    ],
                    address: [
                        {required: true, message: '请输入检测点地点', trigger: 'blur'},
                    ],
                    gonglv: [
                        {required: true, message: '实时功率', trigger: 'blur'},
                    ],
                    vvs: [
                        {required: true, message: '实时电压', trigger: 'blur'},
                    ],
                    zhangtai: [
                        {required: true, message: '状态', trigger: 'blur'},
                    ]
                }
            }
        },
        created() {
            this.$axios({
                url: this.$httpurl + '/table/cctuser',
                method: 'get',
                data: {}
            }).then((res) => {
                console.log(res)
                this.table = res.data;
            }).catch((err) => {
                console.log(err)
            })
        },
        methods: {
            deleteRow(index, rows) {
                this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning',
                    center: true
                }).then(() => {
                    rows.splice(index, 1);
                    this.$message({
                        type: 'success',
                        message: '删除成功!'
                    });
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '已取消删除'
                    });
                });
            },
            updatebtn: function () {
                this.dialog = true
            },
            newbtn: function () {
                this.dialog1 = true
            },
            btn() {
                if (this.from.name == '' || this.from.address == '' || this.from.gonglv == '' || this.from.vvs == '' || this.from.zhangtai == '') {
                    this.$message({
                        showClose: true,
                        message: '请填写完必填数据！！！！',
                        type: 'error'
                    });
                } else {
                    this.$axios({
                        url: this.$httpurl + '/table/new',
                        method: 'post',
                        data: this.from
                    }).then((res) => {
                        console.log(this.from)
                        if (res.data.code == '200') {
                            this.$message({
                                showClose: true,
                                message: '恭喜你，这是一条成功消息',
                                type: 'success'
                            });
                            this.$router.go(0)
                        } else {
                            this.$message({
                                showClose: true,
                                message: '添加数据失败',
                                type: 'error'
                            });
                        }
                    }).catch((err) => {
                        console.log(err)
                    })
                }
            }
        }
    }
</script>
<style>
    
</style>
```

##### ListTables.vue：

```vue
<template>
    <div>
        <div style="margin-bottom: 10px">
            <p style="font-weight: bold;font-size: 20px">能耗数据采集</p>
        </div>
        <div style="margin-bottom: 20px">
            <div style="display: flex;">
                <el-input type="text" v-model="sousuo" placeholder="请输入你要查找的设备名称"></el-input>
                <el-button style="margin: 0 10px" type="success">搜索</el-button>
                <el-button type="primary" @click="btn">重置</el-button>
            </div>
        </div>
        <el-table :data="cctds.filter(data => !sousuo || data.name.toLowerCase().includes(sousuo.toLowerCase()))"
                  height="550px" border style="width: 100%;"
                  :header-cell-style="{background:'#409EFF',color:'#000','text-align':'center'}"
                  :row-style="{height:'37px'}" :cell-style="{padding:'0px','text-align':'center'}">
            <el-table-column prop="name" label="设备名称" width="160px"></el-table-column>
            <el-table-column prop="yichan" label="已产数量" width="160px"></el-table-column>
            <el-table-column prop="haodian" label="单位耗电量" width="160px"></el-table-column>
            <el-table-column prop="alldian" label="总耗电量" width="160px"></el-table-column>
            <el-table-column prop="tan" label="单位碳排放" width="160px"></el-table-column>
            <el-table-column prop="alltan" label="总碳排放" width="160px"></el-table-column>
        </el-table>
    </div>
</template>

<script>
    export default {
        name: "Updatabtn",
        data() {
            return {
                sousuo: '',
                cctds: []
            }
        },
        created() {
            this.$axios({
                url: this.$httpurl + '/cct/cctdata',
                method: 'post'
            }).then((res) => {
                this.cctds = res.data
                console.log(res)
            }).catch((err) => {
                console.log(err)
            })
        },
        methods: {
            btn: function () {
                this.sousuo = null
            }
        }
    }
</script>

<style scoped>

</style>
```

#### router：

##### router.js：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Index from '../views/Index.vue'
import Mine from "@/views/Mine";
import List from "@/views/pages/List";

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'index',
    component: Index,
    // children: [{
    //   path: '/components/Tables',
    //   name: 'Tables',
    //   component:()=>import('@/components/Tables')
    // }]
  },
  {
    path: '/mine',
    name: 'mine',
    component: Mine,

  },
  {path: '/pages/List',name: List,component: List}


]

const router = new VueRouter({
  routes
})

export default router

```



#### App.vue：

```vue
<template>
    <div id="app">
        <div>
            <el-col style="width: 200px;margin-right: 15px" :span="12">
                <el-menu default-active="1" router="true" style="width: 200px;margin-right: 5px;height: 700px"
                         background-color="#0044a5" text-color="#ded5ca">
                    <el-menu-item index="/">
                        <template slot="title">
                            <i class="el-icon-location"></i>
                            <span>Fisrtsnow</span>
                        </template>
                    </el-menu-item>
                    <el-submenu>
                        <template slot="title">
                            <i class="el-icon-location"></i>
                            <span>制造双碳-能效预警</span>
                        </template>
                        <el-menu-item index="/mine">
                            <span>固定碳排放</span>
                        </el-menu-item>
                        <el-menu-item index="/pages/List">
                            <span>能耗数据采集</span>
                        </el-menu-item>
                    </el-submenu>
                    <el-menu-item index="2">
                        <i class="el-icon-menu"></i>
                        <span slot="title">智能双碳-双碳配置</span>
                    </el-menu-item>
                    <el-menu-item index="3">
                        <i class="el-icon-document"></i>
                        <span slot="title">制造双碳-能效预警</span>
                    </el-menu-item>
                    <el-menu-item index="4">
                        <i class="el-icon-setting"></i>
                        <span slot="title">个人中心</span>
                    </el-menu-item>
                </el-menu>
            </el-col>
        </div>
        <div>
            <router-view/>
        </div>
    </div>
</template>

<style>
#app{
    width: 100%;
    height: 700px;
    background-color: #f2f5fd;
}
</style>

```

