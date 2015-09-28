# Smart Framework


### 1.Introduce
##### What is SMART
Smart provides a lightweight Java web framework and some libraries (plugins and modules). The framework includes [IOC](https://en.wikipedia.org/wiki/International_Olympic_Committee), [AOP](https://en.wikipedia.org/wiki/Advanced_oxidation_process), [DAO](http://www.webopedia.com/TERM/D/DAO.html), [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping),[MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) technologies, and the plugins can enhance to the framework and the modules can be reuseful for any other applications. Smart can improve your work efficiency and make your development easy.   
![Smart Framework](http://static.oschina.net/uploads/space/2013/1008/122053_f8sG_223750.png)
- IOC   

IOC (Inversion of control) is a general parent term while DI (Dependency injection) is a subset of IOC. IOC is a concept where the flow of application is inverted. So for example rather than the caller calling the method. Hide Copy Code.
- AOP   

AOP (Aspect-oriented programming) is an approach to programming that allows global properties of a program to determine how it is compiled into an executable program. AOP can be used with object-oriented programming (AOP). An aspect is a subprogram that is associated with a specific property of a program.
- DAO   

DAO (Data access object) is an object that provides an abstract interface to some type of database or other persistence mechanism. By mapping application calls to the persistence layer, 
- ORM   

ORM (Object-relational mapping) is a programming technique for converting data between incompatible type systems in object-oriented programming languages. This creates, in effect, a "virtual object database" that can be used from within the programming language
- MVC   

MVC (Model View Controller) is one of three ASP.NET programming models. MVC is a framework for building web
applications using a MVC design: The Model represents the application core (for instance a list of database records). The View displays the data (the database records).

##### Why is SMART
1.Fully Achieve "front end and back end separation"   

- Clients can use HTML or JSP as a view template
- The server can publish REST services (use REST plugin)
- Client -side data and access to services rendered by AJAX interface    

2.Improve Efficiency   

- Face to small and medium-scale Web-based applications
- New users can easily adapt to the framework
- Core has good customization and plug-ins is easy to expand

### 2.Getting Started
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

### 2.Demo
- Smart Sample：http://git.oschina.net/huangyong/smart-sample
- Smart Bootstrap：http://git.oschina.net/huangyong/smart-bootstrap
- Smart REST Server：http://git.oschina.net/huangyong/smart-rest-server
- Smart REST Client：http://git.oschina.net/huangyong/smart-rest-client



         
         
         
         


Dillinger is a cloud-enabled, mobile-ready, offline-storage, AngularJS powered HTML5 Markdown editor.

  - Type some Markdown on the left
  - See HTML in the right
  - Magic

Markdown is a lightweight markup language based on the formatting conventions that people naturally use in email.  As [John Gruber] writes on the [Markdown site][df1]

> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text you see here is *actually* written in Markdown! To get a feel for Markdown's syntax, type some text into the left window and watch the results in the right.

### Version
3.0.2

### Tech

Dillinger uses a number of open source projects to work properly:

* [AngularJS] - HTML enhanced for web apps!
* [Ace Editor] - awesome web-based text editor
* [Marked] - a super fast port of Markdown to JavaScript
* [Twitter Bootstrap] - great UI boilerplate for modern web apps
* [node.js] - evented I/O for the backend
* [Express] - fast node.js network app framework [@tjholowaychuk]
* [Gulp] - the streaming build system
* [keymaster.js] - awesome keyboard handler lib by [@thomasfuchs]
* [jQuery] - duh

And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

### Installation

You need Gulp installed globally:

```sh
$ npm i -g gulp
```

```sh
$ git clone [git-repo-url] dillinger
$ cd dillinger
$ npm i -d
$ mkdir -p public/files/{md,html,pdf}
$ gulp build --prod
$ NODE_ENV=production node app
```

### Plugins

Dillinger is currently extended with the following plugins

* Dropbox
* Github
* Google Drive
* OneDrivetwo

Readmes, how to use them in your own application can be found here:

* [plugins/dropbox/README.md] [PlDb]
* [plugins/github/README.md] [PlGh]
* [plugins/googledrive/README.md] [PlGd]
* [plugins/onedrive/README.md] [PlOd]

### Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantanously see your updates!

Open your favorite Terminal and run these commands.

First Tab:
```sh
$ node app
```

Second Tab:
```sh
$ gulp watch
```

(optional) Third:
```sh
$ karma start
```

### Todos

 - Write Tests
 - Rethink Github Save
 - Add Code Comments
 - Add Night Mode

License
----

MIT


**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does it's job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [@thomasfuchs]: <http://twitter.com/thomasfuchs>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [marked]: <https://github.com/chjj/marked>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [keymaster.js]: <https://github.com/madrobby/keymaster>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>
   
   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]:  <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>



