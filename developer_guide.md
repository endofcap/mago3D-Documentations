# 목차 

0. [mago3D 아키텍처 및 구성](#0-mago3d-아키텍처-및-구성)
1. [개발환경 구축]()
2. [프로젝트 구성]()
3. [DataGroup, DataInfo와  F4D와의 관계]()
4. [RabbitMQ 동작 이해하기]()
5. [3차원 데이터 자동 변환]()
6. [레이어 관리]()
7. [권한 관리]()
8. [정적 컨텐츠 갱신]()
9. [암호화]()
10. [메모리 캐시]()
11. [자바]()
12. [DB 규약]()
13. [코딩 규약]()
14. [빌드]()
15. [로깅]()
16. [Web/ WAS Server 차이점]()
17. [Web/WAS Server 연동 및 테스트]()
18. [Tomcat 설정]()
19. [서비스 등록]()
20. [Windows 서버 구축하기]()



## 0. [mago3D 아키텍처 및 구성](./README.md) 

## 1. 개발환경 구축

- java 설치
- docker-compose 로 개발환경을 구축하는 경우 geoserver, database, rabbitmq 설정은 skip
- intellij 에서 개발환경을 구축하는 경우 static resource 들을 build 없이 갱신하기 위하여 resource 경로를 file path 로 잡아 주기 때문에, 실행을 bootRun 으로 실행하거나 configuration 에 Working directory 를 **$MODULE_WORKING_DIR$** 로 설정해 주어야 한다.
  [설치 가이드](./installation_guide.md)
- 