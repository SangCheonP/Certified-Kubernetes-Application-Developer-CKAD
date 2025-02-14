## YAML 문법 정리

### 1. **기본 문법**
#### (1) 키-값 쌍 (Key-Value Pair)
```yaml
name: John Doe
age: 30
married: true
```
- `:`(콜론) 뒤에는 공백이 있어야 함
- 값은 문자열, 숫자, 불리언(`true` / `false`) 등을 사용 가능

---

#### (2) 리스트 (List)
```yaml
hobbies:
  - Reading
  - Cycling
  - Hiking
```
또는 한 줄로 표현 가능
```yaml
hobbies: [Reading, Cycling, Hiking]
```
- 리스트 항목은 `-`(하이픈)으로 시작하며, 들여쓰기를 맞춰야 함

---

#### (3) 중첩 구조 (Nested Structure)
```yaml
address:
  city: Seoul
  zip: 12345
```
또는 한 줄로 표현 가능
```yaml
address: {city: Seoul, zip: 12345}
```
- `:`을 사용해 중첩된 구조를 표현할 수 있음

---

### 2. **데이터 타입**
#### (1) 문자열 (String)
```yaml
message: "Hello, World!"
```
- 작은 따옴표(`'`) 또는 큰 따옴표(`"`) 사용 가능
- 큰 따옴표(`"`) 사용 시 `\n` 같은 이스케이프 문자 가능
- 여러 줄 문자열(`|`, `>`) 사용 가능
```yaml
description: |
  이 문자열은 여러 줄로 작성할 수 있습니다.
  줄바꿈이 그대로 유지됩니다.
```
```yaml
description: >
  이 문자열은 여러 줄로 작성할 수 있습니다.
  줄바꿈이 자동으로 공백으로 변환됩니다.
```

---

#### (2) 숫자 (Integer, Float)
```yaml
integer: 42
float: 3.14
```

---

#### (3) 불리언 (Boolean)
```yaml
is_active: true
is_admin: False
```
- 대소문자 구별 없이 `true/false`, `True/False`, `yes/no` 가능

---

#### (4) `null` 값
```yaml
empty_value: null
```
- `null`, `~`, `""`(빈 문자열) 사용 가능

---

### 3. **주석 (Comment)**
```yaml
# 이것은 주석입니다.
name: John Doe  # 이 부분도 주석 가능
```
- `#`을 사용하여 주석 작성 가능

---

### 4. **리스트의 원소가 객체(Map)인 경우**
#### ✅ 원소가 객체(Map)인 리스트 예제
```yaml
containers:
  - name: my-app
    image: my-app:latest
    ports:
      - containerPort: 8080
```
- `containers`는 리스트이고, `- name: my-app`부터 하나의 **객체(Map, Dictionary)**  
- `name`, `image`, `ports`는 객체의 속성 (Key-Value Pair)  
- `ports`는 리스트이므로 다시 `-`을 사용

#### ✅ 리스트에 여러 객체가 있는 경우
```yaml
containers:
  - name: my-app
    image: my-app:latest
    ports:
      - containerPort: 8080

  - name: another-app
    image: another-app:latest
    ports:
      - containerPort: 9090
```
- `containers` 리스트 안에 두 개의 객체(`my-app`, `another-app`)가 존재

---

### 5. **앵커(&)와 별표(*) - 중복 제거**
```yaml
default: &default_settings
  retries: 3
  timeout: 30

server1:
  <<: *default_settings
  host: server1.com

server2:
  <<: *default_settings
  host: server2.com
```
- `&이름`으로 앵커 정의 후, `*이름`으로 재사용 가능
- `<<: *이름`을 사용하면 기존 값을 가져와서 확장 가능

---

### 6. **환경 변수 사용**
```yaml
database:
  user: ${DB_USER}
  password: ${DB_PASSWORD}
```
- `${}`를 사용해 환경 변수 참조 가능 (일부 시스템에서 지원)

---

### 7. **사용 예시**
#### ✅ Docker Compose 파일 예시
```yaml
version: '3'
services:
  app:
    image: my-app
    ports:
      - "8080:8080"
    environment:
      - ENV=production
    depends_on:
      - db

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

#### ✅ Kubernetes Deployment 예시
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
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
        - name: my-app
          image: my-app:latest
          ports:
            - containerPort: 8080
```

---

### **요약**
1. **들여쓰기**: 스페이스 2칸 또는 4칸(탭 금지)
2. **데이터 타입**: 문자열, 숫자, 불리언, 리스트, 맵 지원
3. **리스트**: `-` 사용 (여러 개 나열 가능)
4. **맵(딕셔너리)**: `key: value` 형태
5. **주석**: `#` 사용
6. **리스트 안의 객체(Map)**: `-`로 시작하는 객체 안에 여러 속성 포함 가능
7. **중복 제거**: `&앵커`, `*별표` 활용
8. **환경 변수**: `${}` 활용 가능 (일부 시스템)
