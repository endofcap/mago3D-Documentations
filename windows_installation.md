# 목차 
1. [Java](#1-java)
2. [PostgreSQL/PostGIS](#2-postgresqlpostgis)
3. [GDAL](#3-gdal)
4. [GeoServer](#4-geoserver)
5. [F4D Converter](#5-f4d-converter)
6. [RabbitMQ](#6-rabbitmq)
7. [기본-테이블-생성-및-패스워드-업데이트](#7-기본-테이블-생성-및-패스워드-업데이트)
8. [설정](#8-설정)
9. [레이어-업로드-테스트](#9-레이어-업로드-테스트)
10. [3D-데이터-업로드-테스트](#10-3d-데이터-업로드-테스트)

## 1. Java
### 1) Java 설치

- 자바설치 홈페이지(https://jdk.java.net/archive/)에 접속하여 Java를 구동하기 위한 환경에 해당하는 파일을 내려받는다.
- 내려 받은 압축 파일을 설치할 폴더에 압축 해제한다.

![img](./images/e238d7cc-e6ea-40c8-89b6-17f437362bde.png)


### 2) Java 환경변수 설정

- [제어판] → [시스템 및 보안] → [시스템] 또는 [내 PC]의 [속성]을 클릭 한 후, [고급 시스템 설정]을 클릭한다.

- [시스템 속성]의 [고급]탭 화면에서 [환경 변수]를 클릭한다.

- [환경변수] 화면에서 [새로 만들기]를 클릭하여, 변수 이름과 변수 값 입력란에 **JAVA_HOME**과 Java의 경로를 설정한다.
- Java 설치 경로를 설정 한 후, **시스템 변수의 [Path] 변수**를 선택하고 [편집] 버튼을 클릭한다.
- 제일 앞쪽에 **%JAVA_HOME%\bin;**을 입력하고 저장한다.

![img](./images/a8d04789-6e41-46ff-bf86-6c3ae9c1737c.png)

 

### 3) Java 설치 확인

- 명령 프롬프트 창을 실행시켜 아래와 같이 명령어 실행

```
1java -version
```

![img](./images/d55a5b4e-37dd-4c43-83be-15654a881691.png)


## 2. PostgreSQL/PostGIS
### 1) PostgreSQL 설치

- PostgreSQL 홈페이지(https://www.postgresql.org/)에 접속하여, Windows용 설치 파일을 내려 받는다 (https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)

![](./images/85f72c0f-1fcd-4192-a2a5-d7a14172c051.png)

- 내려 받은 파일을 실행하여, PostgreSQL을 설치한다.

- 폴더 버튼을 클릭하고 설치 할 경로를 지정한다.
- 설치 항목에 Stack Builder 는 선택하지 않는다. 

- Password와 Retype password를 “postgres”로 설정한다.

- 포트(5432) 및 지역(Korea)을 설정한다.

![](./images/89c4c1ba-989b-493b-9d91-75568d2d2fa4.png)

 

### 2) PostGIS 설치

- postGIS 설치 방법으로는 postgresql 설치 후 stack builder를 통해 설치 하거나 직접 인스톨러를 실행하는 방법이 있는데 후자로 진행하도록 한다. stack builder를 통한 설치는 종종 오류가 발생하고 인터넷이 되지 않는 환경을 고려했을 때는 인스톨러를 통한 설치가 안전하다. ([http://download.osgeo.org/postgis/windows/pg12/](http://download.osgeo.org/postgis/windows/pg11/))
- 여기서는 [postgis-bundle-pg12x64-setup-3.0.2-1.exe](http://download.osgeo.org/postgis/windows/pg12/postgis-bundle-pg12x64-setup-3.0.2-1.exe) 버전을 다운받는다.
- 내려 받기가 완료되면 PostGIS 설치를 진행할 수 있다.

![](./images/24fcd1b5-0cdf-4f0e-b4e6-c26c7c052a52.png)

### 3) PostgreSQL 설정

- 파티션 설정을 위해서 postgresql.conf 설정 파일의 “enable_partition_pruning = on”의 주석을 해제한다. 

- postgresql.conf 파일은 윈도우 기준으로 기본으로 설치시 **C:\Program Files\PostgreSQL\12\data** 경로에 위치하며, 설정파일 수정후에는 postgresql 서비스를 재시작해야한다.

  ![](./images/770b3afa-235c-42a4-b08f-2d5132130b8e.png)

   

### 4) 데이터 베이스 생성

- psql 또는 PgAdmin을 사용하여 데이터베이스에 연결한 후, 아래와 같이 데이터베이스를 생성한다.

  ```
  CREATE DATABASE mago3d
  WITH OWNER = postgres
       ENCODING = 'UTF8'
       TEMPLATE = template0
       TABLESPACE = pg_default
       LC_COLLATE = 'C'
       LC_CTYPE = 'C'
       CONNECTION LIMIT = -1;
  ```

### 5) PostGIS 활성화

- psql 또는 PgAdmin을 사용하여 생성한 데이터베이스에 연결한 후, 다음 SQL을 실행한다.

  ```
  CREATE EXTENSION postgis;
  ```

- PostGIS 설치 확인
  ```
  SELECT postgis_version();
  ```
## 3. GDAL
### 1) GDAL 설치

- gdal을 설치하는 이유는 shape 파일을 db에 insert할 때 ogr2ogr을 사용하기 위함인데, gdal만 단독으로 설치하거나 qgis를 설치하여 gdal path를 잡아주는 방법이 있다.

- GDAL을 설치하기 위해 홈페이지(https://trac.osgeo.org/osgeo4w/)에 접속한다.

- Download the [32bit] 0r [64bit]에서 [64bit]를 선택하여 해당 파일을 내려 받는다.

  ![img](./images/082213d2-83bf-44f4-a0dc-755fcf85a00f.png))

   

- 내려 받은 파일을 실행하여, 아래와 같은 설치 창이 생성된다.

- GDAL을 제외한 QGIS/GRASS GIS는 필요하면 체크하여도 무관하다.

![img](./images/ab65e39e-e7a4-4620-989d-9d249738ed4d.png)

### 2) GDAL 환경변수 설정

- [제어판] → [시스템 및 보안] → [시스템] 또는 [내 PC]의 [속성]을 클릭 한 후, [고급 시스템 설정]을 클릭한다.

- [시스템 속성]의 [고급]탭 화면에서 [환경 변수]를 클릭한다.
- **사용자 변수의 [Path] 변수**를 선택하고 [편집] 버튼을 클릭한다.

- [새로 만들기] 버튼을 클릭하여, **C:\OSGeo4W64\bin;**을 추가한다.
- **QGIS를 설치 후 환경변수를 추가할때는 qgis\bin 경로를 추가한다.**

### 3) GDAL 설치 확인

- 명령 프롬프트 창을 실행시켜 아래와 같이 명령어 실행하여 ogr2ogr 이 정상적으로 환경 변수로 등록되었는지 확인한다.

  ```
  ogr2ogr
  ```

  ![img](./images/7c07af03-a122-442e-ad42-7898ace65d5d.png)

- **환경변수를 등록한 후에는 was가 실행중이라면 재기동해야 등록한 환경변수가 적용되므로 주의해야한다. cmd 창역시 환경변수를 등록후에는 새로운 cmd 창에서 확인하도록 한다.**


## 4. GeoServer
### 1) GeoServer 설치

- GeoServer 홈페이지(http://geoserver.org/release/maintain/)에 접속하여, Tomcat에 war형태로 올려서 서비스하기 위해 Web Archive 버전을 내려 받는다.
- 내려받은 파일을 압축해제 후, geoserver.war파일을 톰캣의 webapp밑에 압축을 해제한다.

### 2) WPS Extensions

- GeoServer 버전에 맞는 플러그인을 내려 받는다.

https://sourceforge.net/projects/geoserver/files/GeoServer/2.16.4/extensions/geoserver-2.16.4-wps-plugin.zip/download

- gs-web-wps-2.16.4.jar

- gs-wps-core-2.16.4.jar

- gs-process-geometry-22.4.jar

- gt-xsd-wps-22.4.jar

- net.opengis.wps-22.4.jar

- serializer-2.7.2.jar

- 압축을 풀고, .jar 파일들을 Geoserer의 WEB-INF/lib 폴더에 복사하여 붙여 넣는다.

- GeoServer 재시작 후, 서비스 항목에 WPS 확인한다.

![img](./images/a2aa61eb-0c49-4fb6-9ba0-d07e294c27d8.png)

### 3) OpenGXT + GeoTools Extentions

- GeoServer 버전에 맞는 플러그인을 내려 받는다.

https://sourceforge.net/projects/mango-spatialstatistics/files/GeoServer/

- gs-wps-spatialstatistics-2.16-SNAPSHOT.jar

- gt-process-spatialstatistics-22-SNAPSHOT.jar

- 압축을 풀고, .jar 파일들을 Geoserer의 WEB-INF/lib 폴더에 복사하여 붙여 넣는다.

- GeoServer 재시작 후, 바인딩된 WPS 프로세스 확인

- GeoServer 관리자 페이지 → 데모 → WPS 요청 빌더

![img](./images/4559786b-7e34-4642-bfbf-07c7b8318c2a.png)

 

### 4) CORS 설정

- 톰캣의 web.xml에 다음과 같이 설정한다.

  ```
  <filter>
      <filter-name>CorsFilter</filter-name>
      <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
      <init-param>
          <param-name>cors.allowed.origins</param-name>
          <param-value>*</param-value>
      </init-param>
      <init-param>
          <param-name>cors.allowed.methods</param-name>
          <param-value>GET,POST,HEAD,OPTIONS,PUT</param-value>
      </init-param>
      <init-param>
          <param-name>cors.allowed.headers</param-name>
          <param-value>*</param-value>
      </init-param>
      <init-param>
          <param-name>cors.exposed.headers</param-name>
          <param-value>Access-Control-Allow-Origin,Access-Control-Allow-Credentials</param-value>
      </init-param>
      <init-param>
          <param-name>cors.preflight.maxage</param-name>
          <param-value>10</param-value>
      </init-param>
    </filter>
  ```

### 5) 작업공간 및 저장소 생성

- GeoServer 관리자 페이지 → 데이터/작업공간 → **새로운 작업공간 추가하기**

```
이름 : mago3d
네임스페이스 URI : http://www.gaia3d.com
기본 작업공간으로 설정하기 체크
```

- GeoServer 관리자 페이지 → 데이터/저장소 → 새로운 저장소 생성하기 → PostGIS

- postgresql 설치 후 생성한 database정보를 적어주는데 postgis extension을 설치하지 않으면 저장소 등록이 되지 않으므로 꼭 생성한 db에 posgis extension을 설치해야 한다.

  ![img](./images/627b0410-bfa7-4596-84c4-f5e973057a75.png)
## 5. F4D Converter
- 아래의 링크를 통해서, Converter를 내려 받는다.

[F4DConverter/SetupF4DConverter.msi at master · Gaia3D/F4DConverter](https://github.com/Gaia3D/F4DConverter/tree/master/install) 

- **설치경로 : C:\F4DConverterConsole**

- 내려 받은 파일을 실행하여, Converter를 설치한다.

  ![](./images/8890e42d-321c-4dc3-aa0f-3a7a603276bd.png)
## 6. RabbitMQ
- RabbitMQ란 표준 MQ 프로토콜인 AMOP(Advanced Message Queueing Protocol)의 메시지 브로커 소프트웨어 오픈소스이다.

- RabbitMQ는 처리가 빠르다는 장점이 있으나, Erlang 의존성이 있기 때문에 설치에 앞서 Erlang/OTP를 필수적으로 설치해야 한다. RabbitMQ 버전별 Erlang 버전은 RabbitMQ 홈페이지[(https://www.rabbitmq.com/which-erlang.html](https://www.rabbitmq.com/which-erlang.html))에서 확인할 수 있다.

### 1) Erlang 설치

- rabbit mq 를 설치하기 위해서는 erlang 이 필요하기 때문에 먼저 erlang를 설치하도록 한다.

- Erlang 홈페이지(https://www.erlang.org/downloads)에 접속하여, Download OTP 23.0 항목에서 ‘OTP 23.0 Windows 64-bit Binary File’을 클릭하여 파일을 내려 받는다.

  ![img](./images/9888965a-6966-4969-8aac-538e9b2b5fc2.png)

   

- 내려 받은 파일을 실행한다.

- 구성요소 설정은 기본값으로 할 것이므로 next를 클릭한다.

- 설치 경로는 erlang 버전 관리를 용이하게 하고자 **‘C:\erlang\otp_22.2\erl10.6’** 으로 설정한다.

- 설치 버튼을 클릭한다.

- Visual C++ 구성요소 설치 창이 뜰 경우 체크박스를 체크하고 설치한다.

### 2) RabbitMQ 설치

- RabbitMQ 홈페이지(https://www.rabbitmq.com/download.html)에 접속하여, 최신 버전(RabbitMQ 3.8.7)을 확인하고 운영체제 환경에 맞춰 설치 파일을 내려 받는다.
- 기본 설정으로 설정하고 Next를 클릭한다.

- 경로는 **‘C:\RabbitMQ\’**로 설정하고, Install을 클릭하여 설치를 진행한다.

- 설치가 완료된 것을 확인하고, next를 클릭한다.

- 작업이 끝난 것이 확인되면 Finish 버튼을 클릭하여 창을 닫는다.

 

### 3) RabbitMQ 환경변수 설정

- [제어판] → [시스템 및 보안] → [시스템] 또는 [내 PC]의 [속성]을 클릭 한 후, [고급 시스템 설정]을 클릭한다.

- [시스템 속성]의 [고급]탭 화면에서 [환경 변수]를 클릭한다.

- [환경변수] 화면에서 [새로 만들기]를 클릭하여, 변수 이름과 변수 값 입력란에 **RABBITMQ_HOME**과 RabbitMQ 설치 경로를 설정한다.
- RabbitMQ 설치 경로를 설정 한 후, **시스템 변수의 [Path] 변수**를 선택하고 [편집] 버튼을 클릭한다.
- [새로 만들기] 버튼을 클릭하여, **%RABBITMQ_HOME%\sbin** 을 입력한다.

### 4) RabbitMQ 관리자페이지([http://localhost:15672](http://localhost:15672/)) 접속

- 관리자 페이지에 접속하기 위해서는 management plugin이 활성화 되어야 한다. (비활성화 시 접속 불가)
- RabbitMQ의 management plugin을 활성화하기 위해 명령 프롬프트 창에 ‘**rabbitmq-plugins enable rabbitmq_management**’라고 입력하여 활성화한다.
- 명령 프롬프트를 재시작하고, ‘**rabbitmq-plugins list**’로 플러그인의 활성화 여부를 확인한다.

### 5) RabbitMQ 관리자 설정

- RabbitMQ 관리자 페이지([http://localhost:15672](http://localhost:15672/))에 접속한다.
- 아이디와 비밀번호는 모두 guest로 로그인한다.

![img](./images/1cf84fa2-a2e4-47ad-b841-7dbfbca3ebd8.png)

- 상단에 Exchange 탭을 클릭한다.

- 하단에 Add a new exchanges를 클릭하여 우측 그림과 같이 입력한 뒤, Add exchange 버튼을 클릭한다.

  - *Name : f4d.converter*
  - *Type : topic*
  - *Durability : Durable*

  ![img](./images/88198c22-8c7c-4156-990e-cdf72b6cbe30.png)

- 상단에 Queues 탭을 클릭한다.

- 하단에 Add a new queue를 클릭하여 우측 그림과 같이 입력한 뒤, Add queue 버튼을 클릭한다.

  ![img](./images/ff5e92b7-3c79-45dc-93b6-2d756de039b0.png)

- RabbitMQ를 처음 설치하면 guest 계정이 Administrator로 권한 설정되어 있다. 하지만 이 계정으로 application에 접속할 시, 접속 관련 오류가 발생한다.

- 다른 관리자 계정을 생성하기 위해 Admin 메뉴 하단의 Add a user를 클릭하고 우측 그림과 같이 입력하여 관리자 계정을 생성한다. (아이디와 비밀번호는 동일하다.)

  - Username : mago3d

  - Password : mago3d 

    ![img](./images/b2448bde-1ea4-44b6-abe2-7082ae64cb97.png)

   
- guest 아래에 새로 생성된 mago3d계정을 클릭한다.

- 아래 화면과 같이 Current permissions, Current topic permissions을 생성하고 Update this user에 비밀번호(mago3d)를 입력한 뒤, 하단의 Update user 버튼을 클릭한다.

  ![](./images/d7b0e288-9c6a-4f45-af18-461a693c6a9a.png)
  
## 7. 기본 테이블 생성 및 패스워드 업데이트
### 1) SQL 실행

- mago3d 프로젝트 내 doc 폴더에서 [database] – [ddl] 폴더와 [dml] 폴더에서 sequence 폴더부터 dml 폴더 안의 update.sql까지 순서대로 실행한다.
- **단 reference.sql, delete.sql 은 제외(참조키 관련내용은 실행하지 않는다.)**
- **프로젝트의 브런치는 develop이 아닌 mago3d 브런치를 사용하도록 한다.**
- pgAdmin4를 사용할 경우 실행할 sql 파일들을 선택후 드래그하여 한번에 실행할 수 있다.

### 2) mago3d-web application 계정 비밀번호 설정

- [mago3d-admin] – [src/test/java] – gaia3d– units – passwordTest.java 파일을 Junit Test로 실행한다.

- Console창에 뜬 encodePassword 값을 복사하여 pgAdmin의 Query Editor에서 update문(update user_info set password = ‘(encodePassword 값)’ where user_id=＇admin’; )으로 admin의 비밀번호 값을 변경한다. mago3d 계정도 동일한 방식으로 변경한다.

  ![img](./images/Table-1.png)

   
## 8. 설정
## 9. 레이어 업로드 테스트
## 10. 3D 데이터 업로드 테스트
