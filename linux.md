## VMWare deault Network
host PC : 192.168.xxx.1
Router : 192.168.xxx.2
DNS : 192.168.xxx.2
DHCP : 192.168.xxx.254

## Remote Login
- server : Telnet#23, SSH#22
- Client : Secure-CRT, Tera-Term, Netterm, Putty....

## kernel 확인
```
- ls /boot
vmlinuz-2.6.18-164.el5
```
## Shell 확인
```
- ps
  PID TTY          TIME CMD
 3953 pts/2    00:00:00 bash

- cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
```
## shell variable
- 지역변수 : name=KIM
- 환경변수 : export name[=KIM]   : 자식 프로세스에게 복사되어 전달 됨
- variable substitution : $PATH

## command substitution
= $()
- day=`date +%d`  # bourne sh
- month=$(date +%m) # ksh

## Shell의 File Generation 특수 문자
- * : zero를 포함한 more character
- ? : any single character
- [...] : [...]에 나열된 single character

## Shell의 숨은 파일
- .bash_history : exit 할 때
- .bash_profile :
- .bashrc :
- .bash_logout : logout 시 실행하는 스크립트

## 리눅스의 종료와 재시작
1) 종료
- halt
- sutdown -h +10[0,now,hh:mm]
- init 0

2) 재시작
- reboot
- sutdown -r +10[0,now,hh:mm]
- init 6

## Run Level
```
# /etc/inittab
   0 - halt (Do NOT set initdefault to this)
   1 - Single user mode  <-- network 안됨
   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
   3 - Full multiuser mode
   4 - unused
   5 - X11 <-- GUI
```
```
- 런레벨별 실행, 중지되는 서비스 확인 K / S
# ls /etc/rc?.d
```
- POST : power on self test
- MBR(부트로더) GRUB
- Kernel load
- init process

## IP 고정하기
```
# cat /etc/sysconfig/network-scripts/ifcfg-eth0
# Advanced Micro Devices [AMD] 79c970 [PCnet32 LANCE]
DEVICE=eth0
BOOTPROTO=dhcp
ONBOOT=yes
HWADDR=00:0c:29:7c:ca:a6
```
```
# system-config-network
# service network restart
```

## 리눅스 시간
- 시스템 시간 : data
- H/W 시간 (CMOS 시간) : clock
- 두시간 동기화 :  clock -s(시스템시간 바꾸기) / -w(하드웨어시간 바꾸기)
- 시스템 시간 맞추기
```
# rdate -s time.bora.net  (-p print,  -s setting)
# date mmddHHMM[yy]
```

## Service
- service network restart # status, start, stop

## VI editor
### 비정상 종료시 복구방법
```
vi -r myfile
rm .myfile.swp
```
- VI 환경 설정
  - .exrc(Unix 호환)  / .vimrc
- Tips
  - 입력모드 : i, r, a, o
  - 단어변경 : cw
  - set number / set nonumber
  - set showmode   # 리눅스는 기본, -- INSERT --
  - set showmatch  # 괄호 매치
  - set ts=4

## MAN page 분류번호 : ex) LS(1)
- man 5 passwd
- man -a passwd
- Section 1 : 일반사용자 명령 ( /bin, /usr/bin )
- Section 8 : 관리자 명령 ( /sbin, /usr/sbin )
- Section 2 : System Call (Kernel 함수)
- Section 3 : Library Call ( /lib, /usr/lib )
- Section 5 : File Format ( /etc )

## Mount
- scsi : /dev/sda[a, b, c, d]
- IDE : /dev/hda[a, b, c, d]
- partition
```
----
sda1  <-- FS
----
sda2  <-- SWAP
----
sda3  <-- FS
----
# df
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/sda3             11165404   4003200   6585876  38% /
/dev/sda1               101086     18507     77360  20% /boot

# swapon -s
Filename                                Type            Size    Used    Priority
/dev/sda2                               partition       4096564 964     -1
```
## Command
```
- 일반명령 : /bin/  or /usr/bin
- 관리명령 : /sbin/ or /usr/sbin
- $PATH 에서 명령을 찾아서 실행
$ shutdown -h +10
-bash: shutdown: command not found
$ /sbin/shutdown -h +10
shutdown: you must be root to do that!
```
- id : 내 정보
- passwd : 패스워드 변경
- ipcs : ipc status
- whereis : 어디에 있나?
- pwd : 현재 디렉토리
- date : 시간 확인
- chsh : change shell
- touch : 0byte file 생성
- cat
- head
- tail
- more
- history: !number !character
- alias c=clear
- echo $hello <-- variable
- ps -f (현재 터미널에서 사용중인 프로세스)
- export : 지역변수 -> 환경변수
- env : 환경변수 보기
- set : 지역변수 + 환경변수 보기
- who : 누가 접속해 있나
- who -r : run level 확인
- tty : 접속한 가상 터미널 확인
- date : 시스템 시간 확인
- clock : hardware 시간 확인
- rdate : 시간 재 설정
- df : 파일시스템 확인
- swapon : swap 공간 확인
- free : 메모리 공간 확인