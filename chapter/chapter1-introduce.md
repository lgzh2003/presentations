### Chapt 1: Introduce      
<a href="/smart-framework.md">Main page </a> |<a href="/chapter/chapter2-preparation.md">  Next chapter</a>      

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
            
##### What is Special
- Mart framework of the overall architecture mimics the Struts1 in Action component design mimics the Struts2. With Struts1, the SMART framework there are the ActionServlet controller, Action components, Forwarder components, Convert components, component calls the process also imitate Struts1. Action components are designed to be multi-case model, and Smart framework decoupling, developers can choose to inherit, implement the interface, custom three ways to define the Action component. Convert component in the initialization Struts1 imitate, imitate in the configuration Struts2.                        
                                 
- Smart frame profile data package to imitate the Spring example the configuration file <Action /> in the data will be Java ActionDefine package, and so on.                                   
                                                                                  
- Exception capture mechanism in the framework of the Samrt also mimic the spring, will be converted into a run-time exception, re-throw.                   
                  
- At present, Smart framework MVC trunk, compared with Struts1, not validate, compared with Struts2 interceptor, plus late. Smart framework, profile configuration is almost Struts1.
       
            
<a href="/smart-framework.md">Main page </a> |<a href="/chapter/chapter2-preparation.md">  Next chapter</a>    
