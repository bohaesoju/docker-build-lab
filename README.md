# Multi-Stage Build로 경량화된 Next.js Docker 이미지

이 프로젝트에서는 **Multi-Stage Build**를 활용하여 Next.js 애플리케이션을 경량화된 Docker 이미지로 빌드합니다. 
또한, Next.js 공식 문서에서 소개하는 **Standalone 빌드** 방식을 사용하여 빌드 결과물의 용량을 최소화했습니다.

# Docker 개발 환경 가이드

## 1. 빌드 (Build)

### 개발 환경
아래 명령어를 사용하여 개발 환경에서 이미지를 빌드합니다:
```bash
docker build -t {{빌드할 Image 파일명}} -f ./Dockerfile . --build-arg ENV_MODE=development
```

### 운영 환경
```bash
docker build -t {{빌드할 Image 파일명}} -f ./Dockerfile . --build-arg ENV_MODE=production
```
### 예시
```bash
docker build -t docker-build-lab -f ./Dockerfile . --build-arg ENV_MODE=development
```
## 2. 실행

```bash
docker run -it --rm -p 3000:3000 {{빌드한 Image 파일명}}
```

### 예시
```bash
docker run -it --rm -p 3000:3000 docker-build-lab
```
