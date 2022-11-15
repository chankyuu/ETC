# RoseRT
http://www.uml.org.cn/umltools/pdf/docview.pdf    

## Structure diagram 

캡슐의 인터페이스와 내부 구성을 지정하는데 사용한다.

    
![image](https://user-images.githubusercontent.com/60462925/201046588-58fea571-79de-4730-b13d-af0cdb2becdd.png)
                

## State diagram

 캡슐이 할 수 있는 행동들 표현

![image](https://user-images.githubusercontent.com/60462925/201048560-e2462fd8-3aee-441b-9768-d9cbfd2d1f41.png)

## Flow 
 + 상태가 변화함에 따라 chain*_함수이름 호출,
 + chain*_함수 내부의 transition 함수 호출

## etc
 + Active Objects : 캡슐화 셸, 실행 후 완료 동작, 자체 실행 스레드 데이터가 캡슐화되어 있는 개체
 + Capsule : =Active Objects
 + Port : 포트를 통해 interfaces 기반 메세지를 주고받으며 캡슐끼리 의사소통함, 이러한 메세지는 프로토콜에 명시적으로 정의된다.