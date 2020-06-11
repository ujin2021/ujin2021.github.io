#### django 시작 명령어
git bash + vscode <br>
workspace를 만들 장소 -> git bash here <br>
$ mkdir <workspace> <br>
$ cd <workspace> <br>
$ python -m venv venv -> python 가상환경을 만든다. <br>
$ source venv/Scripts/activate -> 가상환경으로 들어감. (venv)가 뜬다. <br>
($ deactivate -> 가상환경에서 나올 때) <br>
(venv) $ pip install Django==3.6.0 -> django 3.6.0 버전을 install 한다. <br>
$ django-admin startproject [project_name] -> project를 만든다. <br>
$ cd [project_name] -> ls 했을 때 manage.py 가 있는 곳! <br>
$ python manage.py startapp [app_name] -> app을 만든다. <br>
($ pip install djangorestframework -> drf사용시 추가로 install) <br>
한번씩 pip install 할 때, pip 를 upgrade 하라는 문구가 뜬다. <br>
그럴때 -> $ python -m pip install --upgrade pip <br>
<br>
vscode에서 workspace 열기
1) $ code . -> vscode가 실행되고, terminal을 띄우면 자동으로 가상환경으로 들어감. <br>
2) vscode - file - open folder <br>
workspace folder를 열고, terminal을 띄우면 자동으로 가상환경으로 들어감. <br>
<br>

#### django 실행관련 명령어(project내에서 해야함. manage.py가 있는 곳에서)
$ python manage.py runserver <br>
($ python manage.py rusnerver 0.0.0.0:[port_number] - port number로 열어주기) <br>
$ python manage.py makemigrations [app_name] -> db생성 <br>
$ python manage.py migrate -> db저장 <br>
($ pypthon manage.py sqlmigrate [app_name] [migration_number] -> db저장시 돌아가는 sql문 프린트) <br>
($ python manage.py shell -> django shell에서 test가능) <br>
$ python manage.py createsuperuser -> admin 계정 생성 <br>
<br>

#### django 연습하기
django official docs : https://docs.djangoproject.com/ko/3.0/ <br>
tutorial을 따라해보는게 도움이 된다. 설명도 따로 올려주는 블로그들을 참고해서 하면 더 이해가 빠르게 된다.<br>
