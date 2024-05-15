---
tags:
  - Jenkins
  - CI/CD
  - FrameWork
---
### 계기
매일 새벽 3시에 프로젝트를 배포 시키려고 배치를 설정해 놓았습니다.
Jenkins 글로벌 설정에서 한국 시간으로 타임존을 변경 했지만...
웹페이지 상의 시간만 한국 시간으로 나올 뿐!!
실제로 배치가 도는 시간은 변경되지 않고 이상한 시간에 배포가 되고 있었습니다....

오늘은 이를 고쳐보도록 합시다.

### 젠킨스 컨테이너 접속
우선 가장 먼저 해야 할 것이 컨테이너 안으로 접속을 해야 합니다.
`/bin/bash`는 우리가 사용할 shell을 선택해 주는 것입니다.

```bash
sudo docker exec -it ${container_name} /bin/bash
```

### (option) vim 설치
기본적은 cat이나 다른 텍스트 에디터를 사용해도 무방합니다.
저는 vim이 변하기 vim을 설치해보도록 하겠습니다.

```bash
api-get update

api-get install vim
```

### shell script 수정하기
아래의 shell script는 실제 젠킨스를 빌드할 때 사용하는 스크립트 파일입니다.
중요한 정보를 많이 담고 있어, 큰 문제가 없다면 건드리지 않는 것을 추천합니다.
아래 파일에 접속해서 `exec java -Duser~~`을 찾아야 합니다.
검색을 한다면 `exec java`가 적당할 겁니다. 한번밖에 나오지 않거든요.
검색해서 찾았다면 검색해서 찾은 해당 줄 바로 윗부분에 아래의 문구를 추가하시면 됩니다.

(cf. '+' 기호는 추가하라는 의미에서 넣어 둔 것이니`export TZ="Asia/Seoul"`을 추가)

```sh title:"/usr/local/bin/jenkins.sh"
# /usr/local/bin/jenkins.sh

+ export TZ="Asia/Seoul"
 
exec java -Duser.home...
```

### contrainer restart
이제 설정한 내용이 적용이 되도록 해당 컨테이너를 재실행시켜주어야 합니다.
```bash
sudo docker stop ${container_name}

sudo docker start ${container_name}
```