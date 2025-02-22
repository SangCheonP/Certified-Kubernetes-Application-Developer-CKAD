# 🚀 Kubernetes Service: NodePort, ClusterIP, LoadBalancer

## **📌 Kubernetes Service란?**
쿠버네티스에서 **Service**는 파드(Pod) 간의 통신을 가능하게 하고, 외부 트래픽을 쿠버네티스 내부로 연결하는 역할을 한다. 

쿠버네티스에는 여러 종류의 Service가 있으며, 주요한 세 가지 타입은 다음과 같다:
- **ClusterIP**: 클러스터 내부에서만 접근 가능 (기본값)
- **NodePort**: 특정 노드의 포트를 열어 외부에서 접근 가능
- **LoadBalancer**: 클라우드 환경에서 로드밸런서를 이용하여 외부 트래픽을 관리

---
<br>

## **1️⃣ ClusterIP (기본값, 내부 네트워크용)**
✅ **클러스터 내부에서만 접근 가능**  
✅ **외부에서 직접 접근할 수 없음**  
✅ **파드 간 통신을 위해 사용됨**  

### **✔ ClusterIP 트래픽 흐름**
```
(클러스터 내부 파드) → ClusterIP → 대상 파드(targetPort)
```

### **✔ ClusterIP 서비스 예제**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-clusterip
spec:
  type: ClusterIP  # 기본값 (생략 가능)
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 8080      # Service가 내부에서 받을 포트
      targetPort: 80  # 파드가 실제로 사용하는 포트
```
✅ 클러스터 내부에서 `myapp-clusterip` 서비스로 접근 가능
```sh
curl http://myapp-clusterip:8080
```

---
<br>

## **2️⃣ NodePort (외부에서 접근 가능)**
✅ **특정 노드의 포트를 개방하여 외부에서 접근 가능**  
✅ **노드의 IP + NodePort로 접근 가능 (`<노드 IP>:<NodePort>`)**  
✅ **노드가 여러 개 있으면, 어느 노드에서든 동일한 포트로 접근 가능**  

### **✔ NodePort 트래픽 흐름**
```
(클라이언트) → <노드 IP>:NodePort → Service(port) → 파드(targetPort)
```

### **✔ NodePort 서비스 예제**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-nodeport
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 8080      # Service가 내부에서 받을 포트
      targetPort: 80  # 파드가 실제로 사용하는 포트
      nodePort: 30080 # 외부에서 접근할 수 있는 포트 (30000~32767)
```
✅ 외부에서 `<노드 IP>:30080`으로 접근 가능
```sh
curl http://<노드 IP>:30080
```

---
<br>

## **3️⃣ LoadBalancer (클라우드 환경에서 로드밸런싱)**
✅ **클라우드 환경(GCP, AWS, Azure)에서 사용**  
✅ **고정된 외부 IP를 제공**하여 쉽게 접근 가능  
✅ **트래픽을 여러 노드에 자동 분산하여 부하 분산 기능 제공**  

### **✔ LoadBalancer 트래픽 흐름**
```
(클라이언트) → LoadBalancer IP → 여러 노드(NodePort) → Service → 파드(targetPort)
```

### **✔ LoadBalancer 서비스 예제**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 8080      # Service가 내부에서 받을 포트
      targetPort: 80  # 파드가 실제로 사용하는 포트
```
✅ 외부에서 로드밸런서의 IP를 사용해 접근 가능
```sh
curl http://<LoadBalancer IP>
```

---
<br>

## **4️⃣ NodePort vs ClusterIP vs LoadBalancer 비교**
| Service 타입 | 외부 접근 가능 여부 | 사용 목적 | 트래픽 흐름 |
|--------------|------------------|------------|--------------|
| **ClusterIP** | ❌ (불가능) | 클러스터 내부 통신용 | 내부에서 ClusterIP를 통해 접근 |
| **NodePort** | ✅ (노드 IP 필요) | 외부에서 특정 노드로 접근 가능 | `<노드 IP>:NodePort`로 접근 |
| **LoadBalancer** | ✅ (고정된 IP 제공) | 클라우드 환경에서 부하 분산 | LoadBalancer IP를 통해 자동 분산 |

---
<br>

## **5️⃣ NodePort vs LoadBalancer, LoadBalancer를 사용하는 이유**

| 항목 | NodePort | LoadBalancer |
|------|---------|--------------|
| **외부 접근 방식** | 특정 노드 IP + 포트 사용 | 고정된 외부 IP 제공 |
| **트래픽 분산** | ❌ 없음 (클라이언트가 직접 특정 노드로 요청) | ✅ 여러 노드에 자동 분산 |
| **장애 처리** | ❌ 특정 노드가 다운되면 접근 불가 | ✅ 다른 노드로 자동 전환 가능 |
| **보안 및 확장성** | ❌ 모든 노드에서 포트를 열어야 함 | ✅ 클라우드 보안 및 네트워크 연동 가능 |

📌 **즉, NodePort만 사용하면 트래픽이 특정 노드로 집중될 수 있고, 관리가 어려울 수 있다.**

📌 **반면 LoadBalancer를 사용하면 클라우드 환경에서 자동으로 부하 분산을 수행하고, 특정 IP로 안정적인 외부 접근이 가능하다!** 🚀

