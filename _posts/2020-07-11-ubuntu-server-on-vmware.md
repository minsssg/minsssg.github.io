---
title: VMware에 Ubuntu Server 설치하기
excerpt: VMware workstation player를 설치하고 그 위에 Ubuntu server 20.04.3 LTS를 설치한다.
categories:
  - Linux
tags:
  - Linux
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2020-07-11T13:00-18:00
permalink: ubuntu_server_on_vmware.html
---
# VMWare에 Ubuntu Server 설치하기

그 동안 컴퓨터공학을 공부했는데 웹 언어에 대해 너무 몰랐다. 앞으로 웹 언어에 대해서 공부를 해보려 한다.

그래서 html, css, javascript 등 프론트엔드와 node.js를 공부하고자 한다. 그래서 우선 linux 운영체제에서 개발을 해보고 싶어 VMWare를 설치하고 Ubuntu Server를 설치해서 운영하려 한다.

## 1. VMWare Player 설치하기

VMWare(Virtual Machine)는 가상화를 통해 PC위에 가상머신을 돌려서 PC안의 또 다른 PC를 사용하게 해준다. 정말 신기한 기술이다. 나의 컴퓨터의 운영체제는 Windows인데 가상머신을 설치해서 Linux 컴퓨터를 설치하려 한다.

VMware에서는 유료버전인 **VMware Workstation Pro**와 무료 버전인 **VMware Workstation Player**이다. 당연히 유료 버전과 무료 버전 차이가 기능 및 성능의 차이가 있지만, 일단 무료 버전을 써보자.

VMware workstation player를 설치해보자.

1. [VMware 한국 사이트](https://www.vmware.com/kr.html)에 들어간다.
2. 다운로드에서 무료 제품 다운로드로 들어간다.
3. 그러면 VMware Workstation Player가 있다.

VMware Workstation Player 사진

![image-20200710224956596](/assets/images/ubuntu_on_vmware/vmware_download_image.png)

일단 필자는 설치할 때 다른 옵션을 주지 않고 Next만 눌렀다.

## 2. Ubuntu Server 이미지 다운로드

VMware를 설치했으니 VMware에서 작동시킬 운영체제를 다운 받자. Linux 환경에서 개발하고 싶어서 Ubuntu Server를 다운받는다.

**Ubuntu Server** 이미지는 [ubuntu사이트](https://ubuntu.com/download/server)에서 무료로 받을 수 있다. 근데 벌써 Ubuntu Server 20.04 LTS라니..., 시간 참 빠른 것 같다.

![우분투서버다운로드](/assets/images/ubuntu_on_vmware/ubuntu_server_download.PNG)

그리고 Download를 눌러서 다운받자.

## 3. VMware Setting

이제 우분투 iso파일도 생겼으니 이제 VMware에 설치해보자.

1. [Create a New Virtual Machine]을 클릭한다.

   ![VMware실행사진](/assets/images/ubuntu_on_vmware/VMware_run.PNG)

2.  Browse를 눌러서 설치한 iso파일을 불러온다.

   ![iso_import](/assets/images/ubuntu_on_vmware/iso_import.PNG)

   ![iso_imported](/assets/images/ubuntu_on_vmware/iso_imported.PNG)

2. 사용자 이름을 작성한다. 나는 ubuntu로 했음.

   ![ubuntu_install_01](/assets/images/ubuntu_on_vmware/ubuntu_install_01.PNG)

3. vmware설치 장소를 정한다.

   ![image-20200710231428992컴퓨터 하드 디스크(SSD) 사용량 설정을 하게 된다. 솔직히 얼만큼 잡아야 될지 몰라서 일단 구글링 해보니까, 20GB 그대로 해도 되고 더해도 된단다. 그리고 recommend를 하고 있으니 그대로 쓰자. 나중에 변경하니까!

4. ![disk_setting](/assets/images/ubuntu_on_vmware/disk_setting.PNG)

   그리고 옵션 하나가 있는 데, [Store virtual disk as a single file], [Split virtual disk into multiple files] 두 가지 세팅 있다.

   **Store virtual disk as a single file**: 가상 머신의 데이터를 하나의 파일에 저장하는 것으로 NTFS 포맷으로 사용하는 경우 선택하면 된단다.

   **Split virtual disk into multiple files**: 가상 머신 디스크 파일을 2GB 씩 분할하여 저장하는 방식이다. 큰 디스크를 사용할 때 성능 저하를 일으킨다고 한다. 파일시스템 FAT32 포맷이라면 권장한다고 한다.

   결론적으로 나는 **Store virtual disk as a single file**을 선택했다.

5. 이제 Customize Hardware에서 원하는 세팅을 해주면 된다. 나는 메모리 사용량만 조금 더 늘렸다. (8GB)

   ![memory_setting](/assets\images\ubuntu_on_vmware\memory_setting.PNG)

이제부터는 ubuntu server 20.04 LTS를 설치해보자.

## 4. VMware in ubuntu server

VMware의 세팅이 끝났고, 3번의 마지막 사진에서 Finish 버튼을 누르면 VMware가 작동한다. 그럼 다음 그림이 나타난다.

1. Language Setting, 한국어가 없기 때문에 그냥 "English"를 선택한다.

   ![ubuntu_install_02](/assets/images/ubuntu_on_vmware/ubuntu_install_02.PNG)

2. 나는 ubuntu server "20.04.3" 버전을 사용하는 데 "20.06.1"버전으로 업데이트해서 사용하겠느냐고 묻는 창이 나온다. 하지만 "20.04.3"버전은 **LTS(Long Term Support)**버전으로 오래동안 쓸 수 있는 장점이 있다. 그래서 그냥 넘어갔다.

   ![ubuntu_install_03](/assets/images/ubuntu_on_vmware/ubuntu_install_03.PNG)

3. 다음으로 키보드 레이아웃을 설정한다. 나는 둘 다 "Korean"으로 설정했다.

   ![ubuntu_install_04](/assets/images/ubuntu_on_vmware/ubuntu_install_04.PNG)

4. 다음은 네트워크 설정으로 그냥 설정된 값으로 하고 넘어갔다. 네트워크 부분에선 아직 자세히 모르겠다.

   ![ubuntu_install_05](/assets/images/ubuntu_on_vmware/ubuntu_install_05.PNG)

5. proxy도 설정하는 부분도 역시 그냥 넘어갔다.

   ![ubuntu_install_06](/assets/images/ubuntu_on_vmware/ubuntu_install_06.PNG)

6. 우분투를 설치하기 위한 데이터를 받기 위해 mirror archive를 선택해야 한다. 나는 지역상 "kr.archive.ubuntu.com/ubuntu"를 선택했다. 위치상 데이터를 내려받는 시간이 가까울수록 짧기 떄문이다.

   ![ubuntu_install_07](/assets/images/ubuntu_on_vmware/ubuntu_install_07.PNG)

7. 다음으로 디스크 파트이다. 이 부분 역시 자세히 몰라 구글링을 해보다가 그냥 다음으로 넘어갔다.

   ![ubuntu_install_08](/assets/images/ubuntu_on_vmware/ubuntu_install_08.PNG)

8. 이제 파티션 부분이다. 현재 20GB 밖에 마운트하지 않아서 그냥 Default로 하고 넘어갔다.

   ![ubuntu_install_09](/assets/images/ubuntu_on_vmware/ubuntu_install_09.PNG)

9. 이제 디스크를 마운트 시키고 우분투 설치를 시작한다. continue 시작!! 끝이 보이기 시작한다.

   ![ubuntu_install_10](/assets/images/ubuntu_on_vmware/ubuntu_install_10.PNG)

10. continue 누르면 바로 시작할 줄 알았는데 아직 아니다. ㅜㅜ 우분투 설치는 어렵다. 아래 그림과 같이 나온다.

    ![ubuntu_install_11](/assets/images/ubuntu_on_vmware/ubuntu_install_11.PNG)

    * Your_name: 그냥 내 이름 썼다. 아무이름 써도 상관 없다
    * Your_server's_name: 서버명, 그냥 ubuntu_server라고 씀
    * Pick a username: 우분투가 설치되고 첫 계정이 되는 아이디이다. 기억해야 된다.
    * Choose a password: 첫 계정의 비밀번호가 되는 것이다. 꼭 기억하자! 까먹으면 귀찮아진다.ㅜㅜ

11. OpenSSH 는 SSH로 서버에 접근할 수 있는 라이브러리를 설치하는 것인데, 이것은 우분투가 설치되고 나서도 할 수 있어서 설치하지 않고 넘어갔다.

    ![ubuntu_install_12](/assets/images/ubuntu_on_vmware/ubuntu_install_12.PNG)

12. 이제 내가 ubuntu server에서 사용할 서버 목록이다. 뭐가 뭔지 몰라서 그냥 넘어갔다.

    ![ubuntu_install_13](/assets/images/ubuntu_on_vmware/ubuntu_install_13.PNG)

13. 드디어!!! 설치시작!!!

    ![ubuntu_install_14](/assets/images/ubuntu_on_vmware/ubuntu_install_14.PNG)

    "View full log"에 들어가면 어떤 데이터를 받고 있는 볼 수 있다. 근데 봐도 뭐가 뭔지 모르겠다.

    ![ubuntu_install_14_02](/assets/images/ubuntu_on_vmware/ubuntu_install_14_02.PNG)

14. 설치가 끝났다. 이제 "Reboot"을 실행해 우분투 서버가 잘 되는지 보자!!!

    ![ubuntu_install_15](/assets/images/ubuntu_on_vmware/ubuntu_install_15.PNG)

15. 우분투 설치가 끝나고 간단한 명령어를 실행해 보았다. 잘된다. ㅜㅜㅜㅜ 감격이다.

    ![ubuntu_install_finish](/assets/images/ubuntu_on_vmware/ubuntu_install_finish.PNG)

## 4. 결론

Linux에서 개발한 경험이 많지 않다. 웹에 대한 공부를 하기 위해 VMware, ubuntu server를 설치해보았다. 인터넷이 되는지 확인을 했고, 디스크 마운트도 잘 되었다. 그런데 막상 VMware에 우분투를 설치했는데, 글자도 작고 보기도 불편하다. 그래서 OpenSSH를 설치하고 SSH Client를 설치해서 우분투 서버에 들어가는 것을 다음 시간에 해보도록 하겠다. 

역시 컴퓨터는 환경세팅하는데 시간이 많이 걸린다. 학생 때 많은 경험을 하지 못해 아쉽다.... ~~에휴~~~