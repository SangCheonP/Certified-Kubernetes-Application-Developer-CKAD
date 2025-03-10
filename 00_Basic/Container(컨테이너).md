# 🐳 컨테이너(Container)란?

## 1. 컨테이너의 개념
컨테이너(Container)는 **애플리케이션과 그 실행에 필요한 모든 라이브러리 및 종속성을 하나의 패키지로 묶어 실행하는 가상화 기술**입니다. 이를 통해 개발 환경과 운영 환경의 일관성을 유지할 수 있으며, 어디서든 동일한 방식으로 실행될 수 있습니다.

컨테이너 기술의 대표적인 구현체로 **Docker**, **Podman**, **containerd** 등이 있습니다.

<br>

## 2. 컨테이너가 나오게 된 배경
과거에는 애플리케이션을 배포하기 위해 운영체제(OS)와 함께 실행할 수 있도록 **가상머신(VM)**을 사용했지만, 이는 리소스 사용량이 크고 배포가 번거로웠습니다.

### 기존 방식의 문제점
1. **로컬 머신(Local Machine)에서의 실행 문제**  
   - "내 로컬에서는 잘 되는데 서버에서는 안 돼요!"
   - 개발 환경과 운영 환경이 다르면 예상치 못한 문제가 발생

2. **가상머신(Virtual Machine, VM)의 비효율성**  
   - 애플리케이션 실행을 위해 운영체제(OS) 전체를 가상화해야 함 → **높은 리소스 소비**
   - VM을 여러 개 실행하면 각 VM마다 별도의 OS가 필요하여 중복 리소스 사용

3. **배포와 확장의 어려움**  
   - 특정 OS 및 환경에 종속적이어서 애플리케이션을 이식하기 어려움
   - CI/CD(Continuous Integration/Continuous Deployment)와 같은 자동화 배포 과정에서 불편함

이러한 문제를 해결하기 위해 **컨테이너(Container) 기술**이 등장하였습니다!

<br>

## 3. 컨테이너 vs 가상머신(VM) vs 로컬 머신 비교

| 비교 항목 | 컨테이너 | 가상머신(VM) | 로컬 머신 |
|-----------|----------|--------------|------------|
| 실행 방식 | OS 커널을 공유하며 독립적인 환경 제공 | 하이퍼바이저를 통해 OS 전체를 가상화 | 운영체제에서 직접 실행 |
| 성능 | **가볍고 빠름** (OS 없이 실행) | 무겁고 느림 (OS 포함) | 가장 빠름 |
| 리소스 사용 | **최소한의 리소스 사용** (커널 공유) | **많은 리소스 소모** (각 VM마다 OS 포함) | 물리적 리소스 직접 사용 |
| 배포 용이성 | **빠른 배포 가능** (이미지 기반) | OS별 설정 필요 | 직접 설치해야 함 |
| 실행 환경 일관성 | **환경 차이 없음** (Docker 이미지 동일) | OS에 따라 다름 | OS 및 설정에 따라 다름 |

👉 **컨테이너는 로컬 머신보다 이동성이 좋고, 가상 머신보다 가볍고 빠른 실행이 가능하다.**

<br>

## 4. 컨테이너의 장점 ✅

1. **가벼움** 🔹  
   - 가상머신(VM)과 달리 운영체제(OS)를 포함하지 않기 때문에 **리소스를 적게 사용**
   - 빠른 시작(부팅 필요 없음)

2. **이식성(Portability)** 🚢  
   - "어디서든 실행 가능!"
   - 개발 환경과 운영 환경이 동일하게 유지됨 ("로컬에서 되는데 서버에서 안 되는 문제" 해결)

3. **확장성(Scalability)** 📈  
   - 마이크로서비스 아키텍처(MSA)와 잘 어울림
   - 필요할 때 빠르게 컨테이너를 추가하여 부하 분산 가능

4. **자동화 배포(CI/CD)와의 궁합** 🤖  
   - 컨테이너 이미지를 사용하여 지속적 배포(Continuous Deployment) 가능
   - Kubernetes 등과 함께 사용하면 서비스 오케스트레이션 가능

5. **의존성 문제 해결** 🔄  
   - 애플리케이션이 필요한 라이브러리 및 환경 설정을 **컨테이너 내부에 포함** → 호스트 환경과 무관하게 실행 가능

<br>

## 5. 컨테이너의 단점 ❌

1. **운영체제(OS) 커널 공유로 인한 보안 문제** 🔐  
   - 여러 컨테이너가 하나의 OS 커널을 공유하기 때문에 보안 취약점이 발생할 가능성이 있음
   - 해결책: 네임스페이스와 cgroups을 활용한 격리 강화

2. **완전한 OS 가상화가 아님** ⚠️  
   - 컨테이너는 커널을 공유하기 때문에 **완전히 독립적인 OS 환경이 필요할 경우 VM이 더 적합**

3. **데이터 영속성 문제** 📦  
   - 컨테이너 내부에서 파일을 저장하면 컨테이너가 삭제될 때 같이 사라짐
   - 해결책: **볼륨(Volume) 및 Persistent Storage** 사용 필요

4. **복잡한 네트워크 설정** 🌐  
   - 컨테이너 간 네트워크 설정이 복잡할 수 있음 (특히 Kubernetes에서 네트워킹 이해 필요)

<br>

## 6. 컨테이너가 Kubernetes와 연결되는 이유 🏗️
컨테이너는 **빠르고 효율적인 실행 환경**을 제공하지만, 컨테이너가 많아지면 **관리**가 어려워짐.
- 컨테이너를 자동으로 배포, 확장, 복구하려면? → **Kubernetes 필요!**
- **Kubernetes(K8s)**는 다수의 컨테이너를 **효율적으로 오케스트레이션(관리)**하는 플랫폼

💡 **즉, 컨테이너는 애플리케이션을 실행하는 단위이고, Kubernetes는 이 컨테이너들을 관리하는 시스템이다.**

<br>

## 7. 결론 🎯
✅ 컨테이너는 **가볍고 빠른 배포**가 가능한 실행 환경을 제공한다.  
✅ 컨테이너를 사용하면 **환경 차이 없이 어디서든 실행 가능**하다.  
✅ 하지만, 데이터 영속성 문제와 네트워크 설정의 복잡성이 존재한다.  
✅ 컨테이너가 많아지면 관리가 어렵기 때문에 Kubernetes 같은 **컨테이너 오케스트레이션 도구**가 필요하다! 🚀

