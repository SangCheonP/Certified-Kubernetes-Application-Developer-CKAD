# 3. Docker vs ContainerD
## Docker란?
Docker는 **컨테이너 기반 가상화 플랫폼**으로, 컨테이너의 빌드, 배포, 실행을 쉽게 할 수 있도록 다양한 도구를 제공합니다. 컨테이너를 관리할 수 있는 CLI(Command Line Interface) 및 Docker Compose, Swarm과 같은 오케스트레이션 기능도 포함되어 있습니다.

## ContainerD란?
ContainerD는 **Docker에서 파생된 컨테이너 런타임**으로, 컨테이너의 실행 및 관리 기능을 담당하는 경량화된 도구입니다. Docker 엔진의 핵심 런타임 역할을 수행하며, Kubernetes 등에서도 독립적인 런타임으로 사용됩니다.

## Docker와 ContainerD 비교
| 비교 항목 | Docker | ContainerD |
|-----------|--------|------------|
| 개념 | 컨테이너 관리 플랫폼 | 컨테이너 런타임 |
| 기능 | 빌드, 실행, 네트워크 관리 등 포함 | 컨테이너 실행 및 관리만 수행 |
| 사용 방식 | CLI와 UI를 제공하여 개발자가 쉽게 사용 가능 | Kubernetes 및 기타 오케스트레이션 툴에서 주로 사용 |
| 확장성 | Docker Swarm, Compose 등을 제공 | Kubernetes에서 기본 런타임으로 사용됨 |

👉 **Docker는 개발자가 컨테이너를 쉽게 사용하도록 제공하는 도구이며, ContainerD는 컨테이너 실행 자체에 초점을 맞춘 런타임이다.**
