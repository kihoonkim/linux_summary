## VMWare deault Network
- host PC : 192.168.xxx.1
- Router : 192.168.xxx.2
- DNS : 192.168.xxx.2
- DHCP : 192.168.xxx.254

## Remote Login
- server : Telnet#23, SSH#22
- Client : Secure-CRT, Tera-Term, Netterm, Putty....
- client 설정 : /etc/ssh/ssh_config
- server 설정 : /etc/ssh/sshd_config
- ssh / sshd

## kernel 확인
```
- ls /boot
vmlinuz-2.6.18-164.el5
- uname -r
2.6.18-164.el5xen
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
- $()
- day=`date +%d`  # bourne sh
- month=$(date +%m) # ksh

## Shell의 File Generation 특수 문자
- \* : zero를 포함한 more character
- ? : any single character
- [...] : [...]에 나열된 single character

## PWD
- cd -     <-- 이전 디렉토리로 이동
- set | grep PWD
```
OLDPWD=/root
PWD=/etc/sysconfig/network-scripts
```

## Shell 이 로그인시 실행하는 일련의 스크립트
1. /etc/profile
2. ~/.bash_profile : User specific environment and startup programs
3. ~/.bashrc       : User specific aliases and functions
4. /etc/bashrc

## Shell의 숨은 파일
- /etc/skel 에서 계성 생성시 복사 됨 (bash_history 는 로그아웃 될때 생성)
- .bash_history : exit 할 때 메모리에 가지고 있는 history 정보 저장
- .bash_profile
- .bashrc
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

## GRUB
- /etc/grub.conf --> /boot/grub/grub.conf
- hiddenmenu
- timeout=5
- title
- splashimage=(hd0,0)/grub/splash.xpm.gz
- (hd0,0) : 첫번째 하드디스크의 첫번째 파티션 == /boot
- grub passwd 설정  
  ```
  # grub
  grub> md5crypt
  Password: ***
  Encrypted: $1$fOVnB/$Ke.EE9GeW/TuEKQeXYq1V.

  # vi /etc/grub.conf
  password --md5 $1$fOVnB/$Ke.EE9GeW/TuEKQeXYq1V.
  ```

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
- 비정상 종료시 복구방법
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

## Linux Directory
- Tree 모양의 계층적 구조
- / : 최상위 디렉토리
- /boot : 커널 관련
- /dev : 장치파일
- /bin : 명령어
- /sbin : 관리 명령어
- /lib : Library
- /usr : 추가적인..
- /root : root 계정 홈
- /home : user들의 홈
- /etc : 시스템 설정 파일
- /mnt : 새로 마운트 할 곳
- /media : 이동식 저장 장치를 시스템에서 auto mount
- /proc : process 관련된 정보 memory fs,   ls /proc/$$
- /sys : 하드웨어관련된 정보 memory fs
- /misc: NFS 관련
- /net : NFS 관련
- ...

## Mount
- scsi : /dev/sda[a, b, c, d]
- IDE : /dev/hda[a, b, c, d]
- partition  
  ```
  [DISK]
  -----
  |sda1  <-- FS : mounted  /boot
  -----
  |sda2  <-- SWAP
  -----
  |sda3  <-- FS : mounted  /
  -----
  # df
  Filesystem    1K-blocks     Used  Available  Use%  Mounted on
  /dev/sda3     11165404   4003200    6585876   38%  /
  /dev/sda1       101086     18507      77360   20%  /boot
  /dev/hdc       3754570   3754570          0  100%  /media/CentOS_6.8_Final

  # swapon -s
  Filename      Type        Size      Used    Priority
  /dev/sda2     partition   4096564    964      -1
  ```
- \# umount /boot or [/dev/sda1]
- \# mount /dev/sda1 /boot
- /etc/fstab
- /mnt : 새로 마운트 할 곳
- fdisk -> mkfs -> mount
- 리눅스에 따라서 CD/DVD 장치 명이 다를 수 있음 : ls /dev/cdrom

## 사용자와 그룹
- useradd  -u(ID) -g(group) -d(home) -s(shell) : /etc/default/useradd
- usermod
- userdel
- groupadd
- groupmod
- groupdel
- Primary Group : -g ,로그인 했을 때 기본 속한 그룹 /  Secondary Group : -G
- newgrp (group) : 그룹 변경    <-> exit
- passwd -l username : lock
- passwd -u username : unlock
- passwd -d username : delete password
- pwconv / pwunconv : shadow file 생성 삭제
- /etc/passwd : 계정 정보, Primary Group 정보
  ```
  계정:패스워드:UID:GID:GECOS:디렉토리:쉘
  user:x:500:500::/home/user:/bin/bash
  ```
- /etc/shadow : 비밀번호 정보
  ```
  계정:암호화된패스워드:암호변경일:암호사용최소일자:암호사용최대일자:
  user:$1$R0qq4D8J$HM2Eq9PQwP.d/kL2ZhRRz/:17213:0:99999:7:::
  ```
- /etc/group : 그룹 정보, 유저의 Secondary Group 정보
  ```
  user:x:500:
  ```

## File 유형
- \- : file
- d : directory
- b: block device
- c : character device
- l : link
- s : socket
- p : pipe

#### Socket
- 양방향
- 인터넷 소켓 : 네트워크 통신용 ==> netstat 명령으로 확인
- 유닉스 소켓 : IPC용

#### Pipe
- 단방향
- Anonymous pipe : ps -e | grep init
- Named Pipe : mkfifo 명령으로 생성

## File Permission
- user  group  other
-  rwx    rwx    rwx
- chmod 777 file
- chmod 7777 file
- chown username[.groupname] file
- chgrp groupname file

```
type        u    g    o
[0000][000][000][000][000]
       set user id bit  : rws : owner uid 권한으로 실행 가능
       set group id bit : rws : owner gid 권한으로 실행 가능
       sticky bit       : rwt : swap 공간에 남아 있음. root 만 설정 가능

$ ll /usr/bin/chsh
-rws--x--x 1 root root 19096  9월  4  2009 /usr/bin/chsh
$ chsh  -> /etc/passwd 수정해야 됨
[ps table]
* set user id bit == 1
real user id : 일반 유저           : uid
effective user id : 권한 체크 유저  : owner uid
* set group id bit == 1
real group id : 일반 그룹           : uid
effective group id : 권한 체크 그룹 : owner gid

$ ll -d /tmp
drwxrwxrwt 12 root root 4096  2월 21 15:38 /tmp
```
## Switch User
- su : root는 생략되어 실행
- su username : 계정만 바꿈, bash 나 PATH 등은 유지
- su \- username : root 로 로그인 한 것과 동일
- su -l username : 위와 동일
- sudo : root의 제한된 명령을 일반 user에게 허용하기 위해.. ()/etc/sudoers)

## 링크
- ls -i : inode 번호 까지
- ls -l : inode 에 있는 정보까지 같이 보여 줌
```
[Filesystem]
| SB |   INode Block    |     Data Block    |
                               |-> 1000 | fileA
```
- Hard Link : ln
```
$ vi fileA
$ ln fileA fileA.ln
$ ls -il
229923 -rw-r----- 2 root root 12  2월 21 17:05 fileA
229923 -rw-r----- 2 root root 12  2월 21 17:05 fileA.ln

==> inode#, file type, link count, file size 가 동일 함
==> INode block을 공유
==> data를 변경하더라도 둘이 동일. Data block은 동일하게 사용하기 때문에
```
- Symbolic Link : ln -s

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
- cat / head / tail / more / less : 파일 보기
- file : 어떤 종류의 파일인지 보기
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
- mount : 마운트, 장치를 특정 디렉토리에 연결
- umount : 언마운트
- eject : 언마운트 + 장치 제거
