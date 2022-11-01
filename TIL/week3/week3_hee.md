## ✔️ JSP

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4afc9bd0-6c82-4b0f-b3da-a8def006b89a/Untitled.png)

- JSP 태그
  
  - HTML 코드에 JAVA언어를 삽입하여 동적 문서를 만들 수 있다.
    
    - 태그 사용
    
    | 태그 종류    | 표현 방법                    | 태그 의미      |
    | -------- | ------------------------ | ---------- |
    | 지시자 태그   | <%@ %>                   | 페이지 속성     |
    | 주석 태그    | <%— —%>                  | 주석         |
    | 선언 태그    | <%! %>                   | 변수, 메소드 선언 |
    | 표현식 태그   | <%= %>                   | 결과값 출력     |
    | 스크립트릿 태그 | <% %>                    | JAVA 코드    |
    | 액션 태그    | jsp:action </jsp:action> | 자바빈 연결     |

- 스크립(scripe)
  
  - 스크립트릿, 선언, 표현식
  - JSP 문서 안에 JAVA 언어를 넣기 위한 방식
  - **스크립트릿(scriptlet)**
    - JSP 페이지에서 JAVA 언어를 사용하기 위한 요소 중 가장 많이 사용되는 요소로 거의 모든 JAVA 코드를 사용할 수 있다.
  
  ```html
  <%
      int i = 0;
          while (true) {
                  i++;
                  out.println("2 * " + i + " = " + (2*i) + "<br>");
  %>
  ==========<br>
  <%
          if (i>=9) break;
  %>
  ```
  
  - **선언(declaration)**
    - JSP 페이지 내에서 사용되는 변수 또는 메소드를 선언할 때 사용
    - 선언된 변수 및 메소드는 전역의 의미로 사용 > 페이지 안에서 어디에서나 사용할 수 있다.
  
  ```html
  <%!
          int i = 10;
          String str = "ABCDE";
  %>
  
  <%!
          public int sum(int a, int b) {
                  return a + b;
          }
  %>
  
  <%
          out.println("i = " + i + "<br>");
          out.println("str = " + str + "<br>");
          out.println("sum = " + sum(1, 5) + "<br>");
  %>
  ```
  
  - **표현식(expression)**
    - JSP 페이지 내에서 사용되는 변수의 값 또는 메소드 호출 결과값을 출력하기 위해 사용
    - 결과값은 String 타입이며 ‘,’를 사용할 수 없다.
  
  ```html
  <%!
          int i = 10;
          String str = "abc";
  
          private int sum(int a, int b){
                  return a+b;
          }
  %>
  
  <%= i %><br>
  <%= str %><br>
  <%= sum(1, 5) %>
  ```
  
  - **지시자**
    
    - JSP 페이지의 전체적인 속성을 지정할 때 사용
    - page : 해당 페이지의 전체적인 속성 지정 (주로 사용되는 언어 지정 및 import문 많이 사용
    
    ```html
    <%@ page import = "java.util.Arrays"%>
    ```
    
    - include : 별도의 페이지를 현재 페이지에 삽입(header와 같이 반복되는 페이지를 삽입한는데에 사용)
    
    ```html
    <%@ include file = "include01.jsp"%>
    ```
    
    - taglib : 태그 라이브러리의 태그 사용
  
  - **주석**
    
    - 실제 프로그램에는 영향이 없고 프로그램 설명들의 목적으로 사용하는 태그
    - JSP 주석
    
    ```html
    <%-- DURLSMS WNTJRDLQSLEK. --%>
    ```

- request 객체의 이해
  
  - request : 웹 브라우저를 통해 서버에 어떤 정보를 요청하는 것
  - 요청 정보는 request 객체가 관리
  
  | request 객체 관련 메소드 | 설명                       |
  | ----------------- | ------------------------ |
  | getContextPath( ) | 웹애플리케이션의 컨텍스트 패스를 얻는다.   |
  | getMethod( )      | get방식과 post방식을 구분할 수 있다. |
  | getSession( )     | 세션 객체를 얻는다.              |
  | getProtocol( )    | 해당 프로토콜을 얻는다.            |
  | getRequestURL( )  | 요청 URL을 얻는다.             |
  | getRequestURI( )  | 요청 URI를 얻는다.             |
  | getQueryString( ) | 쿼리스트링을 얻는다.              |
  
  ```html
  <%
          out.println("서버 : " + request.getServerName() + "<br>");
          out.println("포트 번호 : " + request.getServerPort() + "<br>");
          out.println("요청 방식 : " + request.getMethod() + "<br>");
          out.println("프로토콜 : " + request.getProtocol() + "<br>");
          out.println("URL : " + request.getRequestURL() + "<br>");
          out.println("URI : " + request.getRequestURI() + "<br>");
  %>
  ```

- Parameter 메소드
  
  | Parameter 관련 메소드                | 설명                     |
  | ------------------------------- | ---------------------- |
  | getParameter(String name)       | name에 해당하는 파라미터 값을 구함  |
  | getParameteNames( )             | 모든 파라미터의 이름을 구함        |
  | getParameterValues(String name) | name에 해당하는 파라미터 값들을 구함 |
  
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
      <form action="FormEx" method="post">
          이름: <input type="text" name="name" size="10"><br>
          아이디: <input type="text" name="id" size="10"><br>
          비밀번호: <input type="password" name="pw" size="10"><br>
          취미: <input type="checkbox" name="hobby" value="read">독서
          <input type="checkbox" name="hobby" value="cook">요리
          <input type="checkbox" name="hobby" value="sleep">취침<br>
          과목 : <input type="radio" name="major" value="kor">국어
          <input type="radio" name="major" value="eng">영어
          <input type="radio" name="major" value="math">수학<br>
          <select name="protocol">
              <option value="http">http</option>
              <option value="ftp" selected="selected">ftp</option>
              <option value="smtp">smtp</option>
              <option value="pop">pop</option>
          </select><br>
          <input type="submit" value="전송">
          <input type="reset" value="초기화">
      </form>
  </body>
  </html>
  ```
  
  ```html
  <%@ page import="java.util.Arrays"%>
  <%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC_KR"%>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "<http://www.w3.org/TR/html4/loose.dtd>">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
  <title>Insert title here</title>
  </head>
  <body>
  <%!
          String name, id, pw, major, protocol;
          String[] hobbys;
  %>
  
  <%
          request.setCharacterEncoding("EUC-KR");
  
          name = request.getParameter("name");
          id = request.getParameter("id");
          pw = request.getParameter("pw");
          major = request.getParameter("major");
          protocol = request.getParameter("protocol");
  
          hobbys = request.getParameterValues("hobby");
  %>
  
  이름 : <%= name %><br>
  아이디 : <%= id %><br>
  비밀번호 : <%= pw %><br>
  취미 : <%= Arrays.toString(hobbys) %><br>
  전공 : <%= major %><br>
  프로토콜 : <%= protocol %><br>
  </body>
  </html>
  ```
  
  | Parameter 관련 메소드        | 설명               |
  | ----------------------- | ---------------- |
  | getCharacterEncoding( ) | 응답할 때 문자의 인코딩 형태 |
  | addCookie(Cookie)       | 쿠키 지정            |
  | sendRedirect(URL)       | 지정한 URL로 이동      |
  
  ```html
  <%
          String str = request.getParameter("age");
          age = Integer.parselnt(str);
  
          if (age >=20 ){
              response.sendRedirect("pass.jsp?age=" + age);
          } else {
                  response.sendRedirect("ng.jsp?age=" + age);
          }
  %>
  ```

- 액션 태그
  
  - JSP 페이지 내에서 어떤 동작을 하도록 지시하는 태그
  - 페이지 이동, 페이지 include 등등
  - forward
    - 현재의 페이지에서 다른 특정 페이지로 전환할 때 사용
    - url이 바뀌지는 않고 forwarding page가 달라진다.
  
  ```html
  <jsp:forward page="sub.jsp"/>
  ```
  
  - include
    - 현재 페이지에 다른 페이지를 삽입할 때 사용
  
  ```html
  <jsp:include page="include02.jsp" flush="true"/>
  ```
  
  - param
    - forward 및 include 태그에 데이터 전달을 목적으로 사용되는 태그
    - 이름과 값으로 이루어져 있다.
  
  ```html
  <jsp:forward page="forward_param.jsp">
          <jsp:param name="id" value="abcdef"/>
          <jsp:param name="pw" value="1234"/>
  </jsp:forward>
  ```
  
  ```html
  <%! 
          String id, pw;
  %>
  
  <%
          id = request.getParameter("id");
          pw = request.getParameter("pw");
  %>
  
  아이디 : <%= id %><br>
  비밀번호 : <%= pw %>
  ```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8972085-b114-409d-bac3-38063344c2c4/Untitled.png)

- JSP 동작 원리
  
  - 클라이언트가 웹브라우저로 helloWorld.jsp를 요청하게 되면 JSP 컨테이너가 JSP 파일을 Servlet 파일(.java)로 변환한다.
  - Servlet 파일 (.java)은 컴파일 된 후 클래스 파일(.class)로 변환되고, 요청한 클라이언트한테 html 파일 형태로 응답된다.

- JSP 내부 객체
  
  - 개발자가 객체를 생성하지 않고 바로 사용할 수 있는 객체
  - JSP에서 제공되는 내부객체는 JSP컨테이너에 의해 Servlet으로 변화될 때 자동으로 객체 생성
  
  | 내부 객체  | 종류                     |
  | ------ | ---------------------- |
  | 입출력 객체 | request, response, out |
  | 서블릿 객체 | page, config           |
  | 세션 객체  | session                |
  | 예외 객체  | exception              |
