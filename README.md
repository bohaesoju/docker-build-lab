# Multi-Stage Build로 경량화된 Next.js Docker 이미지

이 프로젝트에서는 **Multi-Stage Build**를 활용하여 Next.js 애플리케이션을 경량화된 Docker 이미지로 빌드합니다. 
또한, Next.js 공식 문서에서 소개하는 **Standalone 빌드** 방식을 사용하여 빌드 결과물의 용량을 최소화했습니다.

GitHub Actions를 사용하여 AWS ECR(Elastic Container Registry)로 도커 이미지를 자동으로 업로드할 수 있도록 설정했습니다. 이를 통해 CI/CD 파이프라인을 간소화하고, 수동 작업을 최소화했습니다.

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

# GitHub Actions를 활용한 ECR 업로드 자동화
## 자동화 프로세스
1. **AWS ECR 생성**: AWS Management Console에서 ECR 저장소를 생성합니다.
2. **IAM 사용자 생성**:
   - ECR 푸시 권한을 가진 IAM 사용자 생성 (AmazonEC2ContainerRegistryPowerUser 정책 연결)
   - 액세스 키 발급 시 "다른 사용 사례" 선택
3. **GitHub Secrets 설정**: GitHub 레포지토리의 `Settings > Secrets and variables > Actions`에서 다음 값을 추가합니다:
   - `AWS_ACCESS_KEY_ID`: AWS 액세스 키
   - `AWS_SECRET_ACCESS_KEY`: AWS 시크릿 키
   - `ECR_REPOSITORY`: ECR 저장소 이름 (예: `docker-lab`)
4. **GitHub Actions 워크플로 설정**: 레포지토리 루트에 `.github/workflows/docker-deploy.yml` 파일을 생성하고 다음과 같은 워크플로를 추가합니다:
```yaml

## 테스트
1. `main` 브랜치에 코드를 푸시하면 GitHub Actions가 실행됩니다.
2. Actions 실행 후, AWS ECR에 도커 이미지가 업로드됩니다.
3. AWS Management Console에서 업로드된 이미지를 확인할 수 있습니다.

GitHub Actions를 활용한 자동화를 통해 효율적으로 Docker 이미지를 관리할 수 있습니다.
