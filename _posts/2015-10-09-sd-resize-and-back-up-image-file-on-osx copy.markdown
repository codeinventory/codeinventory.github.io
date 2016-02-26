---
layout: post
title:  "SD Resize and Back up Image File on OSX"
date:   2015-10-09 07:40:00
categories: servers
---

[라즈베리파이][raapberrypi-link]를 사용하다 보면 Image를 Backup/Restore 할 일이 발생한다.
[산딸기마을][rasplay-link]에서는 `멀티제어RCCar` 이미지 라던지, `PiSnap` 이미지라던지. 이들의 배포를 하기위해서도 Backup이 필요하다.
Backup을 처음 했던 때 부터 생각했던 문제였지만, Backup된 Image의 Size는 무엇인가 불만스러움이 있었다.
본인은 혹시 모를 용량 부족을 예방하기위해 32G의 SD를 사용하지만, 실제 배포를 위한 사용량은 4G에 불과하다.
4G를 Restore하기 위해 32G Backup 파일을 Download/Restore 한다면, 32/4 배의 시간을 낭비하는 것.
아마도 몇년간 가진 모임 참석자들은 이 문제에 대해서 불만을 토로하고 있을 것이다.

Windows사용자는 [Win32 Disk Imager][win32diskimager-link]를 사용해서 Backup/Restore를 할 것이고, OSX사용자를 위해서는 [ApplePi-Baker][applepi-baker-link]라는 훌륭한 GUI Tool 을 사용할 것이다.
![Win32 Disk Imager][win32diskimager-img]
![ApplePi-Baker][applepi-baker-img]
또는 `dd`명령어도 잘 사용하여 CLI상에서 처리하는 사용자도 있을 것이다. 
아무튼, 몇번이고 찾고, 몇번이고 시도했지만 실패했던 문제가 아주 쉬운 방법으로 해결되었다.
본격적으로 `Mac OSX에서 Command 로 SD Card의 Backup image를 줄이는 방법`에 대해서 알아보자.
열심히 구글링 한 결과, 아래의 링크들을 통해 해결을 볼 수 있었다.
[http://www.htpcguides.com/easy-resize-and-back-up-raspberry-pi-sd-card-with-ubuntu/][resize-sd-ubuntu-gui-link]
[http://askubuntu.com/questions/390769/how-do-i-resize-partitions-using-command-line-without-using-a-gui-on-a-server][resize-sd-ubuntu-cli-link]

### How to on ubuntu ?
ubuntu에서는 [Gparted][gparted-link]를 사용하여 GUI환경으로 resize를 해결할 수 있었다. 
[Easy Resize and Back up Raspberry Pi SD Card with Ubuntu][resize-sd-ubuntu-link]

### Install ext partition utility on OSX
알다시피 라즈베리파이SD에는 두개의 partitions으로 되어있다.
`vfat`과 `ext4`으로, 실제 부팅 partition인 ext4를 OSX에서 인식하기 위해서는 추가 S/W가 필요하다.
이를 해결하기 위해 [E2fsprogs: Ext2/3/4 Filesystem Utilities][e2fsprogs-link]를 설치하자.
```
$ brew install e2fsprogs
```

[Homebrew][Homebrew-link]가 설치되지 않아 `brew`명령어가 깔려있지 않다면, [http://brew.sh][Homebrew-link]를 참고하기 바란다.

### Resize SD Card

저장 위치를 알지 못해서 검색해보았다.
```
$ sudo find / -name e2fsprogs
```

`/usr/local/opt/e2fsprogs`에 설치되었으니 이동하여 확인해보자.

```
$ cd /usr/local/opt/e2fsprogs
$ ls
COPYING			bin			lib
INSTALL_RECEIPT.json	etc			sbin
README			include			share
$ cd sbin
$ ls
badblocks	e2image		fsck.ext2	mkfs.ext2	tune2fs
blkid		e2label		fsck.ext3	mkfs.ext3	uuidd
debugfs		e2undo		fsck.ext4	mkfs.ext4
dumpe2fs	filefrag	fsck.ext4dev	mkfs.ext4dev
e2freefrag	findfs		logsave		mklost+found
e2fsck		fsck		mke2fs		resize2fs
```
`resize2fs`명령어가 보인다. 
실행시켜보니 실행은 안되고 `e2fsck -f`를 실행하라고 친절하게 알려준다.
순진하게 그대로 따라한다.
```
$ sudo ./e2fsck -f /dev/disk2s2
```
`/dev/disk2s2`는 resize 하고자 하는 ext partition이다.
라즈베리파이의 root partition을 resize하고 있는 중이다.
완료되고 나서 resize2fs를 실행하자.
```
$ sudo ./resize2fs /dev/disk2s2 4G
```
partition을 4G로 resize하였다. 
위에 언급한 ubuntu에서는 최소 용량을 가이드해 주는데, 여기서는 알아낼 방법을 모르니 그냥 4G로 넘겨짚었다. 
용량을 모른다면 알고 나서 진행하도록 하자. `실제사용량 +200M` 가 안전할 듯 하다.
그리고, 4.2G일때는 4200M로 입력해야 된다.

### Backup SD Card

이제 Resize 완료된 SD Card를 .img 파일로 Backup 해 보자.
시간이 남아 돌거나, 해보지 않았다면 [Win32 Disk Imager][win32diskimager-link], [ApplePi-Baker][applepi-baker-link]를 이용해서 Backup해 보라.
SD Card 용량이 고스란히 Backup 될 것이다. 32G SD라면 .img의 용량은 32G가 될 것이고, 시간은 1시간이 걸릴지 모른다.

아래의 명령어로 Backup해 보자. OSX나 Linux등에서 사용가능한 명령어다.
```
$ sudo dd if=/dev/disk2 of=/home/rasplay/raspberrypibackup.img bs=1m count=4800
```

이제 라즈베리파이에서 돌려보자.
잘 돌아가는지 모르겠다.

[raapberrypi-link]:			http://www.raspberrypi.org
[rasplay-link]:				http://www.rasplay.org
[win32diskimager-link]:     http://sourceforge.net/projects/win32diskimager/
[win32diskimager-img]:      http://i1.wp.com/www.rasplay.org/wp-content/uploads/win32_1.jpg
[applepi-baker-link]:		http://www.tweaking4all.com/hardware/raspberry-pi/macosx-apple-pi-baker/
[applepi-baker-img]:		http://www.tweaking4all.com/wp-content/uploads/2014/01/applepi-baker-restore-with-compression.jpg
[gparted-link]:				http://gparted.org
[resize-sd-ubuntu-gui-link]: 	http://www.htpcguides.com/easy-resize-and-back-up-raspberry-pi-sd-card-with-ubuntu/
[resize-sd-ubuntu-cli-link]:	[http://askubuntu.com/questions/390769/how-do-i-resize-partitions-using-command-line-without-using-a-gui-on-a-server]
[e2fsprogs-link]:			http://e2fsprogs.sourceforge.net
[Homebrew-link]:			http://brew.sh


