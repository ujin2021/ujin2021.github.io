---
title: "djangoRestFramework+mysql+signup(2일차)"
date: 2020-06-30 00:22:25 -0400
categories: django server python project drf mysql database
---
오늘의 목표는 django와 mysql을 연동하고, 회원가입/로그인 코드와 aws에 django 환경 만들기, django aws에 올리기 였다<br>
mysql이야 저번에 연동해봤으니 빨리 할 수 있을 거라 생각했는데 예상외로 db연동에 애를 먹었다..ㅜㅜ<br>

일단 django 버전 2.1에서는 mysql 5.6이상 연동가능하다.<br>
django version 확인하기 : <br>
>>>import django <br>
>>>django VERSION <br>

mysql은 8.0.20 을 다운받았다. <br>
db관리는 dbeaver를 사용했다.<br>

먼저 mysql을 다운받고, commandline을 실행시킨다. 일단 db table을 만들어야 한다.<br>
mysql 다운로드는 블로그를 참조하였다. 블로그주소 : https://dog-developers.tistory.com/20 <br>
db table 생성하기 : <br>
command창을 띄우면 비밀번호를 입력해야한다. ;도 붙여줘야한다. 안붙여주면 >가 밑에 뜨는데 ;를 입력해주면 된다! <br>
mysql> select version(); // mysql 버전 확인 <br>
mysql> create database [생성할 db 이름]; <br>
mysql> show databases; // database 리스트 보기 <br>
(mysql> drop database [삭제할 db 이름]; // database 삭제시)<br>

database를 만든 후 django project 의 settings.py 에 들어가 db설정내용을 변경해주어야 한다.<br>
참고한 블로그 주소 : https://myjamong.tistory.com/102 <br>

db관리 프로그램은 dbeaver를 이용하였다. 이것도 학원에서 배울 때 사용하던것인데, 편리하다.<br>
vscode bash shell에서 mysqlclient도 설치해준다.
$ pip install mysqlclient <br>

models.py, serializers.py, views.py 에 코드를 입력하고(코드설명은 완성되면 정리하여 올릴 예정이다) migrate를 해주어야 db에 반영된다.<br>
$ python manage.py makemigrations <br>
$ python manage.py migrate <br>

db table이 잘생성되었나 확인을 위해 dbeaver에 들어가 새로고침하거나, mysql shell에서 확인가능하다.<br>
command 창 table 생성 확인: <br>
mysql> use [db이름] // 생성한 db 사용 <br>
mysql> show tables; // table 확인 <br>


나는 User모델을 그대로 사용하면서 user table명을 바꾸고싶었다.. 하지만 이때부터 migrate할 때마다 오류가 발생했다.<br>
rest-auth registration에서는 username, email, password만 나오고 내가원하는 걸 적용하기 힘들었다.(물론 나의 지식과 구글링 실력이 모자라기 때문에..)<br>
email인증을 좀더 편리하게 하기위해(?), 좀더 원하는걸 유연하게 하기위해서 일반 models.py에 정의하고 싶었기 때문이다.
models.py 에서 AbstractUser를 import해서 class User(AbstractUser)를 만들고 class Meta로 db_table명을 'User'로 지정해주었다.<br>
이후 settings.py에서 AUTH_USER_MODEL = 'account.User' 를 지정해주었다. (참고:https://dlwodus.tistory.com/373)<br>

그런데 migrate할 때마다 closet db에는 user table이 없다,, account app이 설치되지 않았다 등등 여러 오류가 발생했다ㅜㅜ<br>
정말 한두시간동안 애썼다. mysql과 연동문제인가? 그것은 아니었다. superuser 을 생성했을 땐 바로 db에 잘 저장되었기 때문이다.<br>

정말 오랜 구글링 끝에 블로그를 들어가게 되었다.(참고 : https://bangseogs.tistory.com/78) <br>
models.py에서 생성한 table이 migration으로 db에는 생성이 안되는 오류였는데, 이 블로그 마지막 부분에 나온다.<br>
해결방법 : https://stackoverflow.com/questions/35494035/django-migrate-doesnt-create-tables/43677713#43677713<br>
$ python manage.py migate --fake [APPNAME] zero <br>
$ python manage.py migrate [APPNAME] <br>

--fake zero 명령어 입력후 migrate하니 잘되었다(구글링하다 찾은것 중에 user관련된 db테이블을 싹다 지우라는 글이 있었는데 그건 잘못된것같다.. 나도 싹다 지웠는데 결국 새로운 db를 다시 생성한 후 fake zero 명령어로 해결했다<br>
오늘이거하다 시간다갔다 내일은 더 열심히 화이팅해서 로그인까지 끝내보자! <br>
