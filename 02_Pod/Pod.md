# 🚀 Kubernetes Pod 명령어 정리

## 📌 1. 파드 생성 및 관리

### 📝 YAML 파일을 사용하여 리소스 생성
```bash
kubectl apply -f <파일명>.yaml
```
✅ YAML 파일을 기반으로 쿠버네티스 리소스를 **생성 및 업데이트**합니다.  

### 📝 텍스트 파일을 사용하여 리소스 생성
```bash
kubectl apply -f <파일명>.txt
```
✅ `.txt` 파일을 적용하여 리소스를 **생성**합니다.  

---
<br>

## 📌 2. 파드 정보 조회

### 🔍 현재 네임스페이스의 모든 파드 조회
```bash
kubectl get pods
```
✅ 현재 네임스페이스에서 **실행 중인 모든 파드 목록을 확인**할 수 있습니다.  
<br>

### 🔍 특정 파드의 상세 정보 조회
```bash
kubectl describe pod <파드명>
```
✅ 특정 파드의 **상태, 이벤트, 환경 변수 등의 상세 정보**를 확인할 수 있습니다.  

---
<br>

## 📌 3. 파드 삭제

### 🗑️ 특정 파드 삭제
```bash
kubectl delete pod <파드명>
```
✅ 지정한 **파드를 삭제**합니다.  

---
<br>

## 📌 4. 특정 이미지로 파드 실행

### 🛠️ Nginx 컨테이너 실행
```bash
kubectl run nginx --image=nginx
```
✅ `nginx` 도커 이미지를 사용하는 **Nginx 파드**를 생성합니다. 

<br>

### 🛠️ Redis 컨테이너 실행 (YAML 파일로 출력)
```bash
kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml
```
✅ `redis` 도커 이미지를 사용하는 **Redis 파드의 YAML 설정 파일을 생성**합니다. 

<br>

### 📌 명령어 상세 설명  
| 옵션 | 설명 |
|------|------|
| `kubectl run redis` | `redis`라는 이름의 파드를 생성 |
| `--image=redis` | `redis` 도커 이미지를 사용하여 컨테이너 실행 |
| `--dry-run=client` | **실제 생성 없이** YAML 파일로 변환 (테스트 용도) |
| `-o yaml` | 출력을 **YAML 형식**으로 변환 |
| `> redis.yaml` | YAML 내용을 `redis.yaml` 파일로 저장 |

<br>

🔹 실행 후 생성되는 `redis.yaml` 예시:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - name: redis
    image: redis
```
📌 이후 `kubectl apply -f redis.yaml` 명령어를 실행하면 Redis 파드가 생성됩니다.

---
<br>

### 🛠️ YAML 파일을 사용하여 Redis 파드 생성
```bash
kubectl create -f redis.yaml
```
✅ **`redis.yaml` 파일을 기반으로 Redis 파드를 생성**합니다.  
