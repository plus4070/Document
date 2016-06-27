### xml에 빈으로 등록

```xml
 <bean id="apiServerUrlProperties" class="org.springframework.beans.factory.config.PropertiesFactoryBean">
	<property name="locations">
		<list>
			<value>classpath:properties/apiServerUrl.properties</value>
		</list>
	</property>
</bean>
```

### 사용

```java
@Component
public class ApiServerUrlUtil {

  @Resource(name="apiServerUrlProperties")
  private Properties apiServerUrlProperties;
  
  public String getUrl(String key) {
  	return apiServerUrlProperties.getProperty(key);
  }
}
```
