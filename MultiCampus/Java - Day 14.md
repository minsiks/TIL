# 4/26 Java day 14

> 시험문제1 수행 평가 Lotto Game
>
> ​               2  데이터 베이스 연동  

## MySQL Workbench -> JAVA

- Select

  -  ```java  
    ps = con.prepareStatement(sql);
    			ps.setString(1, "id99");
    			
    			// 요청 결과를 확인
    			ResultSet rs = ps.executeQuery();
    			rs.next();
    			
    			String id = rs.getString("id");
    			String pwd = rs.getString("pwd");
    			String name = rs.getString("name");
    			
    			System.out.println(id +""+pwd+""+name);
    ```

    - 값을 가져온다.


-  SelectAll

   -  ```java
      ps = con.prepareStatement(sql);
      			
      			// 요청 결과를 확인
      			rs = ps.executeQuery();
      			
      			while(rs.next()) {
      				String id =rs.getString("id");
      				String pwd =rs.getString("pwd");
      				String name =rs.getString("name");
      				System.out.println(id+""+pwd+""+name);
      			}
      ```

-  `int result = ps.executeUpdate();`

  - 값을 업데이트

```mysql
CREATE TABLE PRODUCT(
	id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
    price INT,
    regdate DATE,
    rate FLOAT
);

INSERT INTO PRODUCT VALUES (NULL, 'pants', 10000 , SYSDATE(), 3.4);

SELECT * FROM PRODUCT;
```

