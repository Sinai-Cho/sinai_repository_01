# sinai_repository_01
For github &amp; codyssey connecting
## 1st 과제 : 내 컴퓨터에 개발자용 '작업실' 꾸미기
## 프로젝트 개요(과제 목표)
- GitHub 연동을 통해 다른 사람과 작업 진행 상황을 공유할 수 있도록 한다.
- docker를 사용하여 한 컴퓨터 내에서 각기 다른 환경에서 서비스 및 프로그램을 개발할 수 있는 지식을 갖춘다.
- Dockerfile과 포트 매핑 및 바인드마운팅 기능을 사용해서 서비스 내용의 변경이 있거나 삭제되는 상황이 발생하더라도 변경된 내용의 즉각 반영 및 기존 서비스 안정적으로 제공할 수 있는 환경 구축에 대해 학습한다. 

## 1. 실행환경
- OS : macOS
- Shell : zsh
- Docker : 28.5.2
- Git : git version 2.53.0
  
## 2. 수행 항목 체크리스트
### 1) 터미널로 작업 디렉토리와 권한 정리 
(https://github.com/Sinai-Cho/sinai_repository_01/issues/6#issue-5036660240)
- 작업용 디렉토리 생성 : mkdir ~/task01_webserver
- 생상한 디렉토리로 이동 : cd ~/task01_webserver
- 테스트용 빈 파일 생성 : touch test01.txt
- 파일 내용 확인 : cat test01.txt
- 파일 복사 : cp test01.txt test02.txt
- 파일 이동 : mv test02.txt src/
- 파일 이름 변경 : mv test02.txt test03.txt
- 현재 위치 확인 : pwd
- 디렉토리 내에서 목록 확인 : ls src/
- 파일 권한 확인 : ls -l test03.txt
  * 결과 : -rw-r--r--  1 sinai4867038  sinai4867038  0  8  1 19:31 test03.txt
- 파일 권한 변경 : chmod 755 test03.txt
  * 결과 : -rwxr-xr-x  1 sinai4867038  sinai4867038  0  8  1 19:31 test03.txt
- 파일 삭제 : rm test03.txt
- 디렉토리 권한 정리 : chmod 755 ~/task01_webserver 
  (755 : 소유자는 모든 권한, 다른 사람은 읽기/실행만 가능)
- 디렉토리의 권한 확인 : ls -ld ~/task01_webserver 
  (ls -ld : 폴더 자체의 정보와 권한을 보여줌)
- 권환 확인 결과와 설명 :  
  drwxr-xr-x  2 sinai4867038  sinai4867038  64  8  1 17:06 /Users/sinai4867038/task01_webserver
  * 결과 설명 
  drwxr-xr-x → 권한 (d = 디렉토리, rwx = 읽기/쓰기/실행)
  2 → 링크 수
  sinai4867038 → 소유자
  sinai4867038 → 그룹
  64 → 디렉토리 크기(바이트 단위)
  8  1 15:06 → 마지막 수정 시간 (8월 1일 17시 06분)
  /Users/sinai4867038/task01_webserver → 디렉토리 경로
- 디렉토리 권한 수정 : chmod 700 ~/task01_webserver (원래 권한은 755)
- 디렉토리 권한 수정 결과 : drwx------  4 sinai4867038  sinai4867038  128  8  1 19:38 .
### 2) docker 설치, 점검 및 컨테이너 실행/관리
- docker 설치 : brew install --cask docker
- 권한 문제로 설치 실패(https://github.com/Sinai-Cho/sinai_repository_01/issues/1#issue-5036160924)
- 해결 방법 : 맥북 내 Orbstatk 앱 활성화 후 터미널에서 docker version 명령어를 사용해 사용가능한 상태 및 버전 확인 완료
### 3) Dockerfile 작업
- 현재 디렉토리 안에 하위 디렉토리 생성 : mkdir src
- 웹서버용 파일 생성 : echo "<h1>Hello Wolrd</h1>" > src/index.html
- Dockerfile 만들기 : \
  echo "FROM nginx:latest" > Dockerfile (첫줄은 반드시 각진 괄호 1개. Dockerfile에 괄호 왼쪽 내용을 넣는다는 뜻) \
  echo "COPY src/index.html /usr/share/nginx/html/index.html" >> Dockerfile \
  echo "EXPOSE 80" >> Dockerfile (괄호가 2개면 앞 내용에 이어쓴다는 의미. 1개면 덮어쓴다는 의미. 첫줄만 괄호 1개여야 한다) 
- Dockerfile 에 내용이 제대로 들어갔는지 확인 : cat Dockerfile
  * 결과

          FROM nginx:latest
 
          COPY src/index.html /usr/share/nginx/html/index.html
    
          EXPOSE 80
### 4) 포트 매핑 및 바인드 마운트 작업
- 포트 매핑 : 컨테이너가 정상적으로 실행되고 웹페이지가 보이는지 확인하는 단계
- 바인드 마운트 : 호스트의 파일을 컨테이너와 직접 공유하는 작업. 파일을 수정할 경우 바로 컨테이너에 반영되므로 개발 단계에서 확인 작업에 사용하면 좋음

- docker build : docker build -t task01_server . (https://github.com/Sinai-Cho/sinai_repository_01/issues/3#issue-5036423712)
- 결과 로그 : \
  docker build -t task01_server .
[+] Building 7.6s (7/7) FINISHED                                docker:orbstack
 => [internal] load build definition from Dockerfile                       0.2s
 => => transferring dockerfile: 118B                                       0.0s
 => [internal] load metadata for docker.io/library/nginx:latest            2.3s
 => [internal] load .dockerignore                                          0.1s
 => => transferring context: 2B                                            0.0s
 => [internal] load build context                                          0.2s
 => => transferring context: 89B                                           0.0s
 => [1/2] FROM docker.io/library/nginx:latest@sha256:5a88c9c45479443d7be2  4.0s
 => => resolve docker.io/library/nginx:latest@sha256:5a88c9c45479443d7be2  0.2s
 => => sha256:5a88c9c45479443d7be2eadc894b4ed0a9801bae0 10.23kB / 10.23kB  0.0s
 => => sha256:db4f612f385437d11eb26620a4f1d7efb3ff44e1296 2.29kB / 2.29kB  0.0s
 => => sha256:062e450697faa5f02a3a74eba9864ee4d79bc9cfb 29.78MB / 29.78MB  0.6s
 => => sha256:4e5db4761e0ff445f7fd29aad680ad28e8abf7d2048 9.09kB / 9.09kB  0.0s
 => => sha256:3c7ab7949321f47c96fc0918f9f72e8f51bd452cdef1e0d 626B / 626B  0.6s
 => => sha256:82454cdbf456a77f9ff1bb88b121c2a739e38c30e 33.33MB / 33.33MB  0.9s
 => => extracting sha256:062e450697faa5f02a3a74eba9864ee4d79bc9cfbd65769f  1.1s
 => => sha256:cacfcdd01f309c65d69372716e799ea741065ac1b1e6088 955B / 955B  0.9s
 => => sha256:b6698f04e005497a7f495c0719358d43890cb3997ad7b4a 403B / 403B  0.9s
 => => sha256:2bedaf25031a24fb70b9dc2d56cb17139186d1ae5fd 1.21kB / 1.21kB  1.1s
 => => sha256:d26f27cc8c41e321394cb3c9a80915d90d5f1f1d3cb 1.40kB / 1.40kB  1.2s
 => => extracting sha256:82454cdbf456a77f9ff1bb88b121c2a739e38c30ea689c13  0.7s
 => => extracting sha256:3c7ab7949321f47c96fc0918f9f72e8f51bd452cdef1e0da  0.0s
 => => extracting sha256:cacfcdd01f309c65d69372716e799ea741065ac1b1e60880  0.0s
 => => extracting sha256:b6698f04e005497a7f495c0719358d43890cb3997ad7b4ab  0.0s
 => => extracting sha256:2bedaf25031a24fb70b9dc2d56cb17139186d1ae5fd2054e  0.0s
 => => extracting sha256:d26f27cc8c41e321394cb3c9a80915d90d5f1f1d3cbbbcda  0.0s
 => [2/2] COPY src/index.html /usr/share/nginx/html/index.html             0.4s
 => exporting to image                                                     0.2s
 => => exporting layers                                                    0.1s
 => => writing image sha256:66472bf49838d78b0e0f70d718f0ba5a3ae73a908c14b  0.0s
 => => naming to docker.io/library/task01_server                           0.0s
- docker 포트 매핑 및 바인드 마운트 작업 :\
  docker run -d -p 8080:80 --name con_number01 -v $(pwd)/src:/usr/share/nginx/html task01_server
  * 결과 : b60157574a96c77404dec2760cfa6302f0222ca864028d904572d9d92a3f0a53(
  * -d는 뒤에서 작업하겠다는 의미, -p는 포트 매핑(맥북의 8080포트와 dockerfile에서 설정한 80포트를 연결), --name은 컨테이너 이름, -v는 src 디렉토리 내의 파일과 바인드마운팅 한다는 의미, task01_server 는 docker가 생성한 이미지 이름
- 포트매핑 확인 : http://localhost:8080/ 접속 (https://github.com/Sinai-Cho/sinai_repository_01/issues/4#issue-5036490610)
- 바인드마운트 확인 위해 index.html 파일 내용 변경 : echo '<h1>Goodbye World</h1>' >src/index.html 
- 파일 변경 내용 확인 : cat src/index.html
- 변경 내용 적용 확인 : http://localhost:8080/ 새로 고침 (https://github.com/Sinai-Cho/sinai_repository_01/issues/5#issue-5036520878)



- 컨테이너 삭제 전후로 데이터 확인해서 데이터 유지됨 확인
- 
### 5) 볼륨 작업 (volume : docker가 관리하는 저장공간)
- docker 볼륨 생성 : docker volume create con_01_volume
- 결과 예시 : con_01_volume
- 생성된 볼륨 확인 명령어 : docker volume ls
- 실행 결과 :
DRIVER    VOLUME NAME
local     con_01_volume
- 볼륨 정보 확인 명령어 :docker volume inspect con_01_volume
- 실행 결과 : 
[
  {
     "CreatedAt": "2026-08-05T20:46:31+09:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/con_01_volume/_data",
        "Name": "con_01_volume",
        "Options": null,
        "Scope": "local"
  }

]
- 볼륨 연결하여 컨테이너 실행 명령어 : 
docker run -d --name con_numver02 -p 8081:80 -v con_01_volume:/usr/share/nginx/html task01_server
- 실행 결과 :  
72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c
- 연결 확인 명령어 : docker inspect con_numver02
- 실형 결과 :
[
    {
        "Id": "72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c",
        "Created": "2026-08-05T12:00:44.621355675Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 414,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2026-08-05T12:00:44.834451584Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:48194f0cd9d79b5a3a4cc7cf9e1aee051565addc2c0b348af441c589be67a0e3",
        "ResolvConfPath": "/var/lib/docker/containers/72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c/hostname",
        "HostsPath": "/var/lib/docker/containers/72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c/hosts",
        "LogPath": "/var/lib/docker/containers/72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c/72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c-json.log",
        "Name": "/con_numver02",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "con01_volume:/usr/share/nginx/html"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {
                    "max-file": "5",
                    "max-size": "20m"
                }
            },
            "NetworkMode": "bridge",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "8081"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                24,
                80
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 8413773824,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/interrupts",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "ID": "72a676530fd07bf96135b7d661dc92c8c7b7206441b6b38e0771b6b22ea9959c",
                "LowerDir": "/var/lib/docker/overlay2/88f829e0c63039972a4a4a51f0128416271b18de7ff19df9b28eac00ea7d1178-init/diff:/var/lib/docker/overlay2/rklanam2z36922gjn7ygtgrrl/diff:/var/lib/docker/overlay2/0773b48915fa12982e414ed31a902b89b81a03087b7c85cd3c622c5dfad89e71/diff:/var/lib/docker/overlay2/48c016e4b3f4c036a28bfae639eda05ecc2a6bff8dba5a1f8b068d192ae08be7/diff:/var/lib/docker/overlay2/381bcc039218162d909166118b920541d9c4e086ce2c28b27f76c15640793a01/diff:/var/lib/docker/overlay2/57fbe133aca281bdc01a3d0fe49190bb30ae5ed4bc8e214e153fea6ba6083ad9/diff:/var/lib/docker/overlay2/6a10a512ad6b121ebd6c9273133788efe823a3864b31481af5e33fcc2a3b705c/diff:/var/lib/docker/overlay2/1ae31cff2a28de1cde04fbcac630c08a1ac984319a7f6e3d1709fa6d51456bb5/diff:/var/lib/docker/overlay2/e6ecfe493b7861d6bee8739fba0393b9f2e99c1e71002ca2b389a7f899400724/diff",
                "MergedDir": "/var/lib/docker/overlay2/88f829e0c63039972a4a4a51f0128416271b18de7ff19df9b28eac00ea7d1178/merged",
                "UpperDir": "/var/lib/docker/overlay2/88f829e0c63039972a4a4a51f0128416271b18de7ff19df9b28eac00ea7d1178/diff",
                "WorkDir": "/var/lib/docker/overlay2/88f829e0c63039972a4a4a51f0128416271b18de7ff19df9b28eac00ea7d1178/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "con01_volume",
                "Source": "/var/lib/docker/volumes/con01_volume/_data",
                "Destination": "/usr/share/nginx/html",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "72a676530fd0",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.31.3",
                "NJS_VERSION=1.0.0",
                "NJS_RELEASE=1~trixie",
                "ACME_VERSION=0.4.1",
                "PKG_RELEASE=1~trixie",
                "DYNPKG_RELEASE=1~trixie"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "task01_server",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "42e3c9d4df7bca290e6a9f2964f4c08dc6dfec45c728c1380955388362943124",
            "SandboxKey": "/var/run/docker/netns/42e3c9d4df7b",
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "8081"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "8081"
                    }
                ]
            },
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "3b3f0d4abdffce2a30d548082f1b99e130c0b61ef1b88e3c9df42f4450c1facd",
            "Gateway": "192.168.215.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "192.168.215.3",
            "IPPrefixLen": 24,
            "IPv6Gateway": "",
            "MacAddress": "5e:18:77:b4:b7:84",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "MacAddress": "5e:18:77:b4:b7:84",
                    "DriverOpts": null,
                    "GwPriority": 0,
                    "NetworkID": "a77c1de4a9ffdffb26e70660fe6bf8c3e652f10437a84546bdacfd42d01b0870",
                    "EndpointID": "3b3f0d4abdffce2a30d548082f1b99e130c0b61ef1b88e3c9df42f4450c1facd",
                    "Gateway": "192.168.215.1",
                    "IPAddress": "192.168.215.3",
                    "IPPrefixLen": 24,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        }
    }
]
- 컨테이너 내부 확인 : docker exec con_numver02 ls -l /usr/share/nginx/html
- 실행 결과 :
total 8
-rw-r--r-- 1 root root 497 Jul 15 16:03 50x.html
-rw-r--r-- 1 root root  12 Aug  5 11:34 index.html
- 컨테이너 삭제 : docker rm -f con_numver02
- 새 컨테이너 생성 및 같은 볼륨 연결 : docker run -d --name con_number03 -p 8082:80 -v con_01_volume:/usr/share/mnginx/html task01_server
- 실행 결과 :
80d426a6e000d2812cfa09cf0cc4a2fa27dfd75884e2175103466eec199e514a
- 새로 생성한 컨테이너 내부 확인 : docker exec con_number03 ls -l /usr/share/nginx/html
- 실행 결과 :
total 8
-rw-r--r-- 1 root root 497 Jul 15 16:03 50x.html
-rw-r--r-- 1 root root  12 Aug  5 11:34 index.html
- 새로 생성한 컨테이너 내부 파일 내용 확인 : docker exec con_number03 cat /usr/share/nginx/html/index.html
- 실행 결과 :
Hello World
### 6) 컨테이너 테스트
1. 컨테이너 실행/중지/목록 확인( ps-a) 
- 우분투 컨테이너 실행, 내부 진입 후 간단 명령(ls 수행 결과 기록)
- 컨테이너 종료/유지(attach/exec 의 차이) 관찰해서 정리
- sinai4867038@c6r1s1 task01_webserver % cd
- 우분투 컨테이너 실행 : docker pull ubuntu
- 결과 :
Using default tag: latest
latest: Pulling from library/ubuntu
617772c7d19b: Pull complete 
a7fb98a8eddd: Pull complete 
Digest: sha256:678c6550cc43645e08669028bc177f50be4e7c5b8cca677067b1914d4afc7a03
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
- 컨테이너 실행 : docker run -it --name ubuntu_test ubuntu
- 결과 : root@4dbdc0e40fef:
- 컨테이너 내부 진입 후 목록 확인 : ls
- 결과 : 
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
- 우분투 재시작 후 attach attach 명령어 사용 : docker start ubuntu_test -> docker attach ubuntu_test
root@4dbdc0e40fef:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@4dbdc0e40fef:/# exit
exit
- attach 일 떄 컨테이너 상태 : docker ps\
CONTAINER ID   IMAGE           COMMAND                   CREATED             STATUS             PORTS                                     NAMES
80d426a6e000   task01_server   "/docker-entrypoint.…"   57 minutes ago      Up 57 minutes      0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   con_number03
35770127198d   task01_server   "/docker-entrypoint.…"   About an hour ago   Up About an hour   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   con_number01

- 우분투 종료 후 다시 우분투 시작 : docker start ubuntu_test
- exec 명령어로 상태 확인 : docker exec -it ubuntuP_test bash
Error response from daemon: No such container: ubuntuP_test
sinai4867038@c6r1s1 ~ % docker exec -it ubuntu_test bash
root@4dbdc0e40fef:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@4dbdc0e40fef:/# exit
exit
sinai4867038@c6r1s1 ~ % docker ps\
CONTAINER ID   IMAGE           COMMAND                   CREATED             STATUS             PORTS                                     NAMES
4dbdc0e40fef   ubuntu          "/bin/bash"               27 minutes ago      Up 51 seconds                                                ubuntu_test
80d426a6e000   task01_server   "/docker-entrypoint.…"   59 minutes ago      Up 59 minutes      0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   con_number03
35770127198d   task01_server   "/docker-entrypoint.…"   About an hour ago   Up About an hour   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   con_number01

 * attach / exec 를 각각 사용했을 때 결과값이 다름. attach가 메인 프로세스가 종료되면 컨테이너도 함께 종료되는 것으로 보아 실행 중인 컨테이너의 메인 프로세스에 직접 연결된다는 것을 알 수 있다. exec는 exit를 입력해도 생성한 쉘만 종료되고 컨테이너는 계속 실행되는 것으로 보아(시간에 second가 찍히면서 직전까지 실행되고 있음을 확인할 수 있다.) 실행 중인 컨테이너 내부에서 새로운 프로세스를 생성하여 명령을 실행한다는 것을 알 수 있다. (이미지 : https://github.com/Sinai-Cho/sinai_repository_01/issues/12#issue-5071233406)

### 7). 도커 데몬 동작 여부 확인 결과 기록(info) 과 결과
- 명령어 : docker info
- 결과 : (이미지 : https://github.com/Sinai-Cho/sinai_repository_01/issues/11#issue-5071037587)
sinai4867038@c6r1s1 ~ % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/sinai4867038/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/sinai4867038/.docker/cli-plugins/docker-compose

Server:
 Containers: 3
  Running: 3
  Paused: 0
  Stopped: 0
 Images: 2
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: b328e8d6-d700-4b36-91cf-90e5155ed45b
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set

### 8) Git, vscode 연동
- 기본 브랜치설정 후 vscode에서 깃허브 로그인 및 저장소 연동 완료
- 실행 및 결과 로그와 스크린샷 (https://github.com/Sinai-Cho/sinai_repository_01/issues/7#issue-5068738275)\
sinai4867038@c6r1s2 ~ % git config --global user.name "Sinai Cho")
sinai4867038@c6r1s2 ~ % git config --global user.email "sinai486@gmail.com"
sinai4867038@c6r1s2 ~ % git config --global init..defaultBranch main
sinai4867038@c6r1s2 ~ % git config --list
credential.helper=osxkeychain
init.defaultbranch=main
user.name=Sinai Cho
user.email=sinai486@gmail.com
init..defaultbranch=main
- viscose 연동
- GitHub 저장소 연동


