# Git

### git push origin --delete [브랜치명]
원격 저장소의 브랜치 삭제. 원격 저장소의 헤더가 현재 브랜치를 가르키고 있을 경우 삭제불가능하다.


<br></br>
## .gitignore 
### ■ gitignore란?
의도적으로 git에 버전관리를 하지 말야할 파일은 .gitignore에 기술하면 untracked 시킬 수 있다. 하지만 이미 tracked된 파일은 .gitignore에 작성하더라도 무시되지 않기 때문에 파일을 추적하기 전에 무시해야한다. tracked 되어도 무시시킬 방법이 없는 것은 아니다!

### ■ 왜 파일을 무시해야하나?
자바스크립트의 npm 파일은 용량이 크기 때문에 개발자가 매번 다운받기 부담스럽기도하고 또한 package.json 파일만 있으면 모든 패키지를 설치할 수 있기 때문에 굳이 패키지까지 버전 관리를 할 필요성이 없다. 또한 민감한 데이터가 있는 파일을 버전관리하게 되면 유출되기 때문에 이러한 파일들은 버전관리를 하지 말아야 한다. 

### ■ 실습

#### 1. .gitignore 파일 생성하기
.gitignore 파일은 프로젝트 폴더의 루트에 위치해 있어야한다. linux의 touch 명령어는 0바이트 파일을 생성하기 위한 명령어이다. 이 명령어를 사용하여 .gitignore 파일을 생성하거나 직접 파일을 생성한다.
```bash
$ touch .gitignore
```

#### 2. 무시할 파일 작성
.gitignore 파일에 무시할 파일 또는 폴더를 작성한다. \#은 주석이므로 인식하지 않는다. 한 라인에 하나의 무시할 폴더 또는 파일을 작성한다.
```bash
#.gitignore

# node_modules와 하위 모든 파일 및 폴더 무시
node_modules/
```


<br></br>
# git-flow

## Branch 관리
### ■ master branch
