# 🏗️ 쿠버네티스(Kubernetes) 아키텍처

![KakaoTalk_20250212_161425905](https://github.com/user-attachments/assets/944372e6-9a57-4d56-af79-f21ca24d2289)


## 1️⃣ 쿠버네티스 클러스터 개요
쿠버네티스(Kubernetes)는 **컨테이너화된 애플리케이션을 자동으로 배포, 운영, 확장할 수 있도록 관리하는 오케스트레이션 시스템**입니다.

쿠버네티스 클러스터는 크게 두 가지 주요 구성 요소로 나뉩니다.

- **🖥️ 마스터 노드(Master Node)**: 클러스터 제어 및 관리
- **⚙️ 워커 노드(Worker Node)**: 실제 컨테이너 실행

---

## 2️⃣ 마스터 노드 (Master Node)
마스터 노드는 **클러스터의 중앙 제어 역할**을 합니다. 주요 컴포넌트는 다음과 같습니다.

### **🛠️ 주요 컴포넌트**
1. **kube-apiserver (API 서버)**  
   - 클러스터의 **중앙 관제소** 역할
   - `kubectl` 명령어를 통해 API 요청을 처리

2. **etcd (분산 데이터 저장소)**  
   - 클러스터의 모든 상태 정보를 저장하는 데이터베이스

3. **kube-controller-manager (컨트롤러 매니저)**  
   - 클러스터 상태를 모니터링하고 자동으로 조정
   - Deployment, ReplicaSet, HPA 등의 **컨트롤러 관리**

4. **kube-scheduler (스케줄러)**  
   - 새로운 Pod를 적절한 Worker Node에 할당

5. **kube-proxy (네트워크 관리)**  
   - 클러스터 내부 및 외부 네트워크 트래픽을 관리

---

## 3️⃣ 워커 노드 (Worker Node)
Worker Node는 **실제 컨테이너(애플리케이션)가 실행되는 노드**입니다. 각 Worker Node에는 다음과 같은 핵심 컴포넌트가 있습니다.

### **🔧 주요 컴포넌트**
1. **kubelet**  
   - Worker Node에서 **컨테이너 실행을 담당**
   - API 서버의 명령을 받아 컨테이너를 실행 및 관리

2. **container runtime (컨테이너 런타임, 예: containerd)**  
   - 컨테이너를 실행하는 엔진 (Docker, Containerd, CRI-O 등)
   - kubelet이 컨테이너를 실행하도록 요청하면 실제 실행을 담당

3. **kube-proxy**  
   - Worker Node에서 네트워크 트래픽을 관리하여 Pod 간 통신을 가능하게 함

---

## 4️⃣ Kubernetes 리소스 관리
쿠버네티스에서는 **다양한 리소스를 사용하여 컨테이너를 관리**합니다.

### **📌 주요 리소스 개념**
1. **컨트롤러 (Controller)**: Pod를 관리하는 상위 개념
   - **Deployment**: 애플리케이션 배포 및 장애 발생 시 자동 복구
   - **ReplicaSet**: 동일한 Pod 여러 개를 유지
   - **HPA (Horizontal Pod Autoscaler)**: CPU 사용량에 따라 자동 확장

2. **오브젝트 (Object)**: 실제 리소스
   - **Pod**: 컨테이너를 실행하는 최소 단위
   - **Service**: Pod의 네트워크 접근을 제공
   - **ConfigMap & Secret**: 환경 변수 및 보안 설정 저장
   - **PVC (Persistent Volume Claim)**: 저장 공간을 사용하는 리소스

---

## 5️⃣ 스토리지 및 볼륨 관리
컨테이너는 기본적으로 휘발성이므로, 데이터를 영구적으로 저장하기 위해 **Persistent Volume(PV)**이 필요합니다.

### **📂 볼륨 관리 개념**
- **PV (Persistent Volume)**: 클러스터가 관리하는 영구 저장소
- **PVC (Persistent Volume Claim)**: 애플리케이션이 PV를 요청하는 방식
- **스토리지 솔루션**: Ceph, NFS, AWS EBS 등의 외부 저장소 지원

---

## 6️⃣ 쿠버네티스 설치 및 실행 과정
### **✅ Master Node 설정**
1. `kubeadm`, `kubectl`, `kubelet` 설치
2. `kubeadm init` 실행 후 마스터 노드 설정

### **✅ Worker Node 클러스터 조인**
1. `kubeadm join` 명령어로 Worker Node를 클러스터에 추가
2. `container runtime` 설치하여 컨테이너 실행 환경 구성

---

## 7️⃣ 쿠버네티스 핵심 정리
✔ **Master Node**는 클러스터를 **관리**하고, **Worker Node**는 실제 **애플리케이션 실행**  
✔ **API 서버 → 컨트롤러 → 스케줄러 → kubelet → 컨테이너 런타임(containerd)** 흐름으로 동작  
✔ **Deployment, Pod, Service, PVC 같은 리소스를 사용해 컨테이너 관리**  
✔ **쿠버네티스는 컨테이너 기반 애플리케이션을 자동으로 배포, 확장, 운영**  

---

이제 기본적인 쿠버네티스 아키텍처를 이해했어요! 🚀  
다음 단계로 `kubectl` 명령어를 사용하여 **실제 Pod를 생성하고 관리하는 실습**을 해보는 것을 추천합니다! 😊
