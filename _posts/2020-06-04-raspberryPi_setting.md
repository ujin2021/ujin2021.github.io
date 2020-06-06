---
title: "XProject Raspberrypi"
date: 2020-06-04 21:38:00 -0400
categories: raspberrypi server xproject
---
## 2020 HUFS IA XProject
#### Raspberry pi 서버구축!

### ~200604
#### 1. 장비 대여
* raspberry pi 3(model B+) <br>
* sdcard <br>
* 5핀 충전선 <br>
* HDMI 선 <br>
* monitor(빌리지 않고 실험실에 있는거 아무거나 사용)<br>
* usb 포트 keyboard, mouse

#### 2. raspberry pi setting
1. sd카드를 포맷한다(cmd로도 가능) - sdcard formatter 설치 후 format하면 됨. <br>
2. sd카드를 노트북에 꼽고 rasbian write. os설치 후 프로그램을 실행하면 자동으로 어떤 os를 어느 sd카드에 write할지 고를 수 있다. <br>
> rasbian download) https://www.raspberrypi.org/downloads/ <br>
3. sd카드를 라즈베리파이에 꼽고 전원선과 HDMI선을 연결하면 끝! <br>

#### 3. vnc설정
##### 라즈베리파이를 항상 가지고 다닐수 없으니 원격으로 제어할 수 있는 프로그램이 필요했다.
##### vnc viewer 설치
vnc viewer를 설치하면 raspberry pi ip를 입력하여 연결하면 화면을 볼 수 있다.(회원가입 필요) <br>
> https://www.realvnc.com/en/connect/download/viewer/ <br>
* raspberri pi 
1. ~$ ifconfig 명령어 입력. <br>
2. wlan0 에서 inet 주소 확인. <br>
3. raspberry 모양 - 기본설정 - Raspberry Pi configuration - interfaces - ssh, vnc 를 enable시켜준다. <br>
* windows <br>
1. vnc viewer를 실행시킨다. <br>
2. file - New connection - 아까 raspberry pi에서 확인한 inet주소를 입력. <br>
3. 로그인하여 들어가면 화면이 보인다. <br>

#### 3-1 putty로 연결
vnc는 그래픽이 보이지만 putty를 이용해 ssh로 접속시, terminal만 보인다. <br>
1. putty 설치 <br>
2. inet주소 입력후 ssh 선택 -> 연결 <br>
> putty 연결) https://cccding.tistory.com/97 <br>

#### 4. Port forwarding
외부에서 라즈베리파이를 원격으로 사용하기위해서는 포트포워딩이 필요하다. <br>
라즈베리파이 inet같은 경우 매번 접속할 때마다 바뀐다. 그러면 매번 원격 연결 시 ifconfig로 주소를 확인해줘야한다. <br>
-> 고정 IP 설정 필요. <br>
* 포트포워딩 설정 <br>
1. 192.168.0.1 접속 <br>
2. admin / admin으로 로그인(iptime) <br>
3. 관리도구 - 고급설정 - NAT/라우터 관리 - 포트포워딩 설정 <br>
4. +새규칙 추가 <br>
5. 규칙이름, 내부 IP주소(inet주소), 프로토콜(TCP), 외부포트(22~22), 내부포트(22~22) 설정후 적용. <br>
6. vnc 전 포트번호도 설정해준다.(위에것과 같은데 외부포트, 내부포트 모두 5900~5900으로 설정하면됨.) <br>
* 고정ip설정 <br>
1. 고정ip설정을 위해 고급설정 - 내부 네트워크설정 <br>
2. 수동 IP 할당 설정에서 왼쪽에서 inet과 같은 ip 선택후 추가버튼(오른쪽에 있는)누름. <br>
> 공유기 내부 ip 고정하기) https://studyforus.tistory.com/41 <br>

이제 외부에서도 라즈베리파이를 원격으로 제어할 수 있게 되었다! <br>

#### 5. python 설치
django를 사용하기때문에 python 을 다운로드 받아야 했다. <br>
기본적으로 파이썬 2.7 버전이 설정되어있다. 기본버전을 3.7로 바꾸기위해 파이썬을 설치해주어야 한다. <br>
> Install Python 3.7) https://tecadmin.net/install-python-3-7-on-ubuntu-linuxmint/ <br>
> 2.7 ver을 3.7로 바꾸기) http://amazingguni.github.io/blog/2016/03/python-2-%EC%97%90%EC%84%9C-3%EC%9C%BC%EB%A1%9C <br>
두가지를 해주고 ~$ python -V를 입력하면 3.7.3 이 기본 python 버전인걸 확인할 수 있다! <br> 

#### 6. venv 설정
django를 사용할 때 venv안에서 작업한다. <br>
> venv설정 및 django 설치) https://kis6473.tistory.com/40
