# sales_list
# 회원 매출 조회
# 결과화면 
![x](https://user-images.githubusercontent.com/104752580/195244463-5e8dd02b-ef51-469f-a428-619d8aaa73bc.JPG)
# 코드
```jsp
<%@ page import="java.sql.*" %>
<%@ page import="DB.DBConnect" %>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
String sql = "select my.custno,mb.custname, "
        + "case when grade='A' then 'VIP' when grade='B' then '일반' else '직원' end grade" + ",sum(my.price)"
        + "from member_tbl_02 mb,MONEY_TBL_02 my " + "where my.custno = mb.custno and my.price is not null "
        + "group by my.custno,mb.custname,grade " + "order by sum(my.price) desc";
	
	Connection conn = DBConnect.getConnection();
    PreparedStatement pstmt = conn.prepareStatement(sql);
    ResultSet rs  = pstmt.executeQuery();
    
    
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원매출 조회</title>
<link rel="stylesheet" href="css/style.css?abc">
</head>
<body>
<header>
	  <jsp:include page="layout/header.jsp"></jsp:include>
 </header>

 <nav>
   	 <jsp:include page="layout/nav.jsp"></jsp:include>
 </nav>
		
 <section class = "section">
<h2>회원 매출 조회</h2>

<table class="table_line">

<tr> 
<th>회원번호</th>
<th>회원성명</th>
<th>고객등급</th>
<th>매출</th>
</tr>

<% 
int i = 0;
while(rs.next()) { 
%>
<tr> 
<td> <%= rs.getString(1) %> </td>
<td>  <%= rs.getString(2) %> </td>
<td>  <%= rs.getString(3) %> </td>
<td> <%= rs.getString(4) %>  </td>
</tr>
<%
i += Integer.parseInt(rs.getString(4));
%><%}
%>

<tr class = "center">
<td colspan="3">
총합
</td>
<td>
<%= i %>
</td>
</tr>
</table>


 </section>
		
<footer>
	<jsp:include page="layout/footer.jsp"></jsp:include>
</footer>

</body>
</html>
```
### head 태그에 DB을 연결 시켜준 후 sql 문에서 select를 가져와서 넣어줍니다. body 태그 안에서는 테이블 안에서 변수 i를 선언하고 String 형태로 받아온 매개변수에 값을 while문으로 반복시켜주고 i 값에 반복시켜준 매개변수에 값의 합계를 넣어주면서 값이 바뀌어도 합계도 같이 바뀔 수 있게 합니다. 마지막으로 총합을 i값을 넣어줌으로써 매출액에 합계가 나옵니다.
