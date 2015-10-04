### Chapter 3: Demo-part 2
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

protected String entityName(Class<T> entityClass)
	{
		String entityName = entityClass.getSimpleName();
		Entity entity = entityClass.getAnnotation(Entity.class);
		if(entity.name()!=null&&!"".equals(entity.name()))
		{
			entityName = entity.name();
		}
		return entityName;
	}
```
     
          
          
<a href="/smart-framework.md"> Main page </a> <a href="/pages/6create-project.md">| Previous page </a> <a href="/pages/8write-service.md">| Next page</a>       
