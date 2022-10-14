# 스프링의 실행순서
1. 톰캣이 ? web.xml 파일을 로딩하여 Servlet Container 실행
2. 서블릿 컨테이터는 web.xml에 설정된 ContextLoaderListener 객체를  Pre-load한다.
3. 그리고 이때 ContextLoaderListenr객체는 applicationContext.xml 파일을 로딩하여` 스프링 컨테이너`를 구동한다. 이는 business Layer 로서 service 요청이 들어오기 전 필요한 bean들을 세팅해놓는 작업.
4. 서비스 요청이 들어오면(클라이언트로 부터의 request) 서블릿 컨테이너는 DispatcherServlet을 생성
5. DispatcherServlet은 presentaion-layer.xml 파일을 로딩하여 두번째 스프링 컨테이너를 구동한다.

결과적으로 두개의 XmlWebApplicationContext를 생성하는데, 각각의 기능과 역할이 상이하다.




## ContextLoaderListner
서블릿 컨테이너가 web 파일을 읽어서 구동될 때, ContextLoaderListener가 자동으로 메모리에 생성된다. 즉, ContextLoaderListener 클라이언트의 요청이 없어도 preload되는 객체이다.
- RootContext를 생성
- 