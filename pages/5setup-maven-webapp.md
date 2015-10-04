### Chapter 2: Preparation and Deployment-part 3
##### Set up Maven Webapp     
<a href="smart-framework.md"> Main page </a> <a href="/pages/4m2e-plugin.md">| Previous page </a> <a href="/pages/6create-project.md">| Next page</a> 

- Maven webapp：File>New>Maven Project>Next Archetype, choose maven-archetype-webapp,go next,fill in tables，finish      

 ![setup1](/images/setup1.png)           

- Consummate the src catalog：      

![setup2](/images/setup2.png)        

- Add configuration:
```sh
<plugins>
   <plugin>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.0</version>
    <configuration>
     <source>1.6</source>
     <target>1.6</target>
    </configuration>
   </plugin>
</plugins>
```
- Right key click project item, update catalog: Maven>Update Project.Then, in Properties>Project Facets，change Dynamic Web Module into 3.0、Java into 1.6、choose Runtimes: Tomcat7.0      

![setup3](/images/setup3.png)          


- Servlet testing：Bulid HelloMavenWebappServlet based on Servlet3.0, deploy the project on to Tomcat7, and run http://localhost:8080/HelloMaven/helloMavenWebapp,it will shows Get:/helloMavenWebapp
```sh
@WebServlet("/helloMavenWebapp")
public class HelloMavenWebappServlet extends HttpServlet{
 private static final long serialVersionUID = 8963265462953694987L;
 
 @Override
 public void doGet(HttpServletRequest req, HttpServletResponse resp)
   throws ServletException, IOException {
  resp.getWriter().println("Get:/helloMavenWebapp");
 }
  
 @Override
 public void doPost(HttpServletRequest req, HttpServletResponse resp)
   throws ServletException, IOException {
  resp.getWriter().println("Post:/helloMavenWebapp");
 }
}
```
     
          
             
<a href="smart-framework.md"> Main page </a> <a href="/pages/4m2e-plugin.md">| Previous page </a> <a href="/pages/6create-project.md">| Next page</a> 
