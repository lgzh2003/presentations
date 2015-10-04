### Chapter 2: Preparation and deployment  
<a href="/smart-framework.md"> Main page </a> |<a href="/chapter/chapter1-introduce.md">  Previous chapter </a> |<a href="/chapter/chapter3-get-started.md">  Next chapter</a>      

##### 1.Maven Installment

- Maven Download: Download Maven release from <a href="http://maven.apache.org/download.cgi">Apache official website</a>, and extract to prefered catalog.
- Environment variable configuration: build new varable：M2_HOME, variate-value：F:\maven\apache-maven-3.0.3，add the variate-value to the tail of the path：;%M2_HOME%\bin;
- Checj configuration: Run cmd, type command 'mvn -v'. If there is no error message that means maven has been installed.      

![extract](/images/mavn1.png)         

- Change warehouse location:       

![location](/images/mavn2.png)       


##### 2.m2e plugin

- m2e plugin Installment: Download m2e release from <a href="http://www.eclipse.org/m2e/">eclipse.org</a>

- m2e plugin configuration: cd into Window>Preferences>Maven>Installations, remove the inbuilt maven, and click add button and choose the apache-maven.     

![m2e](/images/m2e1.png)       

- m2e plugin integration: Integrate Maven into Eclipse, and make it visualized.       

##### 3.Set up Maven Webapp

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
<a href="/smart-framework.md"> Main page </a> |<a href="/chapter/chapter1-introduce.md">  Previous chapter </a> |<a href="/chapter/chapter3-get-started.md">  Next chapter</a>  





