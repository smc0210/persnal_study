# WEBTOB 

----------------

##### 1. 개요

WEBTOB 가이드입니다. 버전은 4.1 윈도우를 기준으로 작성하였습니다.
또한 사전에 JEUS가 설치되어 있어야만 합니다.

---------------



##### 2. 설치

[technet.tmaxsoft.com](http://technet.tmaxsoft.com)

제우스 및 웹투비를 설치합니다.

----------------

##### 3. 라이센스 발급

서버를 실행시키기 위한 진행 과정중 라이센스 발급을 진행하여야 합니다.
다운로드 페이지에 보면 데모라이센스 발급이 있습니다.

데모라이센스 발급절차에  각 상세내용을 입력하게 되는데 여기서  HostName은 자신의 컴퓨터명을 입력합니다.
hostname - 자신의 컴퓨터 명 (모를시엔 시작-실행-cmd-hostname 을 입력 하면 알려줍니다.)


라이센스를 발급받은 후엔 라이센스 폴더에  발급받은 파일을 복사합니다.

----------------

##### 4. http.m 컴파일

http.m 파일은 conf폴더에 있습니다.
아래 명령어로 컴파일을 진행합니다.
###### cmd-wscfl -i http.m

컴파일을 하는데 권한이슈로 인해 액세스가 거부될수 있으니 명령프롬프트를 관리자 모드로 실행시킵니다.

----------------

##### 5. 실행

명령프롬프트 창에 wsboot를 입력하여 서버를 실행합니다.
서버가 실행되면 [localhost:8080](http://localhost:8080) 을 입력해서 확인 할 수 있습니다.

로컬경로를 http.m에 기재되어 있는 DOCROOT 와 같습니다.
###### C:/TmaxSoft/WebtoB4.1/docs


