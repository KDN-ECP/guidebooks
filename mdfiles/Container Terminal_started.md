[문서 최종 수정일자 : 2023-05-05]: # 
[문서 최종 수정자 : 류정호]: # 

# Container Terminal 시작하기

이 안내서를 사용하여 **K-ECP Container Terminal(이하. CT)*** 서비스를 시작 하십시오. CT의 서비스 신청서를 작성하고 연결하는 방법을 안내합니다. 

CT는 K-ECP에서 컨테이너를 관리하고 오케스트레이션 할 수 있는 터미널 입니다. CT에 접속하여 K-ECP OpenShift CLI인 `oc` 명령어와 Kubernetes 클러스터 작업을 위한 명령어인 `kubectl`를 사용하실 수 있습니다. 

### 관련 안내서

* Project 만들기
* SSL VPN 시작하기
* Container 시작하기

### 목차

[개요](#abstract)

[전제 조건](#precondition)

[1단계: Container Terminal 서비스 신청](#step1)

[2단계: Container Terminal 접속](#step2)

[3단계: Container Terminal 사용](#step3)

[4단계:Container Terminal 정리](#step4)

[다음 단계](#nextstep)

---

<span id="abstract"/>

## 개요

K-ECP CT 서비스를 사용하기 위해서는 아래와 같은 프로세스로 진행되며, **KDN의 직원일 경우 User Console에서 소속 부서장의 결재**가 필요합니다.

```mermaid
sequenceDiagram
  actor 사용자(일반)
  actor 사용자(KDN직원)
  actor KDN부서장
  사용자(일반) ->>+ K-ECP: CT 사용신청
  K-ECP ->>- 사용자(일반): CT 제공
  사용자(KDN직원) -->> KDN부서장: CT 사용신청 승인요청?
  Note over 사용자(KDN직원), KDN부서장: CT 사용자가 KDN직원일 경우<br/>User Console를 통하여<br/>소속 부서장이 결재 진행.
  KDN부서장 -->>+ K-ECP: [결재완료] CT 사용신청
  K-ECP -->>- 사용자(KDN직원): CT 제공
```

K-ECP CT는 User Console를 통해 신청한 후 최종 승인 시 가상서버 형태로 제공 되며, 아래 개념도와 같이 **SSL-VPN 또는 전용선(Direct Connect 서비스 사용시)을 이용하여 접속**하실 수 있습니다.

  ![개념도](./../resource/concept_ct.png "서비스 개념도")

CT 접속 후 CLI 명령어인 `oc`를 통해 Container Project를 위한 다음과 같은 다양한 작업을 수행할 수 있습니다.

* 프로젝트 소스 코드로 직접 작업
* K-ECP Container Platform 작업 스크립팅
* 애플리케이션 빌드, 배포 및 관리

---

<span id="precondition"/>

## 전제 조건

* 시작하기 전에 [K-ECP User Console](https://kecp.kdn.com/mbr/ "인터넷에서 접속 시")에 회원가입이 되어 있어야 합니다. 

* 사전에 CT를 신청할 프로젝트가 생성되어 있어야 합니다. 

> :bulb: **Tip:** KDN 직원의 경우  KDN 내부망에서 [KDN 전용 User Console](http://kdnecp.kdn.com:8585/mbr/ "KDN 내부망에서 접속 시")로도 접속이 가능합니다.

---

<span id="step1"/>

## 1단계: Container Terminal 서비스 신청

1. K-ECP User Console에서 `[서비스 신청] 자원 > 컨테이너 신청 > Container Terminal 신청`의 돋보기 아이콘 클릭

2. 서비스 신청서 내역 작성 
   
   > :bell: **안내:** CT는 VM Server 형태로 제공되며, 프로젝트 당 1개를 초과할 수 없습니다.
   
   * 프로젝트명: *CT가 포함되어야 될 기 생성완료된 프로젝트 선택*
   
   * 서버대역: *CT에 할당될 IP Subnet 대역 선택* 

3. `신청` 버튼을 클릭 하여 CT 서비스 신청 (단, KDN 직원일 경우 소속 부서장으로 결재자 지정 후 서비스 신청)

---

<span id="step2"/>

## 2단계: Container Terminal 접속

> :warning: **주의사항:** K-ECP SSL VPN이 사전에 신청되어 있어야 합니다.

1. CT 서비스 신청 승인이 완료 된 경우 K-ECP User Console에서 `서비스 현황 > 가상서버`로 이동하여 해당 프로젝트 선택

2. 선택된 프로젝트내에 VM Server형태로 생성된 CT의 **IP 주소 확인**

3. `[서비스 신청] 기술지원 > 관리서비스 신청 > 보안작업 서비스 신청`의 돋보기 아이콘 클릭

4. 보안적업 서비스 신청정보 기술 후 신청 요청
   
   * 클러스터: *CT가 생성된 클러스터 선택*
   
   * 프로젝트명: *CT가 생성된 프로젝트 선택*
   
   * 가상서버: *해당 CT 선택*
   
   * 서비스명: *`SSL VPN 접속 가능서버 추가 요청` 기술*

5. K-ECP 운영팀으로 부터 보안작업 완료 통보를 받은 후 인터넷 가능환경에서 브라우저를 통해 [K-ECP SSL VPN](https://kecp-vpn.kdn.com/) 접속 

> :bell: **안내:** SSL VPN 접속방법 및 사용법은 `SSL VPN 시작하기` 가이드 문서 참고

6. K-ECP 운영팀으로 부터 초기 ID/PW을 전달 받은 후 SSH 프로토콜을 통해 서버 접속
   
   ```주절
   $ssh -p 10040 kecpuser@[CT_IP]
   ```

> :bell: **안내:** 접속 후 초기PW 변경 필요

---

<span id="step3"/>

## 3단계: Container Terminal 사용

1. K-ECP 운영팀에게 OpenShift의 원하는 PW 전달

> :bell:**안내**:원하는 PW가 없는 경우 K-ECP 운영팀에서 임의 설정

2. 접속한 CT서버에서 OpenShift 로그인

```주절
$oc login -u [ID] https://api.ocp4.kdnecp.com:6443
```

3. Openshift 접속 확인
* 접속확인

```주절이
Login successful.
You have one project on this server: "[server_name]"
Using project "[server_name]".
```

* 접속 계정 확인 명령어

```주절이
$oc whoami
```

```주절이
[ID]
```

---

<span id="step4"/>

## 4단계: Container Terminal 정리

1. `oc`명령어를 통해 CT에서 OpenShift계정 로그아웃을 할 수 있습니다.
   
   ```주절이
   $oc logout
   ```
   
   ```주절이
   Logged "[ID]" out on "https:api.ocp4.kdnecp.com:6443"
   ```

2. CT자원을 완전히 삭제하길 원하는 경우 `서비스 변경 및 삭제 신청 > 삭제신청`로 이동
* 프로젝트: 해당`CT`가 있는 프로젝트 선택

* 자원: 가상서버, `CT`의 서버 선택

* `x삭제신청` 버튼 클릭  

> :warning:**주의사항**:CT 서비스를 삭제할 경우 OpenShift계정 또한 삭제됩니다.

---

<span id="nextstep"/>

## 다음 단계

`CT`서비스 시작한 후 다음과 같은 명령어를 통해 컨테이너를 관리할 수 있습니다.

* `$oc status`: 서비스, 배포, 빌드 구성 등 현재 프로젝트에 대한 정보 확인

```주절
$oc status
In project SSG-TEST (ssg-test-del) on server https://api.ocp4.kdnecp.com:6443

http://ssgtest-ssg-test-del.apps.ocp4.kdnecp.com (svc/ssgtest)
  dc/ssgtest deploys istag/ssgtest:latest <-
    bc/ssgtest source builds http://10.100.11.114/222216/k-ecp-test-delete.git on openshift/jboss-webserver56-openjdk11-tomcat9-openshift-ubi8:5.6.0
    deployment #4 deployed 3 days ago - 1 pod
    deployment #3 deployed 3 days ago
    deployment #2 deployed 3 days ago
```

* `$oc projects`: 내 프로젝트

* `$oc project [project_name]`: 프로젝트[project_name] 선택

```주절이
$oc projects
Using Project "[project_name]" on server "https://api.ocp4.kdnecp.com:6443"
```

* `oc get pod`: Pod 정보 확인

```주절이
$oc get pod
NAME               READY   STATUS      RESTARTS   AGE
ssg0412-2-build    0/1     Completed   0          32d
ssg0412-3-build    0/1     Completed   0          32d
ssgtest-1-build    0/1     Completed   0          20d
ssgtest-1-deploy   0/1     Completed   0          20d
ssgtest-2-build    0/1     Completed   0          3d20h
ssgtest-2-deploy   0/1     Completed   0          3d20h
ssgtest-3-build    0/1     Completed   0          3d20h
ssgtest-3-deploy   0/1     Completed   0          3d20h
ssgtest-4-465g4    1/1     Running     0          3d6h
ssgtest-4-build    0/1     Completed   0          3d6h
ssgtest-4-deploy   0/1     Completed   0          3d6h
```

* `oc rsh [pod_name]`:컨테이너 내의 [pod_name]로 접속

```주절이
$oc rsh [pod_name]
oc rsh [pod_name]>
```
