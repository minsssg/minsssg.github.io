---
title: Jdbc Transaction
excerpt: "Jdbc Transaction 관리 방법"
author_profile: true
categories:
  - Java
  - Jdbc
tags: [Java, Jdbc]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-06-12T21:00-22:00
header:
  teaser: "/assets/images/jdbc-transaction/jdbc.jpg"
---

# 1. Jdbc transaction 예제 테스트

 `jdbc`에서 `transaction`관리를 어떻게 하는지 확인하는 과정을 거쳤다. 현재 사용하고 있는 데이터베이스(DB)는 MySQL을 사용하고 있다. MySQL 기본적으로 `auto-commit`모드로 되어 있는데, 이는 개별적인 `SQL`문을 하나의 트랜잭션으로 취급한다. 하지만, 우리가 프로그램을 개발하다보면, 논리적 단위를 하나의 트랜잭션으로 다룬다.

> 위키피디아 참조.
>
> 트랜잭션은 논리적 작업 단위(LUW, Logical Units of Work)이다.

# 2.시나리오

* 게시물을 저장할 때, 파일 메타 데이터도 저장한다.
* 게시물 업로드 시, 게시물 데이터 저장과 파일 데이터 저장을 **트랜잭션**으로 관리한다.
* 관련 테이블은 게시물(bbs), 파일(file) 테이블이다.

**테스트 환경**

* Java 11
* MySQL 8

**BbsDAO.java**

```java
/**
 *	Jdbc transaction 테스트 해보기
 */
public class BbsDAO {
    
    private static final FileDAO fileDAO = new FileDAO();
	/**
	 *  BbsDAO write 메서드
	 */
	public int write(Bbs bbs, File file) {
		String query = "INSERT INTO BBS(bbsTitle, userID, userName, bbsDate, bbsContent, latitude, longitude) VALUES (?, ?, ?, ?, ?, ?, ?)";
		
		int result = -1;
		
		try (Connection con = getConnection();
			 PreparedStatement pstmt = con.prepareStatement(query, PreparedStatement.RETURN_GENERATED_KEYS)) {
			con.setAutoCommit(false); // off the auto-commit mode
			
			pstmt.setString(1, bbs.getTitle());
			pstmt.setString(2, bbs.getUserId());
			pstmt.setString(3, bbs.getUserName());
			pstmt.setString(4, bbs.getBbsDate());
			pstmt.setString(5, bbs.getContent());
			pstmt.setDouble(6, bbs.getLatitude());
			pstmt.setDouble(7, bbs.getLongitude());
			
			pstmt.executeUpdate(); // 게시판 업데이트!!
			
			ResultSet rs = pstmt.getGeneratedKeys();

			long generatedKey = -1L;
			
			if (rs.next()) {
				generatedKey = rs.getLong(1);
			}
			
			System.out.println("Gnerated Key = " + generatedKey);
			
			if (generatedKey == -1L) throw new SQLException();
			
			// transaction 연결을 위해 Connection 객체를 넘겨줌.
			result = fileDAO.upload(con, file); // 파일 저장 시도.
			
			con.commit(); // commit을 하지 않으면 업데이트가 안됨.
			
		} catch (SQLException e) {
			e.printStackTrace();
		}
	
		return result;
	}
}
```

**FileDAO.java**

```java
public class FileDAO {
	
	private final static UserDAO userDAO = new UserDAO();
	private final static BbsDAO bbsDAO = new BbsDAO();

	public int upload(Connection con, File file) throws SQLException { // Connection 객체를 넘겨 받음.
		
		if (file == null) throw new SQLException("File 객체가 null 입니다.");
		
		if (userDAO.findById(file.getUserId()) == null) throw new SQLException("파일의 userID가 누락되었습니다.");
		if (bbsDAO.findById(file.getBbsId()) == null) throw new SQLException("파일의 게시판 ID가  누락되었습니다.");
		
		String query = "INSERT INTO FILE VALUES(?, ?, ?, ?, ?, ?)";
		
		PreparedStatement pstmt = con.prepareStatement(query);
	
		pstmt.setString(1, file.getUserId());
		pstmt.setLong(2, file.getBbsId());
		pstmt.setString(3, file.getFileName());
		pstmt.setString(4, file.getFileRealName());
		pstmt.setDouble(5, file.getLatitude());
		pstmt.setDouble(6, file.getLongitude());
		
		return pstmt.executeUpdate();
	}
}
```

**BbsDAOTest.java**

```java
public class BbsDAOTest {
    @Test
	@DisplayName("Bbs 생성 시, 이미지 file 생성 예제")
	void testCreateBbs() {
		
		Bbs bbs = new Bbs();
		File file = null;
		
		bbs.setTitle("테스트 게시물 생성");
		bbs.setUserId("silverlight_me");
		bbs.setUserName("김은희");
		bbs.setBbsDate(CurrentLocalDateTime.now());
		bbs.setContent("테스트 내용입니다. commit 하지 않을 떄.");
		bbs.setLatitude(0);
		bbs.setLongitude(0);
		
		int result = bbsDAO.write(bbs, file);
		System.out.println("create result = " + result);
	}
}
```

**bbs 테이블**

![bbs-table-definition](\assets\images\jdbc-transaction\bbs-table-definition.png)

MySQL에서 auto-commit이 확인해보면, default로 true가 되어 있다.

> MySQL에서 auto-commit 확인하는 방법, `SELECT @@AUTOCOMMIT` 명령어 조회 시, 1이면 auto-commit 모드가 활성화되어 있다.

# 3. 실행

먼저, auto-commit 모드 일 때, `BbsDAOTest.java`를 실행해보자.

> 즉, `con.setAutoCommit(false)`코드를 제외하고 실행.

**auto-commit** 모드 일 때.
![before-data](\assets\images\jdbc-transaction\before-data.png)

![update-content-error](\assets\images\jdbc-transaction\update-content-error.png)

게시물 생성은 성공했지만, 파일 데이터는 `null`을 입력하여 `insert` 쿼리가 실패했다. 현재 `auto-commit`모드가 `true`이기 때문에 게시물은 생성되었지만 파일이 `SQLException`이 발생했다. `setAutoCommit(false)`를 통해 트랜잭션 관리를 해보자.

**auto-commit 모드 false**

![set-auto-commit-true](\assets\images\jdbc-transaction\set-auto-commit-true.png)

똑같이 게시물 생성을 성공해서 generated Key 가 **13** 이란 값을 리턴했지만, 데이터 베이스에서 게시물 아이디가 **13**인 데이터가 생성되지 않은 것을 볼 수 있다.

![no-data](\assets\images\jdbc-transaction\no-data.png)

이를 통해 트랜잭션을 유지할 수 있다.

### 주의

`con.setAutoCommit(false)`는 커넥션이 종료될 때까지만 유효하다. 새로 `Connection` 객체를 생성하면 `default`로 auto-commit모드가 된다. 따라서 논리적으로 트랜잭션을 유지하려면 생성한 커넥션이 종료되기 전에 비즈니스 로직을 작성하면 된다.

# 4. 정리

트랜잭션의 특성을 이용해 Jdbc만 사용해서 트랜잭션을 관리해보았다. 물론 스프링 프레임워크에선 `@Tranasctional` 애노테이션을 사용해 트랜잭션관리를 할 수 있지만, jdbc만 사용했을 때, 어떤 방법으로 트랜잭션을 관리하는지 확인해보았다.

# 참고자료

* https://docs.oracle.com/javase/tutorial/jdbc/basics/transactions.html
