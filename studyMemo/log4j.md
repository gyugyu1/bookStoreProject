```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="info" monitorInterval="5">
    <!--공통 속성 설정 -->
    <Properties>
        <Property name="logFileName">log4jFile</Property>
        <Property name="consoleLayout">[%d{yyyy-MM-dd HH:mm:ss}] [%-5p] [%c{1}:%L] - %m%n</Property>
        <Property name="fileLayout">%d [%t] %-5level %c(%M:%L) - %m%n</Property>
    </Properties>

    <!-- Log Appender 설정 -->
    <Appenders>
        <Console name="console" target="SYSTEM_OUT">
            <PatternLayout pattern="${consoleLayout}" />
        </Console>
        <!--ConsoleAppender, RollingFileAppneder -->
        <RollingFile name="file"
                     fileName="logs/${logFileName}.log"
                     filePattern="logs/${logFileName}.%d{yyyy-MM-dd-hh}.log">
            <PatternLayout pattern="${fileLayout}" />
            <Policies>
                <TimeBasedTriggeringPolicy
                        modulate="true"
                        interval="1" /><!-- 시간별 로그 파일 생성-->
            </Policies>
            <DefaultRolloverStrategy max="5" fileIndex="min" > <!-- 롤링 파일 5개 까지 생성 -->
                <Delete basePath="/logs" maxDepth="3">
                    <IfLastModified age="10d" />
                </Delete>
            </DefaultRolloverStrategy>
        </RollingFile>


        <RollingFile name="daily_error"
                     fileName="logs/error/error.log"
                     filePattern="logs/error/error.%d{yyyy-MM-dd}.log">
            <PatternLayout pattern="${fileLayout}" />
            <!--LevelRangeFilter필터를 사용할때에 단일 level의 필터만 허용한다. -->
            <LevelRangeFilter minLevel="WARN" maxLevel="WARN" onMatch="ACCEPT" onMismatch="DENY"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="20 MB" />
            </Policies>
            <DefaultRolloverstrategy>
                <Delete basePath="/logs" maxDepth="3">
                    <IfLastModified age="10d" />
                </Delete>
            </DefaultRolloverstrategy>
        </RollingFile>


    </Appenders>

    <!--TRACE > DEBUG > INFO > WARN > ERROR > FATAL -->
    <!--Log가 찍힐 대상 설정.-->
    <Loggers>
        <!-- 스프링 프레임워크에서 찍는건 level을 info로 설정 -->
        <logger name="org.springframework" level="info" additivity="false" >
            <AppenderRef ref="console" />
            <AppenderRef ref="file" />
        </logger>

        <!-- rolling file에는 debug, console에는 info 분리하여 처리 가능하다. -->
        <logger name="kr.pe.study.logforjava2" level="warn" additivity="true" >
            <AppenderRef ref="daily_error" />
        </logger>

		<!-- ROOT logger-->
        <Root level="info"></Root>
    </Loggers>
</Configuration>
```



# configuration
- 로그 설정의 최상위 요소
- properties, Appenders, Loggers 를 자식으로 가짐
- status 속성은 내부 이벤트에 대한 로그 레벨(log4j를 로딩하면서 생기는 log들 level을 성정)

# properties
Configuration에서 사용할 프로퍼티 (변수)

```xml
    <Properties>
        <Property name="logFileName">log4jFile</Property>
        <Property name="consoleLayout">[%d{yyyy-MM-dd HH:mm:ss}] [%-5p] [%c{1}:%L] - %m%n</Property>
        <Property name="fileLayout">%d [%t] %-5level %c(%M:%L) - %m%n</Property>
    </Properties>
```

# apenders  
- log 메세지를 특정위치에 전달해주는 역할.(console or file 등등)
- ex) ConsoleAppender, RollingFileAppender,FileAppender,AsyncAppender ....
- 내부에 출력 방식을 설정해줄 수 있음.



`<Console>`은 콘솔에 로그를 보내는 방식  
    system.out 으로 로그를 출력하는 appender
```xml
<!-- 로그 출력 방식 -->
<Appenders>
  	<!-- 콘솔 출력 방식 -->
  	<Console name="console-log" target="SYSTEM_OUT">
  		<PatternLayout pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n" /> <-- prorperties 속성을 이용하여 주로 설정 -->
  	</Console>
</Appenders>
```
`<File>`은 파일에 출력할 방식

## RollingFileAppender
로그를 한 파일에만 저장하면 문제점이 생길 가능성이 커진다. 로그 파일 용량이 너무 커질 수도있고, 그 파일이 날아가면 모든 로그가 날아기기 때문이다. 이를 해결하기 위해 특정 기준을 설정하고 그에 따라 압축하여 저장하는 방식의 appender이다.  
이와 관련된 속성은 다음과 같다.  
## 1. Policy
file Rolling Up의 기준이다. 여러개의 policy를 적용할 수 있다.
- OnStartupTriggeringPolicy : jvm start시 trigger
- TimeBasedTriggeringPolicy : time에 따른 trigger
- SizeBasedTriggeringPolicy : file size에 따른 trigger
- CronTriggeringPolicy : Cron Expression(시간에 관한 표현)에 따른 trigger

위와 같이 특정 조건에 따라 trigger 된다. 
 

 ```xml
 <RollingFile name="File_Appender" fileName="logs/${logNm}.log" filePattern="logs/${logNm}_%d{yyyy-MM-dd}_%i.log.gz">
    <PatternLayout pattern="${layoutPattern}"/>
    <Policies>
         <SizeBasedTriggeringPolicy size="200KB"/>
         <TimeBasedTriggeringPolicy interval="1"/>
    </Policies>
    <DefaultRolloverStrategy max="10" fileIndex="min"/>
</RollingFile>
 ```
 위 예제에서는 로그 파일이 200kb가 되는 시점과 하루마다  `logs/${logNm}_%d{yyyy-MM-dd}_%i.log.gz` 패턴으로 압축된다.

## 2. DefaultRolloverStrategy


# Logger
로깅 작업의 주체, 각 패키지 별로 설정을 가능하다. 여기서 appender를 가져와서 사용하여 특정 패키지내에 적용시킨다. root는 일반적인 로그 정책을 의미한다 필수적으로 한개를 정의하여야한다.
```xml
<Loggers>
	<Logger name="moniter.PrinterClient" level="DEBUG"></Logger>
	<Root level="DEBUG">
		<appender-ref ref="console-log" level="DEBUG" />
	</Root>
</Loggers>
```

`<Logger>` 는 패키지를 포함한 클래스의 경로, 해당 name을 가진 로그에는 다음과 같은 level을 사용하겠다는 의미.
`<Root>`를 통해 로깅 방식을 임의로 설정 가능
`<name>` 은 정의되는 패키지 범위를 명시한다.

    -> Moniter.PrinterClient 에서 발생한 로그는 <Root>에서 작성한 console-log의 형식에 맞춰 로그를 출력하겠다는 의미
    Debug 이상의 로그들만 출력시키고 그 형식은 appender에서 정의한 것을 따른다.   
    (appender-ref를 통해 appender의 name을 지칭하고 있음)
    FATAL > ERROR > WARN > INFO > DEBUG > TRACE


