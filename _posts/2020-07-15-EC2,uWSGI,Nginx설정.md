---
title: "EC2, uWSGI, Nginx 설정하기"
date: 2020-07-15 12:25:00 -0400
categories: server django python ubuntu linux ec2 aws uwsgi nginx
---

django 를 nginx+ec2 를 사용하여 배포하는 작업을 했다. 현재는 uWSGI연결 해놓은 상태이고, 앞으로 Nginx로 배포할 일이 남아있다.
적어놓지 않으면 나중에 다시할때 너무 힘들거같아서 포스팅을 한다!! 
정말 많은 블로그들을 참고하여 성공했다.ㅜㅜㅜ<br>
참고한 사이트 : https://rainsound-k.github.io/deploy/2018/04/25/instance-setting-and-django-deploy-part1.html (포스팅이 2개이다! 매우 유익했음)<br>
1) EC2 instance 접속후 locale설정($ sudo vi /etc/default/local) <br>
> EC2 생성, putty(or command)로 접속하는 방법은 구글링하면 매우 자세하게 나와있다.
2) apt-get update&upgrade, ... 설치(zsh관련 설치는 건너뛰었다) <br>
3) srv 폴더 생성한 후 권한 수정 <br>
4) local에서 .config 폴더 생성, 필요한 파일 추가, settings.py 몇가지 추가/수정 <br>
5) git clone <br>
> local에서 작업한 django를 git에 push해놓았다.<br>
> 우리는 branch 를 각자 파서 거기에 올려두기때문에 branch를 clone해야 했다. <br>
> 특정 branch clone하기 : git clone -b [branch_name] --single-branch [repo_addr] ([branch_name]대신 branch 이름 넣으면 됨!)<br>
> git bash로 scp를 사용하려했는데 계속 permission denied 가 떴다ㅜㅜ<br>
6) 필요한 requirements.txt install <br>

가상환경도 pyenv가 아니고 원래 쓰던 python -m venv venv를 사용해 만들었다. <br>
중간에 pyenv를 지우려고 rm명령어로 지웠는데 바보같이 가상환경에 접속한 상태에서 지워서 deactivate가 먹히지 않았다.<br>
해결방법은 그냥 인스턴스에 다시 접속하면된다.<br>

.config 추가 후 다른 블로그에서 uWSGI 실행하는 방법을 따라 해봤는데 이상하게 잘 되지 않았다.<br>
uwsgi 관련해서 필요한 것들을 다운받는데 pip 경로가 python3.5였다. 현재 3.8로 버전을 정해놨었는데 이상하게 pip는 3.5 (오류가 이것때문인지는 잘 모르겠다..)<br>
그래서 아까 requirements.txt 를 다시 다운받았는데 pip install 앞에 sudo -H python3.8 -m 를 붙였다.(앞으로도 계속 그래야될듯?) <br>
문제는 다른블로그들과 내가 참고했던 블로그의 방식이 차이가 있어서 이것을 고치는데 고생을좀 했다. <br>

다시 참고한 사이트 : https://wikidocs.net/7387 , https://zladnrms.tistory.com/79 <br>

1) uwsgi install 하기
2) 테스트해보기( uwsgi --http :8080 --home ~/srv/Closet/venv --chdir ~/srv/Closet/Closet/ --wsgi-fil~/srv/Closet/Closet/Closet/wsgi.py) 옵션의 내용은 위 사이트에 자세히 나와있다.<br>
3) 시스템전역에 uWSGI 설치 (sudo -H 옵션을 붙여 전역에 설치해준다! 가상환경을 빠져나와서!)<br>
4) uwsgi.service 파일 생성 - 이전에 만들어둔 service 파일이 있었는데 거기서 Description 내용과 ExecStart 부분을 바꿔주었다.
5) ini 파일 생성 - 위의 사이트에서는 .config 폴더에 생성해놓았는데 그냥 그 파일을 mv 명령어를 사용하여 /etc/uwsgi/sites 디렉토리로 옮겨주었다. <br>
> 위의 사이트에서는 pyenv를 사용했기때문에 경로에 pyenv가 들어가는게 있는데, 아까 생성해준 venv경로를 home에 대신 넣어주었다. <br>
6) uWSGI 서비스 등록, 구동확인
> 소스코드를 수정하면 uWSGI를 재가동해야한다 ($ sudo systemtl restart uwsgi)
