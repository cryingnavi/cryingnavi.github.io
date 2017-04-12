---
layout: post
title: Raspberry Pi Kiosk
date: "2017-03-05 23:16"
---

라즈베리 파이를 키오스크 모드로 돌리는 방법을 알아본다.

### 라즈비안 다운로드
- [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/)

### 부팅시 화면 회전
```
sudo vi /boot/config.txt
add display_rotate=3 ( 이와 같이 작성하면 화면이 시계 방향으로 90도 회전 )

disable_overscan=1
overscan_left=-150
overscan_right=-150
```

### 부팅 로그 감추기
```
sudo vi /boot/cmdline.txt
```

### 부팅 이미지 표시하기
```
sudo apt-get install fbi
vi asplashscreen
```

아래 do_start 부분에서 이미지 경로를 원하는 이미지로 변경한다.
```
#! /bin/sh
### BEGIN INIT INFO
# Provides:          asplashscreen
# Required-Start:
# Required-Stop:
# Should-Start:      
# Default-Start:     S
# Default-Stop:
# Short-Description: Show custom splashscreen
# Description:       Show custom splashscreen
### END INIT INFO

do_start () {
    /usr/bin/fbi -T 1 -noverbose -a /etc/sample.png    
    exit 0
}

case "$1" in
  start|"")
    do_start
    ;;
  restart|reload|force-reload)
    echo "Error: argument '$1' not supported" >&2
    exit 3
    ;;
  stop)
    # No-op
    ;;
  status)
    exit 0
    ;;
  *)
    echo "Usage: asplashscreen [start|stop]" >&2
    exit 3
    ;;
esac
:
```

```
sudo mv asplashscreen /etc/init.d/asplashscreen
sudo chmod a+x /etc/init.d/asplashscreen
sudo insserv /etc/init.d/asplashscreen
```

만약 바로 위 코드 실행시 current start runlevel(s) (2 3 4 5) of script `asplashscreen' overrides LSB defaults (S). 라는 에러가 발생한다면, 다음 명령을 실행한다.

```
sudo insserv -d /etc/init.d/asplashscreen
```

### 라즈비안 GPU 메모리 올리기
```
sudo raspi-config
Advanced Options > Memory Split > 128
```

### 재시동
마지막으로 라즈비안을 재시동한다.
```
sudo reboot
```

### 라즈비안 자동 로그인 하기
systemd 사용 하는 경우
```
ln -fs /lib/systemd/system/getty@.service  /etc/systemd/system/getty.target.wants/getty@tty1.service
To switch back to automatic login, do:
ln -fs /etc/systemd/system/autologin@.service  /etc/systemd/system/getty.target.wants/getty@tty1.service
```
inittab 사용 하는 경우
```
 vi /etc/inittab
 1:2345:respawn:/sbin/getty --autologin {USERNAME} --noclear 38400 tty1
```


### x window 설치하기
리눅스에서 디스플레이창을 표시하고 마우스와 키보드와 상호작용하는 GUI 환경을 위한 프레임웍이 x window 이다. 이를 설치해야만 크로미움이 화면에 표시될 수 있다.
```
sudo apt-get install matchbox-window-manager unclutter xinit xserver-xorg xserver-xorg-legacy x11-xserver-utils
```


### 크로미움 설치하기
```
wget http://launchpadlibrarian.net/237755896/libgcrypt11_1.5.3-2ubuntu4.3_armhf.deb
wget http://launchpadlibrarian.net/263322754/chromium-codecs-ffmpeg-extra_51.0.2704.79-0ubuntu0.14.04.1.1121_armhf.deb
wget http://launchpadlibrarian.net/263322752/chromium-browser_51.0.2704.79-0ubuntu0.14.04.1.1121_armhf.deb

sudo dpkg -i libgcrypt11_1.5.3-2ubuntu4.3_armhf.deb
sudo dpkg -i chromium-codecs-ffmpeg-extra_51.0.2704.79-0ubuntu0.14.04.1.1121_armhf.deb
sudo dpkg -i chromium-browser_51.0.2704.79-0ubuntu0.14.04.1.1121_armhf.deb
```
만약 마지막 chromium-browser_51.0.2704.79-0ubuntu0.14.04.1.1121_armhf.deb 설치시 의존성 에러가 발생하면 아래 명령어를 실행한다.
```
apt-get -f install 실행
```

### 크로미움 필요라이브러리 설치
```
sudo apt install libnss3
sudo apt install -f
```

### 한글 설치
```
sudo apt install ttf-unfonts-core
```

### 크로미움 자동 시작 스크립트 만들기
```
vi /home/pi/startkiosk.sh
```
```
#!/bin/bash

# disable DPMS (Energy Star) features.
xset -dpms

# disable screen saver
xset s off

# don't blank the video device
xset s noblank

# disable mouse pointer
unclutter -idle 0 -root &

# run window manager
matchbox-window-manager -use_cursor no -use_titlebar no  &

# run chromuim
#chromium-browser --noerrdialogs --kiosk --incognito https://www.google.co.kr/
if [ $# -ne 0 ];
then
        chromium-browser --noerrdialogs --kiosk --incognito $1
else
        chromium-browser --noerrdialogs --incognito https://www.google.co.kr/
fi
```
```
chmod +x /home/pi/startkiosk.sh
```
실행하기 위해서는 아래 처럼 한다.
```
vi /home/pi/.bashrc
```
```
if [ -z "${SSH_TTY}" ]; then
  xinit ~/startkiosk.sh
fi
```


### 재시동
마지막으로 라즈비안을 재시동한다.
```
sudo reboot
```
