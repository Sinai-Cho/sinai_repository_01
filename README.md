# sinai_repository_01
For github &amp; codyssey connecting
## 1st 과제 : 내 컴퓨터에 개발자용 '작업실' 꾸미기
## 1. 실행환경
- OS : macOS
- Shell : zsh
- Docker : 28.5.2
- Git : git version 2.53.0
  
## 2. 수행 항목 체크리스트
- 하위 작업 중 문제가 발생했던 기록 포함 정리
### 1) 터미널로 작업 디렉토리와 권한 정리
- 작업용 디렉토리 생성 : mkdir ~/task01_webserver
- 생상한 디렉토리로 이동 : cd ~/task01_webserver
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
### 2) docker 설치, 점검 및 컨테이너 실행/관리
- docker 설치 : brew install --cask docker
- 권한 문제로 설치 실패(https://github.com/Sinai-Cho/sinai_repository_01/issues/1#issue-5036160924)
- 해결 방법 : 맥북 내 Orbstatk 앱 활성화 후 터미널에서 docker version 명령어를 사용해 사용가능한 상태 및 버전 확인 완료
### 3) Dockerfile 작업
- 현재 디렉토리 안에 하위 디렉토리 생성 : mkdir src
- 웹서버용 파일 생성 : echo "<h1>Hello Wolrd</h1>" > src/index.html
- Dockerfile 만들기
  echo "FROM nginx:latest" > Dockerfile (첫줄은 반드시 각진 괄고 1개. Dockerfile에 괄호 왼쪽 내용을 넣는다는 뜻) \
  echo "COPY src/index.html /usr/share/nginx/html/index.html" >> Dockerfile \
  echo "EXPOSE 80" >> Dockerfile (괄호가 2개면 앞 내용에 이어쓴다는 의미. 1개면 덮어쓴다는 의미. 첫줄만 괄호 1개여야 한다)\ 
- Dockerfile 에 내용이 제대로 들어갔는지 확인 : cat Dockerfile
  * 결과

          FROM nginx:latest
 
          COPY src/index.html /usr/share/nginx/html/index.html
    
          EXPOSE 80
### 4) 포트 매핑 및 바인드 마운트 작업
- docker build : docker build -t task01_server .
- docker 포트 매핑 및 바인드 마운트 작업 :\
  docker run -d -p 8080:80 --name con_number01 -v $(pwd)/src:/usr/share/nginx/html task01_server
  * 결과 : b60157574a96c77404dec2760cfa6302f0222ca864028d904572d9d92a3f0a53(https://github.com/Sinai-Cho/sinai_repository_01/issues/3#issue-5036423712)
  * -d는 뒤에서 작업하겠다는 의미, -p는 포트 매핑(맥북의 8080포트와 dockerfile에서 설정한 80포트를 연결), --name은 컨테이너 이름, -v는 src 디렉토리 내의 파일과 바인드마운팅 한다는 의미, task01_server 는 docker가 생성한 이미지 이름
### 5) 볼륨 작업

##3. Github 연동

4. 검증 방법 (어떤 명령으로 뭘 했는지 / 결과 위치 링크)
5. 트러블슈팅 2건 이상(문제 - 뭔인 및 가설 - 확인 - 해결 )
6. 현재 위치 확인, 목록 확인, 이동, 생성, 복사, 이동/이름 변경, 삭제, 파일 내용 확인, 빈 파일 생성
7. 권한 확인/변경하는 명령 수행 결과(변경 전 후 비교)
8. 파일 1개, 디레ㅔㄱ토리 1개에 대한 권한 변경 실험 수행 기록 남길것
9. 도커 버전 확인 결과 기록(version) 과 결과
10. 도커 데몬 동작 여부 확인 결과 기록(info) 과 결과
11. dockerfile 기반 커스텀 이미지 제작
- 어떤 베이스 사용해서(NGINX) 제작했는지
- 내가 적용한 커스텀 포인트 각각의 목적(?!)
- build/실행 명령/출력 결과 로그와 스크린샷
1. 컨테이너 실행/중지/목록 확인( ps-a) 
- hello world 실행 성공 기록
- 우분투 컨테이너 실행, 내부 진입 후 간단 명령(ls 수행 결과 기록)
- 컨테이너 종료/유지(attach/exec 의 차이) 관찰해서 정리
1. 포트 매핑 접속 성공 증거(스크린샷 혹은 로그) - p 8080:80 으로 실행 후 브라우저 접속 화면(주소창 포함)을 기술문서에 첨부할 것
2. 바인드 마운트 반영 & 볼륨 영속성 증거
- 바인드마운트 실행명령과 호스트 변경 전 후 비교
- docker 볼륨 생성하고 컨테이너에 연결
- 컨테이너 삭제 전후로 데이터 확인해서 데이터 유지됨 확인
- 생성/연결/검증 명령+출력 결과를 기록한다.
1. 웹ㅂ서버 소스코드(src/) ????

<git 설정 및 깃허브/vscode 연동 증거>

1. 기본 브랜치설정 후 vscode에서 깃허브 로그인 및 저장소 연동 완료

( git donfig —list 명령어 결과 기록 / 연동 증거로 스크린샷 첨부)
