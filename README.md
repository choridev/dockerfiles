# 개발환경 초기 셋업 도커 파일 모음집

언어별 개발환경 구축을 위한 도커 파일을 제공합니다.
본 문서는 VSCode와 Docker Desktop을 사용하는 환경에서 작성되었습니다.

## 사용법

> 예시: 파이썬

1. 레포지토리 클론

``` shell
git clone https://github.com/ChoriDev/dockerfiles.git
```

2. 도커 이미지 빌드

사용하려는 언어의 디렉토리로 이동한 뒤 도커 이미지를 빌드합니다.

``` shell
cd python
docker build -t python3.12 .
```

3. 볼륨 생성

도커 컨테이너와 연결할 볼륨을 생성합니다.

``` shell
docker volume create python3.12
```

4. 도커 컨테이너 생성

도커 이미지를 사용하여 도커 컨테이너를 생성합니다.

``` shell
docker create -it -v python3.12:/workspace --name python3.12 python3.12 
```

5. Docker Desktop에서 표시된 컨테이너를 시작합니다.

6. VSCode에서 실행 중인 컨테이너를 연결합니다.

7. `/workspace` 디렉토리를 열고 개발을 시작합니다.