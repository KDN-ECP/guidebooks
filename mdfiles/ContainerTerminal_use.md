[문서 최종 수정일자 : 2023-05-19]: # 
[문서 최종 수정자 : 신승규]: #

# Container Terminal Use

`CT`서비스 시작한 후 다음과 같은 명령어를 통해 컨테이너를 관리할 수 있습니다.

### 관련 안내서

- [Container 시작하기](./mdfiles/Container_started.md)
- [Container Terminal 시작하기](./mdfiles/Container%20Terminal_started.md)

### 목차

---

##### 애플리케이션 삭제

* 쿠버네티스의 실행중인 모든 리소스 목록 확인

```d
oc get all -o name
```

다음과 같이 실행중인 모든 리소스를 확인할 수 있습니다.

```ㅇ
pod/k-ecp-test-delete-1-build
pod/k-ecp-test-delete-2-build
pod/k-ecp-test-delete-3-build
pod/k-ecp-test-delete-66f54d7694-xbgdt
pod/ssg0412-2-build
pod/ssg0412-3-build
pod/ssg123-1-build
pod/ssg123-585867bcb7-br5gp
...
```

* route 이름이 지정된 리소스의 세부정보 확인

```ㅇ
oc describe route/[app_name]
```

다음과 같이 해당 리소스 **[app_name]** 의 세부정보를 확인할 수 있습니다.

```ㅇ
Name:                   ssssg
Namespace:              ssg-test-del
Created:                2 hours ago
Labels:                 app=ssssg
                        app.kubernetes.io/component=ssssg
                        app.kubernetes.io/instance=ssssg
Annotations:            openshift.io/host.generated=true
Requested Host:         ssssg-ssg-test-del.apps.ocp4.kdnecp.com
                           exposed on router default (host router-default.apps.ocp4.kdnecp.com) 2 hours ago
Path:                   <none>
TLS Termination:        <none>
Insecure Policy:        <none>
Endpoint Port:          8080-tcp

Service:        ssssg
Weight:         100 (100%)
Endpoints:      10.128.9.40:8080
```

* key-value 쌍이 있는 모든 리소스를 확인

```ㅇ
oc get all --selector app=[app_name]
```

다음과 같이 key-value 쌍이 있는 리소스를 확인할 수 있습니다.

```ㅇ
NAME                        TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
service/k-ecp-test-delete   ClusterIP   172.30.13.51   <none>        8080/TCP,8443/TCP,8778/TCP   144m

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/k-ecp-test-delete   1/1     1            1           144m

NAME                                               TYPE     FROM   LATEST
buildconfig.build.openshift.io/k-ecp-test-delete   Source   Git    3

NAME                                           TYPE     FROM          STATUS     STARTED       DURATION
build.build.openshift.io/k-ecp-test-delete-1   Source   Git@5da0df7   Complete   2 hours ago   1m8s
build.build.openshift.io/k-ecp-test-delete-2   Source   Git@5da0df7   Complete   2 hours ago   1m6s
build.build.openshift.io/k-ecp-test-delete-3   Source   Git@5da0df7   Complete   2 hours ago   1m6s

NAME                                               IMAGE REPOSITORY                                                                             TAGS     UPDATED
imagestream.image.openshift.io/k-ecp-test-delete   default-route-openshift-image-registry.apps.ocp4.kdnecp.com/ssg-test-del/k-ecp-test-delete   latest   2 hours ago

NAME                                         HOST/PORT                                             PATH   SERVICES            PORT       TERMINATION   WILDCARD
route.route.openshift.io/k-ecp-test-delete   k-ecp-test-delete-ssg-test-del.apps.ocp4.kdnecp.com          k-ecp-test-delete   8080-tcp                 None
```

* 리소스 삭제 명령어

```ㅇ
oc delete all --selector app=[app_name]
```

다음과 같이 리소스가 삭제된것을 확인할 수 있습니다.

```ㅇ
service "ssg123" deleted
deployment.apps "ssg123" deleted
buildconfig.build.openshift.io "ssg123" deleted
imagestream.image.openshift.io "ssg123" deleted
```

---

##### 애플리케이션 배포

* 애플리케이션 CLI 배포
  
  ```ㅇ
  oc new-app [image] --name [app_name]
  ```

> [imape]는 gitlab의 저장된 코드의 URL를 통해서 업로드 가능합니다.

---

```ㅇ
oc status: 서비스, 배포, 빌드 구성 등 현재 프로젝트에 대한 정보 확인
```

oc status

```
```주절
In project SSG-TEST (ssg-test-del) on server https://api.ocp4.kdnecp.com:6443

http://ssgtest-ssg-test-del.apps.ocp4.kdnecp.com (svc/ssgtest)
  dc/ssgtest deploys istag/ssgtest:latest <-
    bc/ssgtest source builds http://10.100.11.114/222216/k-ecp-test-delete.git on openshift/jboss-webserver56-openjdk11-tomcat9-openshift-ubi8:5.6.0
    deployment #4 deployed 3 days ago - 1 pod
    deployment #3 deployed 3 days ago
    deployment #2 deployed 3 days ago
```

- `$oc projects`: 내 프로젝트

- `$oc project [project_name]`: 프로젝트[project_name] 선택

```주절이
oc projects
Using Project "[project_name]" on server "https://api.ocp4.kdnecp.com:6443"
```

* 
- `oc get pod`: Pod 정보 확인

```주절이
oc get pod
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

- `oc rsh [pod_name]`:컨테이너 내의 [pod_name]로 접속

```주절이
$oc rsh [pod_name]
oc rsh [pod_name]>
```
