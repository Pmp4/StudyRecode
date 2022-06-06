# IntelliJ에서 SpringBoot 프로젝트 생성하기

## 프로젝트 생성
<img width="802" alt="스크린샷 2022-06-04 오후 6 20 48" src="https://user-images.githubusercontent.com/88151484/171993128-2a17d9a4-7984-4467-a980-0812b20a13d0.png"></br>
- IntelliJ 실행
- 새프로젝트
- 제너레이터 'Spring Initializr'

### 설정
- 이름 : 웹 프로젝트, 웹 사이트, Context path, 웹 어플리케이션
- 그룹 : com.'회사명'
- 아티팩트 : '이름'과 비슷한 개념
- 패키지 : com.'회사명'.'웹 프로젝트' => 실제 경로? 같은 개념으로 보면 될 거 같다.
- 패키지 생성 : 나는 Jsp 사용을 위해 War를 선택할 것이다.
</br>

## 종속성(의존성) 기본 설정
<img width="801" alt="스크린샷 2022-06-04 오후 6 28 21" src="https://user-images.githubusercontent.com/88151484/171993428-923e5df5-1551-481f-be4b-00e778bc6bb7.png"></br>
- <b>Spring Web</b> : Spring 프레임워크 관련 종속성 추가?
- <b>Lombok</b> : VO에서 생성자, setter/getter, toString을 자동으로 생성한다.(Annotation ```@Data```)
- <b>Oracle Driver</b> : DB는 Oracle을 사용하므로 DB Driver 추가
- <b>MyBatis Framework</b> : CRUD를 편하게 쓸 수 있도록 한다.

우선 종속성 추가는 기본적으로 위 리스트로 한다.
</br>

## 추가 종속성 설정과 기타설정
<img width="453" alt="스크린샷 2022-06-04 오후 6 39 25" src="https://user-images.githubusercontent.com/88151484/171993805-8d16bfe8-f459-4ba1-b824-b8c8500b0492.png"></br>
(프로젝트 생성 직후 디렉토리)</br>
- <b>[프로젝트명]Application.java</b> : Spring Boot 실행 모듈
- src/main/resources/```static``` : CSS, Fonts, images, plugin, script 등 정적 리소스

### 설정 
#### src/main/resources/<b>```application.properties```</b>
: Spring Boot가 애플리케이션 구동할 때 함께 로드하는 파일로 key, value 형식, WAS 설정, DB 관련 정보 설정
```properties
server.servlet.context-path=/[프로젝트명]
server.port=9091

# reflesh
spring.devtools.livereload.enabled=true

# JSP Path (ViewResolver)
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

# DataBase
#spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver
#spring.datasource.url=jdbc:oracle:thin:@aaa:1521:xe
#spring.datasource.username=~~~
#spring.datasource.password=~~~

#mapper location settings
#mybatis.config-location=classpath:/config/mybatis/oracle/mybatis-config.xml
#mybatis.mapper-locations=classpath:/config/mybatis/mapper/oracle/*.xml
#mybatis.type-aliases-package=com.it.herb


spring.datasource.hikari.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.hikari.jdbc-url=jdbc:oracle:thin:@aaa:1521:xe
spring.datasource.hikari.username=~~~~
spring.datasource.hikari.password=~~~~
spring.datasource.hikari.connection-test-query=SELECT sysdate FROM dual

#MyBatis
mybatis.configuration.map-underscore-to-camel-case=true


#email
spring.mail.host=smtp.gmail.com
spring.mail.port=587
#spring.mail.username=이메일주소
#spring.mail.password=비밀번호
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

#### pom.xml
: Project Object Model
: 프로젝트에 대한 설명 및 종속성 기술
```xml
<dependency>
  <groupId>org.apache.tomcat.embed</groupId>
  <artifactId>tomcat-embed-jasper</artifactId>
  <scope>provided</scope>
</dependency>
		        
<!-- jstl 라이브러리(Spring boot에서의 Jsp 사용) -->
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>jstl</artifactId>
</dependency>
<!-- jstl 라이브러리 -->

<!-- auto reflash -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
</dependency>
<!-- auto reflash -->

<!-- File Upload 관련 -->
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.6</version>
</dependency>
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.3.3</version>
</dependency>
<!-- File Upload 관련 -->
		
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-mail</artifactId>
  <!-- <version>2.0.1.RELEASE</version> -->
</dependency>
```
기존 pom.xml에 위 종속성관련 내용 추가

///추가 (```<bulid></build> 안에 추가```)
```xml
<!--IntelliJ에서의 mapper xml 위치 지정하기 위해 사용-->
<resources>
	<resource>
		<directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
	</resource>
</resources>
```
mapper.xml 위치를 DBConfiguration.java에서 지정해주고 eclipse에서는 mapper 위치를 알맞게 찾아갔는데 이상하게 IntelliJ에서는 계속 못찾았다. 그래서 열심히 구글링 해보니 해당 리소스를 bulid 태그 내 넣어주면 된다고한다..


</br>

## 직접 생성
<img width="428" alt="스크린샷 2022-06-04 오후 7 33 53" src="https://user-images.githubusercontent.com/88151484/171995461-0ec1158b-496e-49c3-880c-2e6a12cb6877.png"></br>
- com.[회사].[프로젝트명].configuration
  - DBConfiguration.java : Data Source 객체 생성(ConnectionPool), sqlSessionFactory 객체 생성(DB Connection과 sql 관련 모든 것)
  - MvcConfiguration.java : FileUpload 관련

- config.mybatis.mapper.oracle : mapper xml 디렉토리
- webapp/WEB-INF/views : view 관련 Jsp

### DBConfiguration
```java
@Configuration
@PropertySource("classpath:/application.properties")
@EnableTransactionManagement
public class DBConfiguration {

	@Autowired
	private ApplicationContext applicationContext;

	@Bean
	@ConfigurationProperties(prefix = "spring.datasource.hikari")
	public HikariConfig hikariConfig() {
		return new HikariConfig();
	}

	@Bean
	public DataSource dataSource() {
		return new HikariDataSource(hikariConfig());
	}

	@Bean
	public SqlSessionFactory sqlSessionFactory() throws Exception {
		SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
		factoryBean.setDataSource(dataSource());
		factoryBean.setMapperLocations(applicationContext.getResources("classpath*:/config/mybatis/mapper/oracle/*.xml"));
		factoryBean.setTypeAliasesPackage("com.[회사].[프로젝트이름]");
		factoryBean.setConfiguration(mybatisConfg());
		return factoryBean.getObject();
	}

	@Bean
	public SqlSessionTemplate sqlSession() throws Exception {
		return new SqlSessionTemplate(sqlSessionFactory());
	}

	@Bean
	@ConfigurationProperties(prefix = "mybatis.configuration")
	public org.apache.ibatis.session.Configuration mybatisConfg() {
		return new org.apache.ibatis.session.Configuration();
	}

	//tx:annotation-driven 설정-@Transactional를 선언하여 트랜잭션 처리를 할 수 있다.
	@Bean
	public PlatformTransactionManager txManager() throws Exception{
		return new DataSourceTransactionManager(dataSource());
	}
}
```
### MvcConfiguration
```java
@Configuration
public class MvcConfiguration implements WebMvcConfigurer{

	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(new LoginInterceptor())
		.addPathPatterns("/shop/cart/*", "/shop/order/*","/member/memberEdit.do","/member/memberOut.do");
		
		registry.addInterceptor(new AdminLoginInterceptor())
		.excludePathPatterns("/admin/login/adminLogin.do")
		.addPathPatterns("/admin/*/*", "/admin/*");
	}

	@Bean
	public CommonsMultipartResolver multipartResolver() {
		CommonsMultipartResolver multipartResolver 
			= new CommonsMultipartResolver();
		multipartResolver.setDefaultEncoding("UTF-8"); // 파일 인코딩 설정
		multipartResolver.setMaxUploadSizePerFile(2 * 1024 * 1024); // 파일당 업로드 크기 제한 (2MB)
		return multipartResolver;
	}
}
```


