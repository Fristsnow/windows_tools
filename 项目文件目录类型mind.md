# 一、项目文件目录类型

## Java类下（Src/main/Java）

### 1.	Common（逻辑配置）

##### (1)	全局后端跨域配置文件

参考链接（http://t.csdn.cn/o9G6F)

文件名：CorsConfig.java         //建议使用此名称

```java
package com.《项目名称》.common                          //java类文件自动生成


import org.springframework.context.annotation.Configuration;                    //导包（可自动生成）
import org.springframework.web.servlet.config.annotation.CorsRegistry;          //导包（可自动生成）
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;      //导包（可自动生成）


@Configuration                                                                                               
public class CorsConfig implements WebMvcConfigurer {
    
	@Override
	public void addCorsMappings(CorsRegistry registry){
		registry.addMapping(pathPattern"/**")
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

##### (2)	前端请求数据返回值配置文件

文件名：Result.java          //建议使用此名称

```java
package com.《项目名称》.common                          //java类文件自动生成

import lombok.Data;

@Data
public class Result {
    private int code;
    private String msg;
    private Long total;
    private Object data;                     
    
    //创建变量名称
    
    private static Result result(int code,String msg,Long total,Object data){
        Result res =new Result();
        res.setCode(code);
        res.setMsg(msg)
        res.setTotal(total);
        res.setData(data);
        return res;
    //设置变量数据
    }
    
    public static Result fail(){	
        return result(code:400,msg:"失败"，total:0L，data:null);
    }
    public static Result suc(Object data){
        return result(code:200,msg:"成功"，total 0L,data);
    }
}
```

### 2.	Controller（前端逻辑代码）

文件名：《组件名》+Controller        //建议取名类型

```java
package com.《项目名称》.common                          //java类文件自动生成

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.wxs.common.Result;
import com.wxs.service.pmer.UserService;
import com.wxs.shiti.Users;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequesMapping("/user")      //通行证
public class 《组件名》+Controller  {
    @Autowired
    private UserService userService;
    
    @GetMapping("/list")       //查询所有数据
    public List<User> list(){return userService.listAll();}
    
    @PostMapping("/New")       //新增数据
    public boolean New(@RequestBody Users users) {return userService,save(user);}
    
    @postMapping("login")      //登录数据
    public Result login(@RequestBodt User users){
        List list=userService.lambdaQuery()
            .eq(Users::getUsername,users.getUsername())
            .eq(Users::getPassword,user.getPassword()).list();
        return list .size()>0?Result.suc(list.get(0)):Result.fail();
    }
    
    @PostMapping("/update")       //更新数据
    public boolean update(@RequestBody Users users) {return userService.updateById(users);}
    
    @PostMapping("/newOrUpdate")      //新增或更新数据
    public boolean newOrUpdate(@RequestBody Users users) {return userService.saveOrUpdate(users);}
    
    @GetMapping("/delete")      //删除数据
    pubic boolean delet(Intefer id) {return userService.removeById(id);}
    
    @PostMapping("/list")      //查询指定的数据
    public List<Users> list(@RequesrBody Users users){
        LambdaQueryWrapper<Users> lambdaQueryWrapper = new LambdaQueryWrapper<>();
        lambdaQueryWrapper.eq(Users:getUsername,users.getUsername());
        return userService.list(lambdaQueryWrapper);
    }
}
```

### 3.	mapper（API接口文件）

文件名：《组件名》+mapper        //建议取名类型

```java
package com.《项目名称》.common                          //java类文件自动生成

【自动导包】

@Mapper
public interface 《组件名》+mapper extends BaseMapper<Users> {
    List<Users> listAll();
}
```

### 4.	service

##### (1).	接口服务（包名自定义）

文件名：《组件名》+service        //建议取名类型

```java
package com.《项目名称》.common                          //java类文件自动生成

【自动导包】
    
public interface UserService extends IService<Users> {
    List<Users> listAll();
}
```

##### (2).	接口逻辑（service根目录）

文件名：《组件名》+service        //建议取名类型

```java
package com.《项目名称》.common                          //java类文件自动生成

【自动导包】
    
@Service
public class ServiceShiti extends ServiceImpl<UserMapper, Users> implements UserService {

    @Resource
    private UserMapper userMapper;
    @Override
    public List<Users> listAll() {
        return userMapper.listAll();
    }
}
```

### 5.	实体（声明表头）

文件名：与数据库名称对应          //必须

```java
package com.《项目名称》.common                          //java类文件自动生成
    
import lombok.Data;

@Data
public class Users {
    private int id;
    private String username;
    private String password;
}
```

————————————————————————分割线—————————————————————————————————

# 二、resources文件

### 1.	Mapper文件（SQL数据库命令集）

文件名：《组件名》+Mapper        //建议取名类型

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wxs.mapper.UserMapper">
    <select id="listAll" resultType="com.wxs.shiti.Users">
        select * from users
    </select>
</mapper>
```

### 2.	application(文件改名为YML格式)

```yml
server:
  port: ****     //端口号

spring:
  datasource:
    url: jdbc:mysql://《数据库地址库名+》?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=GMT%2B8
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: *****       //数据库账号
    password: *****       //数据库密码
```

————————————————————————分割线—————————————————————————————————

# 三、pom.xml

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.2.0</version>
</dependency>
```

