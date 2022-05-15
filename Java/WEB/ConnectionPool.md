# 🖇 Connection Pool (커넥션 풀) 🖇
DataBase에 연결하기 위한 커넥션 객체는 새로 만들어질 때 많은 시스템 자원을 요구한다.

데이터베이스에 연결하기 위한 커넥션 객체는 새로 만들어질 때 많은 시스템 자원이 요구된다.

객체는 메모리에 적재가 되는데, 메모리에 객체를 할당 할 자리를 만들고, 객체가 사용할 여러 자원들에 대한 초기화 작업, 객체가 필요 없게 되면 객체를 거두어 들 여야 하는 작업 등이 요구되어서 객체의 생성 작업은 많 은 비용을 요구함

데이터베이스 커넥션은 데이터베이스에 한 번 연결하기 위한 작업 => 매번 새로운 데이터베이스 연결에 대한 요청이 들어올 때마다 이러한 작업들을 수행해야 한다면 많은 부담이된다.


## 사용방법
1. 직접 작성하여 사용
2. 컨테이너나, WAS차원에서 제공하는 커넥션 풀을 사용 (DBCP)

### 직접 작성하여 사용
```java
//싱글톤 패턴 적용(하나의 인스턴스)
public class ConnectionPoolMgr {
	private String url,user,pwd;
	private HashMap<Connection, Boolean> hmap;	//Map에 Connection을 저장한다
	private int increment; //증가치
	
	private static ConnectionPoolMgr instance;
	
	//생성자
	private ConnectionPoolMgr(){
		increment=5;	//5만큼씩 증가
		hmap=new HashMap<Connection,Boolean>(10);	
		Connection con=null;	
		
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			System.out.println("드라이버 로딩 성공!");
			url="jdbc:oracle:thin:@localhost:1521:xe";	
			user="herb"; 	
			pwd="herb123";
			
			//커넥션 객체를 미리 생성해 놓기 - 10개
			for(int i=0;i<10;i++){	
				con = DriverManager.getConnection(url,user,pwd);		
				//해시맵의 key에 커넥션 저장
				
				hmap.put(con, Boolean.FALSE);		
				//해시맵의 value에 true, false 저장, false - 쉬는 커넥션이라는 표시	
			}//for
			
			System.out.println("ConnectionPool 생성!");
		}catch (ClassNotFoundException e) {			
			e.printStackTrace();
			System.out.println("Class Not Found!"); 
		}catch (SQLException e) {			
			e.printStackTrace();
			System.out.println("sql 예외발생!"); 
		}
	}//생성자
  
  //static 메서드
	public static ConnectionPoolMgr getInstance() {
		if(instance == null) {
			instance = new ConnectionPoolMgr1();
		}
		
		return instance;
	}
		
	public synchronized Connection getConnection() //jsp - 요청시 Thread로 처리
			throws SQLException{
		Iterator<Connection> iterKeys=hmap.keySet().iterator();	
		Connection con=null;	
		while(iterKeys.hasNext() ){ //hmap에 key가 있는 동안 반복	
			con=iterKeys.next();//key값	
			Boolean b=hmap.get(con);//value값	
			//만약 쉬고있는 컨넥션이라면 일하는 컨넥션으로 표시해주고 반환한다.	
			if(b==Boolean.FALSE){	
				hmap.put(con, Boolean.TRUE);//일한다고 표시		
				return con; //일하러 나감	
			}//if	
		}//while
		
		//쉬고 있는 컨넥션이 없으면 일할 Connection을 5개 증가시킨다	
		for(int i=0;i<increment;i++){	
			Connection con2=DriverManager.getConnection(url,user,pwd);	
			hmap.put(con2, Boolean.FALSE);	
		}//for
		
		return getConnection();//재귀호출
	}
	
	//커넥션을 사용하고 난 후 다시 되돌려주는 메소드	
	public void returnConnection(Connection returnCon){
		Iterator<Connection> iterKeys=hmap.keySet().iterator();	
		Connection con=null;	
		while(iterKeys.hasNext() ){	
			con=iterKeys.next();		
			if(con==returnCon){	//con의 주소값이 일치하면
				hmap.put(con, Boolean.FALSE);  //쉬는 커넥션으로 표시	
				break;
			}//if
		}//while
		
		try{	
			removeConnection(); //쉬고있는 커넥션 10개를 유지해주는 메소드	
		}catch(SQLException e){	
			e.printStackTrace();	
			System.out.println("sqlerror:" + e.getMessage());
		}
	}
	
	//Connection 10개만 유지해주는 메서드
	public void removeConnection() throws SQLException{
		Connection con=null;
		Iterator<Connection> iterKeys=hmap.keySet().iterator();
		int count=0;//false인 커넥션 개수
		while(iterKeys.hasNext() ){ 	
			con=iterKeys.next();	
			Boolean b=hmap.get(con);
			boolean b_pre=b.booleanValue();
			if(!b_pre){//쉬고있는 커넥션 개수 세기 - false인 경우
				count++;
				if(count>10){ //쉬고 있는 커넥션이 10개가 넘어가면
					//해시맵에서 삭제
					hmap.remove(con);
					con.close();
				}
			}//if
		}//while
	}
	
	//모든 커넥션 close하는 메서드
	public void closeAll() throws SQLException{
		Iterator<Connection> iterKeys=hmap.keySet().iterator();	
		Connection con=null;	
		while(iterKeys.hasNext() ){ 	
			con=iterKeys.next();	
			con.close();
		}//while
	}
	
	
	//자원해제하는 메서드
	public void dbClose(PreparedStatement ps,  Connection con) throws SQLException{
		if(ps!=null) ps.close();
		if(con!=null)returnConnection(con);
	}
	
	public void dbClose(ResultSet rs,  PreparedStatement ps,  
			Connection con) throws SQLException{
		if(rs!=null)rs.close();
		if(ps!=null) ps.close();
		if(con!=null)returnConnection(con);				
	}
}//class
```
<br>

### 컨테이너나, WAS차원에서 제공하는 커넥션 풀을 사용 (DBCP)
#### DBCP(Database Connection Pool) 설정
1. Tomcat의 lib 폴더 내 ```tomcat-dbcp.jar``` 파일 사용(자동)
2. conf/server.xml 수정
```xml
<Context docBase="[실제 웹 프로젝트]" path="/[맵핑할 웹 프로젝트명]" reloadable="true" source="org.eclipse.jst.jee.server:[웹 프로젝트]">
	<Resource 
		auth="Container" 
		driverClassName="oracle.jdbc.driver.OracleDriver" 
		maxActive="20" 
		maxWait="-1" 
		name="jdbc/oracledb" 
		username="[DBUser]"
		password="[DBPwd]" 
		type="javax.sql.DataSource" 
		url="jdbc:oracle:thin:@aa:1521:xe"/>
</Context>
```

#### ConnectionPoolMgr Class 생성
```java
public class ConnectionPoolMgr {
	private DataSource ds;
	//DataSource - 톰캣이 구현한 커넥션풀 객체
	
	public ConnectionPoolMgr() {
		Context ctx;
		
		try {
			ctx = new InitialContext();
			ds = (DataSource) ctx.lookup("java:comp/env/jdbc/oracledb");
			//JNDI(java Naming Directory Interface)
			//XML과 같은 외부 자원을 통해 객체의 레퍼런스를 얻어오는 기법 이미 올라온 객체를 이름을 통해 검색해서 찾아내는 것
			//lookup()이라는 메소드를 이용
			
			//java:comp/env의 하위 컨텍스트 중에 jdbc/oracledb 라는 항목의 리소스를 가져옴 이 리소스의 타입은 DataSource 타입이므로 형변환
			
			System.out.println("DataSource = " + ds);
		} catch(NamingException e) {
			System.out.println("DataSource 생성 실패");
			e.printStackTrace();
		}
	}
	
	public Connection getConnection() throws SQLException {
		Connection con = ds.getConnection();
		System.out.println("DB연결 여부 con = " + con);
		
		return con;
	}
	
	public void dbClose(ResultSet rs, PreparedStatement ps, Connection con) 
			throws SQLException {
		if(rs != null) rs.close(); 
		if(ps != null) ps.close(); 
		if(con != null) con.close(); 
	}
	
	public void dbClose(PreparedStatement ps, Connection con) 
			throws SQLException {
		if(ps != null) ps.close(); 
		if(con != null) con.close(); 
	}
}
```
- ```InitialContext``` 객체를 생성해서 ```(Context)ctx.lookup("java:/comp/env")``` “” 안에 기술된 이름을 lookup() 메서드를 이용해서 찾는 부분임
- 다시 “java:/comp/env” 이름으로 찾아낸 ```Context``` 객체를 가지고 ```(DataSource)ctx.lookup(“jdbc/oracledb”);``` lookup() 메서드를 사용 해서 “jdbc/oracledb” 이름을 가지고 객체를 리턴 받아서 ```DataSource``` 객체 타입으로 형 변환됨
- 어떠한객체든지찾을수있도록해야하기때문에실제할당된객체들 이 Object 타입으로 감싸져 있으므로 사용시 원하는 타입으로 형 변환 을 해야 함
- ```DataSource``` 객체의 getConnection() 메서드를 가지고 커넥션 객체를 얻어냄
