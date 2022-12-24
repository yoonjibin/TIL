# 서블릿(Servlet)
## 서블릿이란?
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbc76R9%2Fbtq7EeRfHx3%2FrBY8OFkMbUZH8cI84O2wu0%2Fimg.png" width="45%"><br>
서블릿이란 Dynamic Web Page를 만들때 사용되는 자반 기반의 웹 애플리케이션 프로그래밍 기술입니다. 웹을 만들때 존재하는 다양한 요청(Request)과 응답(Response)의 흐름을 간단한 메서드 호출만으로 체계적으로 다룰 수 있게 해주는 기술입니다.<br><br>
## 서블릿의 주요특징
* 클라이언트의 요청에 대해 동적으로 작동하는 웹 애플리케이션 컴포넌트이다.
* 기존 정적 웹 프로그램의 문제점을 보완하여 동적인 여러 가지 기능을 제공한다.
* JAVA 스레드를 통해 동작한다.
* MVC 패턴에서의 컨트롤러로 이용된다.
* 컨테이너에서 실행
* 보안 기능을 적용하기 쉽다.
* UDP보다 속도가 느리다.
## 서블릿의 동작과정
<img src="https://velog.velcdn.com/images%2Ffalling_star3%2Fpost%2F4fabf50a-d3d7-4391-8eb5-0cb436379d71%2Fimage.png" width="45%"> <br><br>
클라이언트가 웹 서버에 요청하면 웹 서버는 톰캣과 같은 WAS에 위임한다. WAS는 각 요청에 해당하는 서블릿을 실행한다. 그리고 서블릿은 요청에 대한 기능을 수행한 후 결과를 반환하여 클라이언트에 전송한다.
### 동작과정
```
1. 클라이언트 요청
2. HttpServletRequest, HttpServletRepsone 객체 생성
3. Web.xml이 어느 서블릿에 대한 요청할 것인지 탐색
4. 해당 서비스에서 service()호출
5. doGet() 또는 doPost() 호출
6. 동적 페이지 생성 후 ServeletRepsonse 객체에 응답 전송
7. HttpServletRequest, HttpServletResponse 객체 소멸
```
## 서블릿 형식
``` java
public class myServlet extends HttpServlet{
    @Override
    public void init(ServletConfig config) throws ServletException {
       ...
    }
    
    @Override
    public void destroy() {
        System.out.println("destroy method 호출!");
    }
    
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException
    {
        ...
    }
    
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException
    {
        ...
    }
}
```
## 서블릿의 생명주기
> 서블릿도 자바 클래스이므로 실행하면 초기화부터 서비스 수행 후 소멸하기까지의 과정을 거친다. 이 과정을 서블릿의 생명주기라고 한다. 
>각 단계마다 호출되어 기능을 수행하는 콜백 메서드를 서블릿 생명주기가 메서드라고 한다.
1. 클라이언트의 요청이 들어오면 컨테이너는 해당 서블릿이 메모리에 있는지 확인하고, 없을 경우 init()메서드를 호출하여 메모리에 적재한다. init()은 처음 한번만 실행되기 때문에, 서블릣의 스레드에서 공통적으로 사용해야 하는 것이 있다면 오버라이딩 하여 구현하면 된다. 실행 중 서블릿이 변경될 경우, 기존 서블릿을 destroy()하고 init()을 통해 새로운 내용을 다시 메모리에 적재한다.
2.  init()이 호출된 후 클라이언트의 요청에 따라서 Service() 메소드를 통해 요청에 응답한 doGet()과 doPost()로 분기된다. 이 때 서블릿 컨테이너가 클라이언트의 요청이 오면 가장 먼저 처리하는 과정으로 생성된 HttpServletRequest과 HttpServletResponse에 의한 request와 response 객체가 제공된다.
3. 컨테이너가 서블릿에 종료 요청을 하면 destroy() 메소드가 호출되는데 마찬가지로 한번만 실행되며, 종료시에 처리해야 하는 작업들은 destroy()메소들을 오버라이딩하여 구현하면 된다.
## 서블릿 생명주기 메서드
---
### 초기화: init()
   *  서블릿 요청 시 맨 처음 한번만 호출된다.
   *  서블릿 생성 시 초기화 작업을 주로 수행한다.
### 작업수행: doGet(),doPost()
   * 서블릿 요청 시 매번 호출한다.
   * 실제로 클라이언트가 요청하는 작업을 수행한다.
### 종료: destroy()
* 서블릿 기능을 수행하고 메모리에서 소멸될때 호출한다.
* 서블릿의 마무리 작업을 주로 수행한다.
  
## 서블릿 컨테이너
> 서블릿 컨테이너란, 구현되어 있는 Servlet 클래스의 규칙에 맞게 서블릿을 담고 관리해주는 컨테이너다. <br>
> 클라이언트에서 요청을 하면 컨테이너는 HttpServletRequest, HttpServletResponse 두 객체를 생성하여 Get,Post 여부에 따라 동적인 페이지를 생성하여 음답을 보낸다.
> 
### -웹서버 와의 통신 지원
서블릿 컨테이너느  서블릿과 웹서버가 손쉽게 통신하도록 해준다.
### -서블릿 생명주기 관리
서블릿 컨테이너는 서블릿 클래스를 로딩하여 인스턴스화 하고, 초기화 메소드를 호출하고, 요청이 들어오면 적절한 메서드를 호출한다. 또한 수명이 다한 서블릿을 적절하게 가비지 콜렉터를 호출하여 필요없는 자원 낭비를 막는다.
### -멀티쓰레드 지원
서블릿 컨테이터는 Request가 올 때마다 새로운 스레드를 생성하는데, HTTP 서비스 메소드를 실행하고 나면, 쓰레드는 자동으로 죽게 된다. 원래는 스레드를 관리해야되지만 서버가 다중 스레드 생성 및 운영해주기 때문에 스레드에 안정성에 대해 걱정하지 않아도 된다.
### -선언적인 보안 관리
서블릿 컨테이너를 사용하면 개발자는 보안에 관한 서술적인 내용을 서블릿 또는 자바 클래스에 구현해 놓지 않아도 된다. 일반적으로 보안관리 XML배포 서술자에다가 기록하므로, 보안에 대해 수정할 일이 생겨도 자바 소스 코드를 수정하여 다시 컴파일 하지 않아도 보안관리가 가능하다.
