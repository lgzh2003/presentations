### Chapt 3: Getting Started     
<a href="/smart-framework.md"> Main page </a> |<a href="/chapter/chapter2-preparation.md">  Previous page </a>      

#####1. Create a Maven Web Project    
The directory structure of the whole project are as follows:
```sh
smart-sample/
　　┗ src/
　　　　┗ main/
　　　　　　┗ java/
　　　　　　┗ resources/
　　　　　　┗ webapp/
　　┗ pom.xml
```
In java catalog,create the following directory structure package names:
```sh
org/
　　┗ smart4j/
　　　　┗ sample/
　　　　　　┗ action/
　　　　　　┗ entity/
　　　　　　┗ service/
```
#####2.Configuring **Maven** Dependency     
Edit pom.xml file, Add smart-framework dependency:
```sh
<dependency>
    <groupId>org.smart4j</groupId>
    <artifactId>smart-framework</artifactId>
    <version>[Version Number]</version>
</dependency>
```
#####3.Edit **Smart** Configuration:
```sh
smart.framework.app.base_package=org.smart4j.sample
smart.framework.app.home_page=/users
smart.framework.jdbc.driver=com.mysql.jdbc.Driver
smart.framework.jdbc.url=jdbc:mysql://localhost:3306/smart-sample
smart.framework.jdbc.username=root
smart.framework.jdbc.password=root
```
#####4.Write **Enity** Class
```sh
package org.smart4j.sample.entity;
import org.smart4j.framework.orm.annotation.Entity;
@Entity
public class User {
    private long id;
    private String username;
    private String password;
    // getter/setter
}
```
#####5.Write Service Interface and Implementation
- Service Interface
```sh
package org.smart4j.sample.service;

import java.util.List;
import java.util.Map;
import org.smart4j.sample.entity.User;

public interface UserService {
    List<User> findUserList();
    User findUser(long id);
    boolean saveUser(Map<String, Object> fieldMap);
    boolean updateUser(long id, Map<String, Object> fieldMap);
    boolean deleteUser(long id);
}
```
- Service Implementation
```sh
package org.smart4j.sample.service.impl;

import java.util.List;
import java.util.Map;
import org.smart4j.framework.orm.DataSet;
import org.smart4j.framework.tx.annotation.Service;
import org.smart4j.framework.tx.annotation.Transaction;
import org.smart4j.sample.entity.User;
import org.smart4j.sample.service.UserService;

@Service
public class UserServiceImpl implements UserService {
    @Override
    public List<User> findUserList() {
        return DataSet.selectList(User.class);
    }
    @Override
    public User findUser(long id) {
        return DataSet.select(User.class, "id = ?", id);
    }
    @Override
    @Transaction
    public boolean saveUser(Map<String, Object> fieldMap) {
        return DataSet.insert(User.class, fieldMap);
    }
    @Override
    @Transaction
    public boolean updateUser(long id, Map<String, Object> fieldMap) {
        return DataSet.update(User.class, fieldMap, "id = ?", id);
    }
    @Override
    @Transaction
    public boolean deleteUser(long id) {
        return DataSet.delete(User.class, "id = ?", id);
    }
}
```
#####6.Write Action Class
```sh
package org.smart4j.sample.action;

import java.util.List;
import java.util.Map;
import org.smart4j.framework.ioc.annotation.Inject;
import org.smart4j.framework.mvc.DataContext;
import org.smart4j.framework.mvc.annotation.Action;
import org.smart4j.framework.mvc.annotation.Request;
import org.smart4j.framework.mvc.bean.Params;
import org.smart4j.framework.mvc.bean.Result;
import org.smart4j.framework.mvc.bean.View;
import org.smart4j.sample.entity.User;
import org.smart4j.sample.service.UserService;

@Action
public class UserAction {
    @Inject
    private UserService userService;
    @Request.Get("/users")
    public View index() {
        List<User> userList = userService.findUserList();
        DataContext.Request.put("userList", userList);
        return new View("user.jsp");
    }
    @Request.Get("/user")
    public View create() {
        return new View("user_create.jsp");
    }
    @Request.Post("/user")
    public Result save(Params params) {
        Map<String, Object> fieldMap = params.getFieldMap();
        boolean result = userService.saveUser(fieldMap);
        return new Result(result);
    }
    @Request.Get("/user/{id}")
    public View edit(long id) {
        User user = userService.findUser(id);
        DataContext.Request.put("user", user);
        return new View("user_edit.jsp");
    }
    @Request.Put("/user/{id}")
    public Result update(long id, Params params) {
        Map<String, Object> fieldMap = params.getFieldMap();
        boolean result = userService.updateUser(id, fieldMap);
        return new Result(result);
    }
    @Request.Delete("/user/{id}")
    public Result delete(long id) {
        boolean result = userService.deleteUser(id);
        return new Result(result);
    }
}
```
#####7.Write View
在 Action 中使用了 JSP 作为视图展现技术，需要编写以下 JSP 文件
In Action Class, use JSPh as technic to show the view, we need to write the JSP file as follows:
- user.jsp
- user_list.jsp
- user_create.jsp
- user_edit.jsp      

    
<a href="/smart-framework.md"> Main page </a> |<a href="/chapter/chapter2-preparation.md">  Previous page </a>  

