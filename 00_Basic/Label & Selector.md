# 🚀 Kubernetes Label & Selector

## ** Kubernetes 리소스별 레이블과 셀렉터 사용 방식**
| 리소스 | 레이블(Label) | 셀렉터(Selector) | 설명 |
|--------|-------------|------------------|------|
| **Pod** | ✅ 있음 | ❌ 없음 | 단순히 `metadata.labels`에 레이블만 지정 |
| **ReplicaSet / Deployment** | ✅ 있음 | ✅ `selector.matchLabels` 사용 | 특정 조건을 만족하는 파드를 선택 |
| **Service** | ✅ 있음 (선택 사항) | ✅ `selector` 사용 | 특정 레이블을 가진 파드를 네트워크로 연결 |

---
<br>

### **✔ Pod (단순히 레이블만 붙이면 됨)**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp  # ✅ 단순히 레이블만 정의
spec:
  containers:
    - name: nginx
      image: nginx:latest
```

---
<br>

### **✔ ReplicaSet / Deployment (selector + matchLabels 사용)**
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp  # ✅ 파드 중에서 app=myapp 레이블을 가진 것만 선택
  template:
    metadata:
      labels:
        app: myapp  # ✅ 파드가 matchLabels에 맞게 생성됨
    spec:
      containers:
        - name: nginx
          image: nginx:latest
```

---
<br>

### **✔ Service (selector만 사용하면 됨)**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp  # ✅ matchLabels 없이 단순 selector만 사용
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 80
```

---
<br>

📌 **즉, Pod은 레이블만 정의하고, Deployment/ReplicaSet은 `matchLabels`을 통해 파드를 매칭하며, Service는 `selector`만 사용하면 된다!** 🚀

