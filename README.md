# AWS 강의

## IAM

JSON 형식으로 작성. 작성되지 않은 내용은 모두 차단된다. 

### IAM 정책

#### 권한 경계

IAM 사용자 또는 역할에 최대 권한을 제한. 

### IAM 역할

AWS리소스에서 사용하는 자격증명. 정책을 연결해서 IAM역할에 작업 수행에 필요한 권한을 부여

#### 신뢰정책

>  IAM 역할을 사용하여 AWS 계정간 접근 권한 위임을 하는 기능. 

특정 AWS 서비스가 다른 AWS 서비스에 접근할 때 사용하는 기능. 예를 들면, 개발서버와 운영서버가 있을 때, 일시적으로 운영서버에 대한 코드 푸시 권한을 개발 계정을 가진 사용자에게 부여해야 한다고 하자. 이 때 사용할 수 있는 솔루션은 운영계정에 IAM역할을 생성한 다음, 개발 계정에 신뢰정책을 정의하면 된다.

#### IAM 역할 예제

IAM > 역할 > 필요한 서비스 선택

EC2에서 실행되는 앱이 RDS, S3와 같은 다른 서비스에 접근하려고 할 때 EC2에 정책을 추가한 IAM 권한을 주면 된다.

##### EC2에 IAM 추가 방법

인스턴스 생성 시, 고급 프로파일에 들어가서 IAM 인스턴스 프로파일에 생성한 IAM 역할을 추가해주면 된다.



### IAM 보안 도구

##### IAM 자격증명 보고서

생성된 계정과 관련한 패스워드, 엑세스 키 등의 정보를 볼 수 있다.

##### IAM 엑세스 관리자

서비스 별로 어떤 접근 권한을 가지고 있는지 알 수 있다.



#### IAM 정책 시뮬레이터

> AWS 계정의 IAM 사용자, 사용자 그룹 또는 역할에 연결된 정책을 테스트

어떤 IAM A(계정, 역할, RULE-정책)가 B action 권한을 가지고 있는지 확인 가능하다. 예를 들면 A 계정이 이미지 생성 action을 할 수 있는지와 같은 확인을 할 수 있다. 



## EC2

>  클라우드 컴퓨팅 서비스.

리소스. 인스턴스, 로드밸런서, 배치그룹 등이 있다. 태그를 추가할 수 있다. 애플리케이션 이미지가 있다. 인스턴스 유형을 선택할 수 있다. 키페어로 원격접속 시 사용가능한 키 파일 등을 생성할 수 있다. 네트워크 설정을 할 수 있다. 보안 그룹을 선택할 수 있다. 보안 그룹을 통해 접근 규칙 등을 선택할 수 있다. 스토리지 구성을 이용해 가상 컴퓨터가 사용할 스토리지를 선택할 수 있다. 공유파일을 위한 파일 시스템도 선택할 수 있다.

- 사용자 데이터

인스턴스를 실행할 때, 여기에 스크립트가 있으면 해당 스크립트를 실행함.

### 연결방식

#### 1. SSH

키 파일을 이용해 커맨드 프롬프트를 이용해 접근이 가능하다. 22번 포트로 접속하기 때문에 22번 포트 방화벽 오픈이 필요하다.

키파일이 너무 많은 로컬 사용자에게 오픈되어 있는 경우 접근이 차단될 수 있다.

=> 파일 속성에 상속사용안함 선택하고 현재사용자만 추가해주자.

#### 2. RDP (Windows only)

원도우즈 OS의 RDP 프로토콜을 이용한다. 접속할 때 암호는 EC2 콘솔에서 키파일을 이용해 암호를 생성할 수 있다.

인스턴스 > 인스턴스 ID선택 > 인스턴스에 연결 선택 >

\> RDP 클라이언트 > 암호가져오기 선택해서 키파일 데이터 업로드하면 암호화 키 생성할 수 있음.

#### 3. Instance Connect

리눅스만 가능. Ec2 `연결` 선택하면 된다. 

원격 연결 불가시 리눅스 > 보안 > 22번 포트 오픈인지 확인

### 인스턴스 유형

인스턴스 유형에 따라 컴퓨터 하드웨어가 결정. CPU, 메모리, 스토리지, 네트워크 용량의 여러 조합으로 구성. 컴퓨터 스펙을 선택한다고 보면 된다. 

### 네트워크 인터페이스

`ENI` 가상 네트웨크 인터페이스. 인스턴스에 가상 IP, MAC 주소를 준다. 

인스턴스 > 네트워킹 tab. 보안그룹관 연결이 됨. 서브넷은 인스턴스와 동일한 가용영역에 있어여 함. 하나의 인스턴스에 2개 이상의 네트워크 인터페이스를 줄 수 있다.

### 보안그룹

EC2 인스턴스에 대한 인바운드 아웃바운드 트래픽을 제어하는 가상 방화벽 역할을 한다. 인스턴스의 네트워크 인터페이스와 연결됨. `상태저장 방화벽`이다. 허용 규칙만 지정 가능하고 거부 규칙은 지정할 수 없음. 

#### 상태저장 방화벽

아웃바운드 규칙 상관 없이 허용된 인바운드 트래픽에 대한 반응으로 외부로 나가는 흐름이 수행. 내가 이해한 바로는 정당한 인바운드 요청에 대해 아웃바운드 트래픽(예_http 응답)은 허용된다. 마찬가지로 아웃바운드 요청이 EC2에서 먼저 발생했다면 그에 맞는 응답은 인바운드 요청 관계없이 허용된다. 

### 탄력적 IP

인스턴스를 생성할 때 할당받은 public IP는 인스턴스를 재시작하면 다른 IP로 할당 받아 주소가 변경된다. Elastic IP를 할당 받으면 인스턴스를 다시 시작해도 고정 IP를 받아서 쓸 수 있다. 탄력적 IP는 유료 서비스이다.

### AMI

> 아마존 머신 이미지. 

3가지 종류가 있다. 아마존에서 제공하는 기본 이미지, 마켓플레이스, 사용자 커스텀 AMI가 있다. elastic block store에서 스냅샷을 만들어서 내가 만들어둔 이미지를 저장할 수 있다.  

### 배치그룹

인스턴스를 물리적으로 어디에 배치할지. 

#### 클러스터 배치그룹

인접한 물리적 서버끼리 배치해서 네트워크 지연시간이 매우 짧아서(**고속 네트워크 연결**) 고성능 컴퓨팅에 적합하다.

#### 파티션 배치그룹

하드웨어를 파티션을 그룹화. 파티션끼리는 서로 다른 하드웨어 사용. 하드웨어를 공유하지 않기 때문에 하나의 하드웨어 장애 발생 시 다른 하드웨어에 영향 없음.  하둡 등의 빅데이터 분산처리 시스템에 활용. 

#### 분산형 배치 그룹

같은 서버랙의 서버를 그룹화. 서버랙 단위로 장애가 발생했을 때 영향도 없음. 매우 중요하고 고가용성이 필요한 애플리케이션에 적합.

### EC2 라이프 사이클

시작: 인스턴스 전원을 켜는 것

재부팅 : 인스턴스 재부팅

중지 : 인스턴스 전월을 끄는 것

종료 : 인스턴스 삭제

`최대절전모드` : RAM에 있는 애플리케이션 상태를 저장 후 중지상태 전환 노트북에서 종료를 안하고 스크린만 닫을 때와 동일. 서버 부팅 시 프로세스 로드시간이 오래걸리는 시스템의 경우 유용.



## Elastic Load Balancing

### 확장성

- 수직적 확장 : 자원을 추가하는 방식 (사양 업그레이드)

- 수평적 확장 :  노드를 추가하는 방식. EC2 인스턴스 개수를 늘리는 것.

고가용성 : 이중화를 통해 가용영역 하나에 장애가 발생해도 가용성 유지 가능

### 로드밸런서 종류

애플리케이션, 네트워크, 게이트웨이 로드밸런서

#### 애플리케이션 

7계층에서 사용

#### 네트워크 

4계층에서 사용. TCP, UDP 프로토콜. 대용량, 게이밍과 같은 데이터를 처리할 때 사용

#### 게이트웨이

 보안검사를 위한 용도로 사용



### 구성 (게이트웨이 제외)

생성방법은 

1. 로드밸런싱 > 대상그룹을 선택하면 생성할 수 있다.
2. 로드밸런서를 생성하여 등록해주면 로드밸런서를 사용할 수 있다.  

#### 1. 리스너 

client, 타켓그룹 사이 연결을 위해 프로토콜, 포트번호로 구성. 

#### 2. 타켓그룹

타켓은 EC2, 오토 스케일링 그룹, IP주소, 람다, 앱 로드밸런서 등등이 있다. `개별 인스턴스`, `인스턴스 여러개(오토스케일링)`, `IP 주소`, `람다`, `다른 앱 로드밸런서` 등등이 있음.

앱 로드밸런서는 웹 트래픽을 다루기 때문에 http, https 프로토콜, 네트워크는 TCP, TLS, UDP 등을 사용한다. 게이트웨이는 `제네브` 프로토콜 사용

##### 상태검사 

대상의 상태 확인



#### 3. 타켓그룹 종류

##### 인스턴스

EC2 오토 스케일링

##### IP 주소

로컬 VPC CIDR

public IP 주소

##### 람다

##### 앱 로드 밸런서



#### 타켓그룹 속성

1. 등록취소지연 : 요청이 없어서 deregisteration된 인스턴스에 더 이상의 요청을 보내지 않는 기능
2. slow start duration : 보내는 요청 수를 선형으로 증가시킴
3. 알고리즘 : 라운드 로빈 (일정 시간 라우팅 변경), 최소 미해결 요청(처리하고 있는 요청이 가장 작은 대상에 라우팅)
4. 고정 : 클라이언트가 로드밸런서를 통해 특정 대상에 연결되었을 때 해당 클라이언트는 해당 대상에 계속 요청을 하도록 고정. 세션과 쿠키를 이용해 구현
5. 등록해제시 연결종료 : 등록취소 지연에 도달했을 때 네트워크 로드밸런서가 활성 연결 종료



### ALB

리스너 규칙

- host header : 호스트 이름을 기반으로 라우팅
- path : URL 경로에 패턴 기반
- http header : 요청의 http 헤더기반
- http request method : 요청 메소드 기반 라우팅
- query string : 쿼리 문자열 키-value 페어
- source IP : 각 요청의 소스 IP주소 기반 라우팅

각 규칙을 타겟그룹을 연결해주면 된다.

** HTTPS는 ACM 서비스를 이용해 인증서를 발급 받고 적용을 하고나면 사용할 수 있다.



### NLB

TCP, UDP, TLS 요청을 로드밸런싱해야 하는 경우 사용. 고정 IP 주소할당 가능. 클라이언트 IP주소를 전달할 수 있다. 리스너 규칙설정이 없음. 수백만 대용량 처리에 적합

프로토콜 : TCP, TCP/UDP, TLS 사용

데이터 전송보안을 위해 SSL/TLS 인증서를 배포해야 하고 인증서는 ACM 또는 클라이언트 인증서를 사용해야 한다.



## EC2 Auto Scaling

원하는 용량, 최소용량, 최대용량을 설정할 수 있다. 구성요소로 오토스케일링 그룹, 시작 템플릿(AMI), 조정옵션(오토스케일링을 실행하기 위한 조건) 등이 있다. 

### 조정정책

수동조정, 일정기반 조정, 온디맨드 기반 조정, 예측 조정 사용(머신러닝, cloud watch) 등의 정책이 있음. `항상 현재 인스턴스 수준 유지 관리`정책은 지정된 갯수의 실행인스턴스를 유지하도록 하는 정책

#### 동적 조정

대상 추척 조정 : 지정한 지표에 도달하면 그룹 확장 (CPU, 바이트 수 등등)

단계 조정 : 클라우드 워치 기반. 크기조정 활동이 진행되는 기간에도 경보에 응답.

단순 조정 : 클라우드 워치 기반. 크기조정 활동이 진행되는 기간에는 경보에 응답 안함

아마존 SQS 기반 크기 조정 : 대기열의 시스템 로드 변경에 따라 오토 스케일링 그룹 조정

#### 조정 쿨다운

증가/감소하는 활동이 발생하면 쿨다운 기간을 가짐. 부팅하는 동안에는 CPU사용량이 증가할 수 밖에 없기 때문.

#### 수명주기 후크

이벤트를 인식한 다음 해당수명주기 이벤트가 발생할 때 사용자 지정 작업을 수행

인스턴스가 `인서비스 상태 가기전`, `터미네이티드` 상태에 가지전 특정 작업을 수행하도록 함. 예를 들면 로깅. 백업 등등 



![lifecycle_hooks](.\lifecycle_hooks.png)





### 실습

1. 인스턴스 > 시작 템플릿

시작 템플릿을 만들 수 있음.

2. auto scaling > auto scaling 그룹 생성



























