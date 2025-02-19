# Deployment 업데이트 및 롤백

## 1. Deployment 업데이트 방식

### 1.1 `kubectl apply` (선언적 방식)
- YAML 파일을 기반으로 **Deployment를 생성하거나 업데이트**하는 방식
- 변경된 부분만 적용하며, 전체 배포 상태를 관리 가능
- GitOps 등 **CI/CD 환경에서 유용**

#### ✅ 사용 예시
```sh
kubectl apply -f deployment.yml
```

#### ✅ 장점
- 변경된 부분만 적용 (기존 리소스 유지)
- YAML 파일을 기준으로 관리 가능

#### ❌ 단점
- 클러스터에서 직접 변경된 값(YAML 외부 변경)은 반영되지 않음

---

### 1.2 `kubectl set` (명령형 방식)
- 특정 속성(예: 이미지)을 CLI에서 직접 변경하는 방식
- 빠르게 수정 가능하지만, YAML 파일에는 반영되지 않음

#### ✅ 사용 예시
```sh
kubectl set image deployment/myapp-deployment nginx=nginx:1.10.0
```

#### ✅ 장점
- YAML 수정 없이 **즉시 특정 필드(예: 이미지) 변경 가능**
- 긴급한 핫픽스 적용에 유용

#### ❌ 단점
- YAML 파일과 불일치 발생 가능
- 이후 `kubectl apply` 실행 시 원래 YAML 값으로 덮어씌워질 수 있음

##### 📌 해결 방법
변경 내용을 유지하려면 YAML을 업데이트해야 함.
```sh
kubectl get deployment myapp-deployment -o yaml > deployment.yml
```

---
<br>

## 2. Deployment 전략: `Recreate` vs `RollingUpdate`

### 2.1 `Recreate` 전략
- **기존의 모든 파드를 종료 후 새로운 파드를 생성하는 방식**
- **다운타임이 발생**할 수 있음

#### ✅ 사용 예시
```yaml
strategy:
  type: Recreate
```

#### ✅ 장점
- 기존 파드를 **완전히 초기화** 가능
- 빠른 배포 가능

#### ❌ 단점
- 모든 파드가 종료되므로 **서비스 중단 발생 가능**

---

### 2.2 `RollingUpdate` 전략
- **기존 파드를 점진적으로 교체하여 무중단 배포를 지원**

#### ✅ 사용 예시
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

#### ✅ 장점
- **무중단 배포 가능**
- 트래픽을 유지하며 점진적 배포

#### ❌ 단점
- 업데이트 속도가 상대적으로 느릴 수 있음

---
<br>

## 3. Deployment 상태 확인

### ✅ 현재 Deployment 목록 조회
```sh
kubectl get deployments
```

### ✅ 특정 Deployment의 배포 상태 확인
```sh
kubectl rollout status deployment/myapp-deployment
```

### ✅ Deployment 변경 이력 확인
```sh
kubectl rollout history deployment/myapp-deployment
```

---
<br>

## 4. Deployment 롤백

### 4.1 최신 버전으로 롤백
```sh
kubectl rollout undo deployment/myapp-deployment
```

### 4.2 특정 버전으로 롤백
```sh
kubectl rollout undo deployment/myapp-deployment --to-revision=2
```

#### 📌 롤백 시 주의점
- `kubectl apply` 실행 시 YAML 기준으로 덮어씌워질 수 있음
- `kubectl set`으로 변경한 내용이 있으면 `kubectl get deployment -o yaml`로 업데이트 필요

---
<br>

## 5. `apply` vs `set` 비교

| 변경 방법 | 클러스터 반영 | YAML 파일 반영 |
|------------|--------------|--------------|
| `kubectl apply -f deployment.yml` | ✅ | ✅ |
| `kubectl set image deployment/myapp-deployment nginx=nginx:1.10.0` | ✅ | ❌ |
| `kubectl get deployment myapp-deployment -o yaml > deployment.yml` | ✅ | ✅ (반영됨) |

📌 **Deployment 관리를 YAML 중심으로 유지하려면 `kubectl apply`를 사용하고, `kubectl set`을 사용할 경우 변경 내용을 YAML에도 반영해야 한다.**

