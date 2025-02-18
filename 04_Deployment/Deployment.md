# Kubernetes Deployment 가이드

## 1. 개요
Deployment는 Kubernetes에서 애플리케이션을 배포하고 관리하는 리소스로, 자동으로 ReplicaSet을 생성 및 관리합니다. 이를 통해 **롤링 업데이트, 롤백, 확장**을 쉽게 수행할 수 있습니다.

---
<br>

## 2. Deployment vs. ReplicaSet

| 기능            | ReplicaSet  | Deployment  |
|----------------|------------|-------------|
| 목적           | 특정 개수의 파드 유지 | ReplicaSet 관리 및 버전 관리 제공 |
| 롤링 업데이트 | ❌ 지원 안함 | ✅ 지원 |
| 롤백 기능     | ❌ 지원 안함 | ✅ 지원 (자동 롤백 가능) |
| 버전 관리     | ❌ 지원 안함 | ✅ 지원 (`kubectl rollout history`) |
| 확장 기능     | ✅ 가능 | ✅ 가능 |
| 사용 추천     | 단순한 파드 개수 유지 | 실무 애플리케이션 배포에 적합 |

**📌 결론:** 실무에서는 **Deployment를 사용하는 것이 일반적**이며, 업데이트 및 롤백 기능을 제공하여 유지보수가 용이합니다.

---
<br>

## 3. Deployment 생성하기
아래는 Deployment를 정의하는 YAML 예제입니다.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: nginx
        image: nginx:latest
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

### ✅ 주요 설명
- `replicas: 3` → 항상 3개의 파드를 유지
- `selector.matchLabels` → `app: my-app` 레이블이 있는 파드를 관리
- `strategy.rollingUpdate` → 롤링 업데이트 전략 적용 (점진적 배포)

---
<br>

## 4. Deployment 관리하기

### 📌 4.1 Deployment 적용하기
```sh
kubectl apply -f deployment.yaml
```

### 📌 4.2 Deployment 상태 확인
```sh
kubectl get deployments
kubectl describe deployment my-deployment
```

### 📌 4.3 Deployment 스케일링 (확장)
```sh
kubectl scale deployment my-deployment --replicas=5
```

### 📌 4.4 Deployment 업데이트
YAML 파일에서 새로운 이미지 버전을 지정한 후 적용:
```sh
kubectl set image deployment/my-deployment nginx=nginx:1.21.3 --record
```

### 📌 4.5 Deployment 롤백
업데이트에 문제가 발생하면 이전 버전으로 롤백 가능:
```sh
kubectl rollout undo deployment my-deployment
```

### 📌 4.6 Deployment 배포 이력 조회
```sh
kubectl rollout history deployment my-deployment
```

---
<br>

## 5. Deployment 삭제하기
Deployment를 완전히 제거하려면:
```sh
kubectl delete deployment my-deployment
```

---
<br>

## 6. 결론
- **Deployment는 Kubernetes에서 가장 추천되는 배포 방식**입니다.
- **자동 확장, 롤링 업데이트, 롤백** 기능을 지원하여 유지보수가 편리합니다.
- Deployment를 사용하면 **ReplicaSet을 자동으로 관리**하여 배포 프로세스를 단순화할 수 있습니다.
