# GitHub Actions를 활용한 AWS ECS & ECR 자동 배포

이 프로젝트에서는 **Multi-Stage Build**를 활용하여 Next.js 애플리케이션을 경량화된 Docker 이미지로 빌드합니다. 
또한, Next.js 공식 문서에서 소개하는 **Standalone 빌드** 방식을 사용하여 빌드 결과물의 용량을 최소화했습니다.

GitHub Actions를 사용하여 AWS ECR(Elastic Container Registry)로 Docker 이미지를 자동으로 업로드하고,  
AWS ECS(Amazon Elastic Container Service)로 자동 배포할 수 있도록 설정했습니다.  
이를 통해 CI/CD 파이프라인을 간소화하고, 수동 작업을 최소화했습니다.

# Docker 개발 환경 가이드

## 1. 빌드 (Build)

### 개발 환경
아래 명령어를 사용하여 개발 환경에서 이미지를 빌드합니다:
```bash
docker build -t <빌드할 Image 파일명> -f ./Dockerfile . --build-arg ENV_MODE=development
```

### 운영 환경
```bash
docker build -t <빌드할 Image 파일명> -f ./Dockerfile . --build-arg ENV_MODE=production
```
### 예시
```bash
docker build -t docker-build-lab -f ./Dockerfile . --build-arg ENV_MODE=development
```

## 2. 실행

```bash
docker run -it --rm -p 3000:3000 <빌드한 Image 파일명>
```

### 예시
```bash
docker run -it --rm -p 3000:3000 docker-build-lab
```

# GitHub Actions를 활용한 ECR 업로드 자동화
## 자동화 프로세스
1. **AWS ECR 생성**: AWS Management Console에서 ECR 저장소를 생성합니다.
2. **AWS ECS 생성**: AWS Management Console에서 ECS 클러스터를 생성합니다.
   - 태스크와 서비스 생성
3. **IAM 설정**: ECR, ECS 배포 관련 권한 추가
4. **GitHub Secrets 설정**: GitHub 레포지토리의 `Settings > Secrets and variables > Actions`에서 다음 값을 추가합니다:
   - `AWS_ACCESS_KEY_ID`: AWS 액세스 키
   - `AWS_SECRET_ACCESS_KEY`: AWS 시크릿 키
   - `ECR_REPOSITORY`: ECR 저장소 이름 (예: `docker-lab`)
5. **GitHub Actions 워크플로 설정**: 레포지토리 루트에 `.github/workflows/ecs-deploy.yml` 파일을 생성하고 다음과 같은 워크플로를 추가합니다:

# 배포 프로세스

## 1. main 브랜치에 코드 푸시
## 2. GitHub Actions 워크플로우 실행

### - Docker 이미지 빌드
### - ECR에 이미지 푸시
### - ECS 작업 정의 업데이트
### - ECS 서비스 배포
