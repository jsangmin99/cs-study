## Infra

### **1. 가상화가 무엇이고, 이것이 가상머신과 어떠한 차이가 있는지 설명해 주세요.**

- **그렇다면 Docker는 둘 중 어디에 속하나요? 왜 사람들이 Docker를 많이 채택할까요?**
- **하나의 Host OS에서 돌아간다면 충분히 한 컨테이너가 다른 컨테이너에 간섭할 수 있는 위험이 있지 않을까요? 이를 어떻게 방어할 수 있을까요?**
- **Docker 위에 Docker를 올릴 순 없을까요?**
- **Docker Engine 은 무엇인가요?**

### **2. CI/CD 를 사용해 본 경험이 있나요? 있다면 간단하게 설명해 주세요.**
1. Spring Boot와 CI/CD
    
    CI:
    - Spring Boot 프로젝트에서 GitHub Actions를 통해 자동으로 테스트와 빌드를 진행.
    DockerFile을 실행 시켜 이미지를 생성하였음 
    - 빌드속도를 향샹시키기 위해 JIB를 사용해 Docker 이미지를 빌드하며, 캐싱 설정을 통해 빌드 속도를 최적화.
    - 빌드시 환경분리를 하여 이를 효율적으로 관리

    - 주의사항 : 캐싱 설정: Docker 빌드에서 불필요한 단계 재실행을 방지하고 이전 레이어를 재사용하도록 설정.
    
    CD:
    - Docker 이미지가 빌드되면 Docker Hub 또는 레지스트리에 이미지를 푸시하여 배포준비 + ssh 로 접속하여 실행.

2. Argo CD와 CD(Continuous Deployment)
Argo CD를 활용해 Kubernetes 클러스터에 Spring Boot 애플리케이션을 자동으로 배포.

    구성 및 워크플로우:
    - Helm 차트 또는 Kustomize를 활용해 애플리케이션의 Kubernetes 매니페스트 관리.
    - Argo CD의 GitOps 방식을 채택하여 Git 저장소를 애플리케이션의 소스 코드와 설정 파일의 단일 진실 소스로 사용.
    - Argo CD UI 를 통해 애플리케이션의 동기화 상태와 리소스를 모니터링.

    주요 설정:
    - Sync Policy:
    수동 또는 자동 동기화를 설정.
    자동 동기화의 경우 application.yaml 파일을 주기적으로 확인하여 변경 사항을 클러스터에 적용.
    
    환경 분리:
    - staging, production과 같은 환경별 설정을 Helm 차트의 values 파일이나 Kustomize overlays로 분리 관리.
    - Rollback 지원 : Argo CD의 History 기능을 통해 이전 버전으로 복구 가능.
    
    효과:
    - 애플리케이션 배포 프로세스의 자동화로 수작업 배포에서 발생하던 실수 방지.
    - 배포 상태와 애플리케이션의 리소스 상태를 직관적으로 확인 가능.
    - GitOps 기반으로 코드와 배포 상태를 일치시켜 환경 일관성 확보.

3. 프론트엔드 CI/CD 경험
    CI:
    - GitHub Actions를 사용해 프론트엔드 코드 변경 시 자동 빌드 및 테스트.

    CD:
    - 빌드된 정적 파일을 S3 버킷에 업로드하고, CloudFront를 통해 캐싱 및 콘텐츠 배포.

    - Route 53: 도메인 네임 시스템(DNS)을 설정해 CloudFront와 사용자 도메인을 연동.
    캐싱 무효화: GitHub Actions에서 CloudFront의 캐시를 무효화(Invalidation)하여 최신 콘텐츠 제공.


### **3. Web Server, Web Application Server 의 차이는 무엇인가요?**
- Web Server는 **정적 콘텐츠(HTML, CSS, JS)**를 클라이언트에 제공하는 역할을 하고,
- **WAS(Web Application Server)**는 **동적 콘텐츠(데이터 처리, 비즈니스 로직)**를 실행해 응답합니다.
- 정리하면 Web Server는 정적 리소스 제공에 최적화되어 있고, WAS는 데이터베이스와 통신하며 애플리케이션 로직을 처리합니다.

### 4. Git에 대해 설명해 주세요.
 **여러 브랜치를 합쳐야 할 때, 어떤 방법을 사용할 수 있는지 "모두" 설명해 주세요.**
- 분산 버전 관리 시스템으로 코드의 변경 내역을 관리하고 팀원 간 협업을 도와주는 도구 이다.
- 브랜치를 합치는 방법에는 Merge와 Rebase를 주로 사용합니다. 좀더 심화적으로 들어가면 Cherry-pick, Squash, Fast-forward 같은 방법도 있습니다.
  - merge : 두브랜치를 합쳐서 하나의 커밋으로 만드는 방법
  - rebase : 브랜치의 커밋을 직선형으로 재배치하여 히스토리를 단순화 하여 하는 방법입니다. Origin에서 사용하면 충돌을 일으킬수있어서 조심히 사용해야합니다.
  - Cherry-pick : 특정 커밋만 골러서 머지하고 싶을때 사용합니다.
  - Squash : 여러 커밋을 하나의 커밋으로 압축시켜 커밋히스토리를 깔끔하게 합니다. 주로 PR 이후 머지할때 사용합니다.
  - Fast-forward : 병합 대상 브랜치가 현재 브랜치의 직계 후손인 경우, 병합 커밋 없이 브랜치를 단순히 이동합니다.