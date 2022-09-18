--File객체에 대해

    1> File 객체의 정의 
                 * File객체는 하드디스크에 존재하는 실제 파일이나 디렉토리가 아니고 그것에 대한 경
                    로(Pathname) 또는 참조(reFerence)를 추상화한 객체이다. 파일 객체는 새 파일에 
                    대한 경로나 만들고자하는 디렉토리를 캡슐화한 것이다.
        2> File 객체의 용도 
                 * 물리적 파일 시스템에 대해 캡슐화한 경로명을 확인하고 실제의 파일이나 디렉토리 
                   와 대응되는지 알아볼 때.
                 * 파일 스트림객체를 생성하고자 할 때.
1. File 객체로 작업하기
        1> File 객체의 정의 
                 * File객체는 하드디스크에 존재하는 실제 파일이나 디렉토리가 아니고 그것에 대한 경
                    로(Pathname) 또는 참조(reFerence)를 추상화한 객체이다. 파일 객체는 새 파일에 
                    대한 경로나 만들고자하는 디렉토리를 캡슐화한 것이다.
        2> File 객체의 용도 
                 * 물리적 파일 시스템에 대해 캡슐화한 경로명을 확인하고 실제의 파일이나 디렉토리 
                   와 대응되는지 알아볼 때.
                 * 파일 스트림객체를 생성하고자 할 때.

2.  File 객체 생성하기 
        1>File 객체를 생성하는 데에는 네 개의 생성자를 사용할 수 있다.
                * 인자로 String 객체를 전달하는 것.
                       File mtDir - new File("c:/j2SDK 1.4.0/src/java/io");
                       File 클래스의 생성자는 인자로 전달된 경로를 확인하지 않는다 .
                       File 클래스의 생성자로 어떤 문자열을 넘겨도 된다는 것을 알 수 있다.
                       File 객체는 불변적이므로 객체를 생성하고 나면 그것이 가진 경로를 바꿀 수 없다
                * 첫번째 인자로 부모 디렉토리를 가진 File객체 전달, 두번째 인자로 String객체 전달.
                  부모 디렉토리 - 해당 파이을 가지고 있는 디렉토리.
                * 첫번째 인자로 부모 디렉토리의 경로를 String객체로 전달, 두번째 인자로 String객체 
                   전달.
                * URL이용.
        2>경로에 대한 이식성을 고려
                * 경로 이름을 만드는 것은 시스템에 의존적이지만 File 클래스의 separator 변수를 사
                  용함으로써 좀 더 시스템에 독립적으로 myFile에 대한 경로를 지정 가능.
        3>절대경로와 상대경로
                * 절대 경로 : 접두사(c:\, c://)를 포함하는 경로.
                  ex) cd c:\java\API 이런 식의 경로 이동을 하는 방법이 절대 경로.
                * 상대 경로 : 접두사가 없는 경로.
                  ex) cd java 이런 식의 경로 이동을 하는 방법이 상대 경로.
        4>시스템 속성에 접근하기
                * 파일에 대한 상대 경로를 지정하는 것이 시스템 독립적이긴 하지만 현재의 환경에 독
                 립적인 경로를 지정하고자 하는데, 현재 디렉토리 또는 현재 디렉토리의 하위 디렉토
                 리는 데이터 파일을 저장하기에 적합하지 않은 상황도 있을 수 있다. 이런 경우에 시스
                 템 속성에 접근하는 것이 도움이 될 수 있다.

3. File 객체를 확인하고 테스트하기
        1> File 객체에 대한 정보를 얻을 수 있는 메서드
                getName() - 경로를 제외한 파일의 이름. 즉, 경로의 가장 마지막 부분을 String 객체로 
                                   리턴한다.
                getPath() - 파일이나 디렉토리 이름을 포함한 File 객체의 경로를 String으로 리턴한다.
                isAbsolute() - File 객체가 절대경로를 참조하고 있다면 true, 아니라면 false를 리턴한
                                    다.
                getParent() - 현재 File 객체가 나태내는 파일 EH는 디렉토리의 부모 디렉토리의 이름
                                   을 String으로 리턴한다. 이때 리턴되는 경로는 파일 이름을 포함하지 않
                                   은 순수한 경로명이다. 만약 File 객체가 현재 디렉토리에서 단순히 파일 
                                   이름만을 사용해서 만들어져 부모 디렉토리가 명시되어 있지 않을 경우
                                   에는 null을 리턴한다.
                toString() - 현재 File 객체의 String표현을 리턴하며 File 객체가 String 객체로 변환될 
                                필요가 있을 때 자동으로 호출된다. 
                hashCode() - 현재 File 객체의 해시코드 값을 리턴한다. 
                equals() - 두 개의 File 객체가 동일한 것인지 비교할 때 사용되는 메소드이다. 인자로 
                               넘겨지는 File 객체가 현재의 객체와 같은 경로를 가지고 있으면 true를 그렇
                               지 않으면 false를 리턴한다.
        2> File 객체가 참조하는 파일이나 디렉토리를 테스트하기 위한 메서드
                exist() - File 객체가 참조하는 파일 또는 디렉토리가 실제로 존재하면 true를, 그렇지 
                            않으면 false를 리턴한다.
                isDirectory() - File 객체가 디렉토리를 참조하는 경우 true를, 그렇지 않으면 false를
                                     리턴한다.
                isFile() - File 객체가 파일를 참조하는 경우 true를, 그렇지 않으면 false를 리턴한다.
                isHidden() - File 객체가 숨김 속성이 있는 파일을 참조하는 경우 true를, 그렇지 않으
                                  면 false를 리턴한다.
                canRead() - File 객체가 참조하는 파일 또는 디렉토리를 읽을 권한이 있다면 true를, 
                                  없다면 false를 리턴한다. 만약 권한이 없는데 읽으려고 시도하면 
                                  SecurityException이 발생한다.
                canWrite() - File 객체가 참조하는 파일 또는 디렉토리에 쓰기 권한이 있다면 true를, 
                                  없다면 false를 리턴한다. 만약 권한이 없는데 읽으려고 시도하면 
                                  SecurityException이 발생한다.
                getAbsolutePath() - 현재의 File 객체가 참조하는 파일 또는 디렉토리의 절대경로를 
                                             리턴한다. 만약 객체가 절대경로를 포함하고 있다면 getPath() 메
                                             소드에 의해 리턴되는 것과 동일한 문자열을 리턴할 것이다. 그렇
                                             지  않다면 윈도우즈 환경에서는 현재 디렉토리가 나타내는 경로
                                             의 드 라이브 이름을 사용하거나 경로에 드라이브 이름이 명시되
                                             어 있지 않다면 현재 사용자 디렉토리에 대해서 절대경로가 결정
                                             된다. 유닉스 환경에서는 현재 사용자 디렉토리에 대해서 절대 경
                                             로가 결정된다.
                getAbsoluteFile() - 현재 File 객체가 참조하는 파일 또는 디렉토리의 절대경로를 포함
                                            한 File 객체를 리턴한다.


--MultipartRequest

httpservletRequest 처럼 사용도 가능 (ex_getPrameter 메소드 사용)
들어오는 파일을 읽고 저장한다.

메소드
getFile(name)-> 특정된 파일의 File 객체를 반환
getFileNames() -> 업로드된 파일의 이름을 반환

getFileNames 로 업로드 파일의 이름을 받아오고 
반복문을 사용하여 getFile로  File 객체 받아옴, 이때 multipartFile 로 변환 가능 multipartFile은 multipart request에 업로드된 파일 객체


