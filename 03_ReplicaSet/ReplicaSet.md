# 📌 ReplicaSet

## 🔹 1. ReplicaController vs. ReplicaSet
### 🛑 (1) ReplicaController (RC)
- ⚠️ **구버전**으로 현재는 거의 사용되지 않음.
- 특정 개수의 Pod을 유지하는 역할.
- 단순한 Label Selector(`matchLabels`만 지원)만 가능.

### ✅ (2) ReplicaSet (RS)
- **ReplicaController의 개선판**으로 현재 사용됨.
- 🔹 더 정교한 Label Selector(`matchExpressions` 지원).
- 🚀 **Deployment에서 내부적으로 사용됨** (보통 직접 사용하지 않고 Deployment를 이용함).

---
<br>

## 🔹 2. Labels vs. Selector
### 🏷️ (1) Labels
- 📌 Key-Value 형태의 **메타데이터**.
- 📦 Kubernetes 객체(Pod, Service, ReplicaSet 등)에 **태그를 달아 분류**.
- 🔹 단순한 정보로만 존재하며, 직접 필터링 기능은 없음.

### 🔍 (2) Selector
- ✅ 특정 Labels이 붙은 객체를 **선택**하여 관리.
- `matchLabels`: 단순한 Key-Value 매칭.
  ```yaml
  selector:
    matchLabels:
      app: my-app
  ```
- `matchExpressions`: 여러 조건을 적용 가능.
  ```yaml
  selector:
    matchExpressions:
    - key: env
      operator: In
      values:
      - production
      - staging
  ```

---
<br>

## 🔹 3. ReplicaSet 예제
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-app-rs
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
      - name: my-app-container
        image: nginx
        ports:
        - containerPort: 80
```
### ✨ 주요 개념
1. 🎯 `selector.matchLabels.app: my-app` → 관리할 Pod을 선택.
2. 🔖 `template.metadata.labels.app: my-app` → 생성된 Pod에 자동으로 라벨 추가.
3. ⚙️ `template.spec` → 생성할 Pod의 컨테이너 설정.

---
<br>

## 🔹 4. Scale (Pod 개수 조정)
### 📝 (1) 직접 수정 (kubectl edit)
```sh
kubectl edit rs my-app-rs
```
- `spec.replicas: 3` 값을 원하는 숫자로 변경 후 저장.

### 🚀 (2) scale 명령어 (추천)
```sh
kubectl scale rs my-app-rs --replicas=5
```
- ✅ 즉시 Pod 개수 변경 가능.
- ❌ **하지만 `replicaset.yaml` 파일에는 변경이 반영되지 않음!**

### **🔍 변경 적용 방식 비교**
| 방법 | 실행 명령어 | RS에 즉시 반영? | YAML 파일도 변경됨? |
|------|------------|--------------|--------------|
| `kubectl scale` | `kubectl scale rs my-app-rs --replicas=5` | ✅ 예 | ❌ 아니요 |
| `kubectl edit` | `kubectl edit rs my-app-rs` | ✅ 예 | ❌ 아니요 |
| `kubectl apply` | `kubectl apply -f replicaset.yaml` | ✅ 예 | ✅ 예 (파일에 직접 적용) |


### **1️⃣ YAML 파일에도 변경을 반영하려면?**
#### **방법 1: `kubectl apply`를 다시 실행**
- 기존 `replicaset.yaml`을 수정 후 적용
```yaml
spec:
  replicas: 5
```
```sh
kubectl apply -f replicaset.yaml
```

#### **방법 2: `kubectl replace` 사용**
```sh
kubectl replace -f replicaset.yaml
```
- 기존 리소스를 삭제 후 다시 생성하는 방식.

---
<br>

### 🔹 5. 주요 명령어 정리
| 🛠️ 기능 | 🔹 명령어 |
|------|------------|
| **📌 생성 (create/apply)** | `kubectl apply -f replicaset.yaml` |
| **🔍 조회 (get)** | `kubectl get rs` |
| **🗑️ 삭제 (delete)** | `kubectl delete rs my-app-rs` |
| **✏️ 수정 (replace)** | `kubectl replace -f replicaset.yaml` |

---
<br>

## 🔹 6. template의 역할
- 🏗️ **ReplicaSet, Deployment에서 Pod의 템플릿을 정의하는 영역**.
- 🏷️ 생성되는 Pod의 라벨과 컨테이너 설정 포함.
- 예제:
  ```yaml
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: nginx
  ```
- `replicas: 3` → 이 템플릿을 기반으로 3개의 Pod 생성.

---
<br>

## 🔹 7. 결론
✅ `ReplicaSet` = **Pod 개수를 유지하면서 자동 관리**하는 역할.  
✅ `Labels` = **객체의 카테고리를 지정하는 태그**.  
✅ `Selector` = **해당 카테고리에서 특정 객체를 선택**.  
✅ `template` = **Pod을 만들기 위한 설계도**.  
✅ **보통 ReplicaSet을 직접 사용하지 않고 Deployment를 이용**하는 것이 일반적! 🚀

