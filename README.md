### Spring 프로젝트 AWS에 배포하기 (macOS)



1. #### [EC2 인스턴스 생성](https://blog.naver.com/dudu1104/221043424470)

2. #### [RDS서버 생성하기](https://blog.naver.com/dudu1104/221054584842)

   ```
   - RDS(Relational Database Service) : 클라우드에서 관계형 DB를 간편하게 설정,운영 할 수 있다
   
   - Oracle DB 인스턴스를 생성했으므로 Oracle SQL Developer를 실행해 접속해주었다. 이 때 호스트 이름에는 DB 인스턴스의 엔드포인트를, SID에는 DB 이름을 넣어준다. 접속 이름을 알아서, 사용자 이름은 2에서 설정한 Master Username을, 비밀번호는 Master Password를 넣어준다.
   
   * 기존의 DB 내용을 생성한 DB 인스턴스에 넣고자 한다면 Oracle SQL Developer에서 [도구]-데이터베이스 익스포트를 클릭한다.
   그리고 아래와 같이 테이블공간과 사용자 생성 및 권한 설정을 해준 후(AWS RSD에서는 테이블공간 설정 시 파일과 사이즈 입력하면 안 됨), 익스포트된 Sql문을 실행해주면 기존 DB와 똑같아진다.
   ```

   ```
   -- 테이블스페이스 생성
   CREATE TABLESPACE SCHOOL_TS;
   
   -- 사용자 생성
   CREATE USER SCHOOL -- 사용자명
   IDENTIFIED BY 1234 --비밀번호
   DEFAULT TABLESPACE SCHOOL_TS;
   
   -- 권한 부여
   GRANT CONNECT, RESOURCE TO SCHOOL; --CONNECT와 RESOURCE 롤 부여
   ```



3. #### [AWS Ubuntu에 웹 개발환경 구축하기](http://blog.moramcnt.com/?p=1061)

   - 스프링 프로젝트를 실행시키기 위한 환경설정 필요.  JDK, 아파치, 톰캣 설치



4. #### STS(Spring Tool Suite) 설치

   - STS 파일 다운로드

     ```
     > wget http://download.springsource.com/release/STS/3.9.5/dist/e4.4/spring-tool-suite-3.6.3.RELEASE-e4.4.1-linux-gtk-x86_64.tar.gz
     ```

   - 압축 해제

     ```
     > sudo tar -xvf spring-tool-suit(Tab 자동완성)
     ```

     ```
     > sudo ln -s/opt/sts-bundle/sts-3.9.5.RELEASE/STS/usr/local/bin/sts
     ```

     

5. #### git 설치

   ```
   > sudo apt-get install git
   > git --version  (git version 제대로 나오는지 확인)
   ```



6. #### Tomcat에 WAR 파일 배포하기

   - 우분투에서 `> cd /var/lib/tomcat8/webapps/` 경로에 war 파일을 넣어주면 된다

     github 레포지토리에 war 파일을 push 하고 ubuntu에서 clone해 위 경로에 넣어주면 된다

   - webapps 경로에 war 파일을 넣는데 실패하면 권한문제이므로 아래와 같이 권한을 변경한다

     ```
     > sudo su
     > chmod -R 777 /var/lib/tomcat8/webapps
     > chown -R tomcat8:tomcat8 /var/lib/tomcat8/webapps
     ```

   - tomcat 재시작

     ```
     > sudo service tomcat8 restart 
     ```

   - 브라우저 검색창에`서버ip:8080/프로젝트명` 을 입력해 프로젝트가 제대로 돌아가는지 확인한다



7. #### [파일 저장을 위해 S3버킷 생성하기](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/user-guide/create-bucket.html)

   - 프로그램은 잘 돌아가지만 기존에 로컬 경로에 저장해 둔 이미지 파일들이 엑박으로 뜨는 문제점 발생
   - 기존 로컬 경로에 저장하던 파일들을 S3버킷으로 옮겨주어야 함



8. #### [S3버킷 스프링과 연동하기](https://shj7242.github.io/2017/12/28/Spring34/)

   - 기존 프로젝트에서 사용자가 계정을 생성할 때 프로필 이미지를 업로드하면 로컬 경로에 저장해 둠.
   - 이 부분에서 이미지 파일이 AWS 버킷에 저장되도록 코드 변경

   