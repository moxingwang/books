# 一、Webservice实现

## 实现方式分类
### 1.  spring实现
* bean配置

````
<bean class="org.springframework.remoting.jaxws.SimpleJaxWsServiceExporter">
        <property name="baseAddress" value="http://localhost:8088/"/>
    </bean>
````

* service

````
@Component
@WebService(serviceName="sapPushExpenseWebservice")
public class SapPushExpenseWebservice  {
    private static Logger logger = LoggerFactory.getLogger(SapPushExpenseWebservice.class);
    @WebMethod
    public Result<Object> pushExpense(@WebParam(name="expenseDTOSet") Set<String> expenseDTOSet) {
        logger.info(JSON.toJSONString(expenseDTOSet));
        return null;
    }
}
````

### 2.  spring boot实现
* Cxf boot配置

````
@Configuration
public class SapCxfConfig {

    @Bean
    public ServletRegistrationBean dispatcherServlet() {
        return new ServletRegistrationBean(new CXFServlet(), "/sap/soap/*");
    }
    @Bean(name = Bus.DEFAULT_BUS_ID)
    public SpringBus springBus() {
        return new SpringBus();
    }

    @Bean
    public ISapPushExpenseWebservice sapPushExpenseWebservice() {
        return new SapPushExpenseWebservice();
    }
    @Bean
    public Endpoint endpoint() {
        EndpointImpl endpoint = new EndpointImpl(springBus(), sapPushExpenseWebservice());
        endpoint.publish("/push");
        return endpoint;
    }

}
````

* service实现

````
@WebService
public interface ISapPushExpenseWebservice {
    @WebMethod
    Result<Object> pushExpense(Set<SapExpenseDTO> expenseDTOSet);

}

public class SapPushExpenseWebservice implements ISapPushExpenseWebservice {

    private static Logger logger = LoggerFactory.getLogger(SapPushExpenseWebservice.class);

    @Autowired
    private ISapExpenseService sapExpenseService;

    @Override
    public Result<Object> pushExpense(Set<SapExpenseDTO> expenseDTOSet) {
        logger.info(JSON.toJSONString(expenseDTOSet));
        return sapExpenseService.pushExpense(expenseDTOSet);
    }

}

````

### 3.  dubbo实现

# 二、Webservice调用

## cxf客户端生产代码

* 下载地址

````
http://cxf.apache.org/
````

* 生成代码

````
./wsdl2java -d /Users/moxingwang/Desktop/soap -client http://localhost:8080/sap/soap/user?wsdl
````

## 使用代理调用
* 依赖jar

````
 <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-frontend-jaxws -->
        <dependency>
            <groupId>org.apache.cxf</groupId>
            <artifactId>cxf-rt-frontend-jaxws</artifactId>
            <version>3.1.9</version>
        </dependency>
        <dependency>
            <groupId>org.apache.cxf</groupId>
            <artifactId>cxf-rt-transports-http</artifactId>
            <version>3.1.9</version>
        </dependency>
````

* 调用代码
````
public class Test {

    public static void main(String[] args) {

        JaxWsDynamicClientFactory dcf = JaxWsDynamicClientFactory.newInstance();
        Client client = dcf.createClient("http://192.168.224.182:8088/sapPushExpenseWebservice?wsdl");
        try {
            List<String> list = new ArrayList<>();
            list.add("测试");

            Object[] objects = client.invoke("pushExpense", list);

            System.out.println(JSON.toJSONString(objects[0]));

        }catch (Exception e){
            e.printStackTrace();
        }

    }

}
````