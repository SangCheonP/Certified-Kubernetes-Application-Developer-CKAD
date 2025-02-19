# 🚀 Kubernetes Syntax Guide

## **📌 기본 명령어 형식**
```sh
kubectl <명령어> <리소스 유형> <리소스 이름> [추가 옵션]
```
---
<br>

## **🔍 `kubectl` 명령어 구조**
| 구성 요소 | 설명 | 예시 |
|----------|---------------------------------|------------------------------|
| `kubectl` | Kubernetes CLI 명령어 | `kubectl` |
| `<명령어>` | 수행할 작업 | `get`, `apply`, `set`, `delete`, `rollout` 등 |
| `<리소스 유형>` | Kubernetes 리소스 유형 | `pod`, `deployment`, `service`, `configmap` 등 |
| `<리소스 이름>` | 특정 리소스 지정 (선택적) | `my-app`, `nginx-deployment` 등 |
| `[추가 옵션]` | 추가적인 설정 | `-o yaml`, `--all-namespaces`, `--watch` 등 |

---
<br>

## **🔹 명령어 유형별 예제**

### 🟢 **1️⃣ 리소스 조회 (`get`)**
```sh
kubectl get pods
kubectl get deployments my-app
kubectl get services -o yaml
```
📌 **목록 조회, 특정 리소스 조회, 출력 포맷 설정**

---

### 🟡 **2️⃣ 리소스 생성 및 적용 (`apply`, `create`)**
```sh
kubectl apply -f deployment.yaml
kubectl create deployment my-app --image=nginx
```
📌 **YAML 파일을 적용하거나 직접 생성**

---

### 🟠 **3️⃣ 리소스 수정 (`set`, `edit`)**
```sh
kubectl set image deployment my-app nginx=nginx:1.20
kubectl edit deployment my-app
```
📌 **이미지 변경 또는 YAML 직접 편집**

---

### 🔴 **4️⃣ 리소스 삭제 (`delete`)**
```sh
kubectl delete pod my-pod
kubectl delete deployment my-app
```
📌 **리소스 삭제**

---

### 🔵 **5️⃣ 롤링 업데이트 및 롤백 (`rollout`)**
```sh
kubectl rollout status deployment my-app
kubectl rollout history deployment my-app
kubectl rollout undo deployment my-app --to-revision=2
```
📌 **배포 상태 확인, 변경 이력 조회, 특정 버전으로 롤백**

---
<br>

## **✨ 쉽게 기억하는 방법**
1️⃣ **무엇을 할지?** → `get`, `apply`, `delete`, `set`, `rollout` 등

2️⃣ **어떤 리소스?** → `pod`, `deployment`, `service`, `configmap` 등

3️⃣ **어떤 대상?** → `my-app`, `nginx-deployment` 등 (필요 시 지정)

4️⃣ **추가 옵션은?** → `-o yaml`, `--all-namespaces`, `--watch` 등

📌 **이 형식을 이해하면 `kubectl` 명령어 사용이 훨씬 쉬워집니다!** 🚀

