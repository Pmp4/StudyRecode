# π Connection Pool (μ»€λ₯μ ν) π
DataBaseμ μ°κ²°νκΈ° μν μ»€λ₯μ κ°μ²΄λ μλ‘ λ§λ€μ΄μ§ λ λ§μ μμ€ν μμμ μκ΅¬νλ€.

λ°μ΄ν°λ² μ΄μ€μ μ°κ²°νκΈ° μν μ»€λ₯μ κ°μ²΄λ μλ‘ λ§λ€μ΄μ§ λ λ§μ μμ€ν μμμ΄ μκ΅¬λλ€.

κ°μ²΄λ λ©λͺ¨λ¦¬μ μ μ¬κ° λλλ°, λ©λͺ¨λ¦¬μ κ°μ²΄λ₯Ό ν λΉ ν  μλ¦¬λ₯Ό λ§λ€κ³ , κ°μ²΄κ° μ¬μ©ν  μ¬λ¬ μμλ€μ λν μ΄κΈ°ν μμ, κ°μ²΄κ° νμ μκ² λλ©΄ κ°μ²΄λ₯Ό κ±°λμ΄ λ€ μ¬μΌ νλ μμ λ±μ΄ μκ΅¬λμ΄μ κ°μ²΄μ μμ± μμμ λ§ μ λΉμ©μ μκ΅¬ν¨

λ°μ΄ν°λ² μ΄μ€ μ»€λ₯μμ λ°μ΄ν°λ² μ΄μ€μ ν λ² μ°κ²°νκΈ° μν μμ => λ§€λ² μλ‘μ΄ λ°μ΄ν°λ² μ΄μ€ μ°κ²°μ λν μμ²­μ΄ λ€μ΄μ¬ λλ§λ€ μ΄λ¬ν μμλ€μ μνν΄μΌ νλ€λ©΄ λ§μ λΆλ΄μ΄λλ€.


## μ¬μ©λ°©λ²
1. μ§μ  μμ±νμ¬ μ¬μ©
2. μ»¨νμ΄λλ, WASμ°¨μμμ μ κ³΅νλ μ»€λ₯μ νμ μ¬μ© (DBCP)

### μ§μ  μμ±νμ¬ μ¬μ©
```java
//μ±κΈν€ ν¨ν΄ μ μ©(νλμ μΈμ€ν΄μ€)
public class ConnectionPoolMgr {
	private String url,user,pwd;
	private HashMap<Connection, Boolean> hmap;	//Mapμ Connectionμ μ μ₯νλ€
	private int increment; //μ¦κ°μΉ
	
	private static ConnectionPoolMgr instance;
	
	//μμ±μ
	private ConnectionPoolMgr(){
		increment=5;	//5λ§νΌμ© μ¦κ°
		hmap=new HashMap<Connection,Boolean>(10);	
		Connection con=null;	
		
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			System.out.println("λλΌμ΄λ² λ‘λ© μ±κ³΅!");
			url="jdbc:oracle:thin:@localhost:1521:xe";	
			user="herb"; 	
			pwd="herb123";
			
			//μ»€λ₯μ κ°μ²΄λ₯Ό λ―Έλ¦¬ μμ±ν΄ λκΈ° - 10κ°
			for(int i=0;i<10;i++){	
				con = DriverManager.getConnection(url,user,pwd);		
				//ν΄μλ§΅μ keyμ μ»€λ₯μ μ μ₯
				
				hmap.put(con, Boolean.FALSE);		
				//ν΄μλ§΅μ valueμ true, false μ μ₯, false - μ¬λ μ»€λ₯μμ΄λΌλ νμ	
			}//for
			
			System.out.println("ConnectionPool μμ±!");
		}catch (ClassNotFoundException e) {			
			e.printStackTrace();
			System.out.println("Class Not Found!"); 
		}catch (SQLException e) {			
			e.printStackTrace();
			System.out.println("sql μμΈλ°μ!"); 
		}
	}//μμ±μ
  
  //static λ©μλ
	public static ConnectionPoolMgr getInstance() {
		if(instance == null) {
			instance = new ConnectionPoolMgr1();
		}
		
		return instance;
	}
		
	public synchronized Connection getConnection() //jsp - μμ²­μ Threadλ‘ μ²λ¦¬
			throws SQLException{
		Iterator<Connection> iterKeys=hmap.keySet().iterator();	
		Connection con=null;	
		while(iterKeys.hasNext() ){ //hmapμ keyκ° μλ λμ λ°λ³΅	
			con=iterKeys.next();//keyκ°	
			Boolean b=hmap.get(con);//valueκ°	
			//λ§μ½ μ¬κ³ μλ μ»¨λ₯μμ΄λΌλ©΄ μΌνλ μ»¨λ₯μμΌλ‘ νμν΄μ£Όκ³  λ°ννλ€.	
			if(b==Boolean.FALSE){	
				hmap.put(con, Boolean.TRUE);//μΌνλ€κ³  νμ		
				return con; //μΌνλ¬ λκ°	
			}//if	
		}//while
		
		//μ¬κ³  μλ μ»¨λ₯μμ΄ μμΌλ©΄ μΌν  Connectionμ 5κ° μ¦κ°μν¨λ€	
		for(int i=0;i<increment;i++){	
			Connection con2=DriverManager.getConnection(url,user,pwd);	
			hmap.put(con2, Boolean.FALSE);	
		}//for
		
		return getConnection();//μ¬κ·νΈμΆ
	}
	
	//μ»€λ₯μμ μ¬μ©νκ³  λ ν λ€μ λλλ €μ£Όλ λ©μλ	
	public void returnConnection(Connection returnCon){
		Iterator<Connection> iterKeys=hmap.keySet().iterator();	
		Connection con=null;	
		while(iterKeys.hasNext() ){	
			con=iterKeys.next();		
			if(con==returnCon){	//conμ μ£Όμκ°μ΄ μΌμΉνλ©΄
				hmap.put(con, Boolean.FALSE);  //μ¬λ μ»€λ₯μμΌλ‘ νμ	
				break;
			}//if
		}//while
		
		try{	
			removeConnection(); //μ¬κ³ μλ μ»€λ₯μ 10κ°λ₯Ό μ μ§ν΄μ£Όλ λ©μλ	
		}catch(SQLException e){	
			e.printStackTrace();	
			System.out.println("sqlerror:" + e.getMessage());
		}
	}
	
	//Connection 10κ°λ§ μ μ§ν΄μ£Όλ λ©μλ
	public void removeConnection() throws SQLException{
		Connection con=null;
		Iterator<Connection> iterKeys=hmap.keySet().iterator();
		int count=0;//falseμΈ μ»€λ₯μ κ°μ
		while(iterKeys.hasNext() ){ 	
			con=iterKeys.next();	
			Boolean b=hmap.get(con);
			boolean b_pre=b.booleanValue();
			if(!b_pre){//μ¬κ³ μλ μ»€λ₯μ κ°μ μΈκΈ° - falseμΈ κ²½μ°
				count++;
				if(count>10){ //μ¬κ³  μλ μ»€λ₯μμ΄ 10κ°κ° λμ΄κ°λ©΄
					//ν΄μλ§΅μμ μ­μ 
					hmap.remove(con);
					con.close();
				}
			}//if
		}//while
	}
	
	//λͺ¨λ  μ»€λ₯μ closeνλ λ©μλ
	public void closeAll() throws SQLException{
		Iterator<Connection> iterKeys=hmap.keySet().iterator();	
		Connection con=null;	
		while(iterKeys.hasNext() ){ 	
			con=iterKeys.next();	
			con.close();
		}//while
	}
	
	
	//μμν΄μ νλ λ©μλ
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

### μ»¨νμ΄λλ, WASμ°¨μμμ μ κ³΅νλ μ»€λ₯μ νμ μ¬μ© (DBCP)
#### DBCP(Database Connection Pool) μ€μ 
1. Tomcatμ lib ν΄λ λ΄ ```tomcat-dbcp.jar``` νμΌ μ¬μ©(μλ)
2. conf/server.xml μμ 
```xml
<Context docBase="[μ€μ  μΉ νλ‘μ νΈ]" path="/[λ§΅νν  μΉ νλ‘μ νΈλͺ]" reloadable="true" source="org.eclipse.jst.jee.server:[μΉ νλ‘μ νΈ]">
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

#### ConnectionPoolMgr Class μμ±
```java
public class ConnectionPoolMgr {
	private DataSource ds;
	//DataSource - ν°μΊ£μ΄ κ΅¬νν μ»€λ₯μν κ°μ²΄
	
	public ConnectionPoolMgr() {
		Context ctx;
		
		try {
			ctx = new InitialContext();
			ds = (DataSource) ctx.lookup("java:comp/env/jdbc/oracledb");
			//JNDI(java Naming Directory Interface)
			//XMLκ³Ό κ°μ μΈλΆ μμμ ν΅ν΄ κ°μ²΄μ λ νΌλ°μ€λ₯Ό μ»μ΄μ€λ κΈ°λ² μ΄λ―Έ μ¬λΌμ¨ κ°μ²΄λ₯Ό μ΄λ¦μ ν΅ν΄ κ²μν΄μ μ°Ύμλ΄λ κ²
			//lookup()μ΄λΌλ λ©μλλ₯Ό μ΄μ©
			
			//java:comp/envμ νμ μ»¨νμ€νΈ μ€μ jdbc/oracledb λΌλ ν­λͺ©μ λ¦¬μμ€λ₯Ό κ°μ Έμ΄ μ΄ λ¦¬μμ€μ νμμ DataSource νμμ΄λ―λ‘ νλ³ν
			
			System.out.println("DataSource = " + ds);
		} catch(NamingException e) {
			System.out.println("DataSource μμ± μ€ν¨");
			e.printStackTrace();
		}
	}
	
	public Connection getConnection() throws SQLException {
		Connection con = ds.getConnection();
		System.out.println("DBμ°κ²° μ¬λΆ con = " + con);
		
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
- ```InitialContext``` κ°μ²΄λ₯Ό μμ±ν΄μ ```(Context)ctx.lookup("java:/comp/env")``` ββ μμ κΈ°μ λ μ΄λ¦μ lookup() λ©μλλ₯Ό μ΄μ©ν΄μ μ°Ύλ λΆλΆμ
- λ€μ βjava:/comp/envβ μ΄λ¦μΌλ‘ μ°ΎμλΈ ```Context``` κ°μ²΄λ₯Ό κ°μ§κ³  ```(DataSource)ctx.lookup(βjdbc/oracledbβ);``` lookup() λ©μλλ₯Ό μ¬μ© ν΄μ βjdbc/oracledbβ μ΄λ¦μ κ°μ§κ³  κ°μ²΄λ₯Ό λ¦¬ν΄ λ°μμ ```DataSource``` κ°μ²΄ νμμΌλ‘ ν λ³νλ¨
- μ΄λ νκ°μ²΄λ μ§μ°Ύμμμλλ‘ν΄μΌνκΈ°λλ¬Έμμ€μ ν λΉλκ°μ²΄λ€ μ΄ Object νμμΌλ‘ κ°μΈμ Έ μμΌλ―λ‘ μ¬μ©μ μνλ νμμΌλ‘ ν λ³ν μ ν΄μΌ ν¨
- ```DataSource``` κ°μ²΄μ getConnection() λ©μλλ₯Ό κ°μ§κ³  μ»€λ₯μ κ°μ²΄λ₯Ό μ»μ΄λ
