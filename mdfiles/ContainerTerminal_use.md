[문서 최종 수정일자 : 2023-05-19]: # 
[문서 최종 수정자 : 신승규]: #

# Container Terminal Use

`CT`서비스 시작한 후 다음과 같은 명령어를 통해 컨테이너를 관리할 수 있습니다.

### 관련 안내서

- [Container 시작하기](./mdfiles/Container_started.md)
- [Container Terminal 시작하기](./mdfiles/Container%20Terminal_started.md)

### 목차

[1. podman 컨테이너 실행](#step1)
[2. podman 컨테이너 접속](#step2)
[3. podman 컨테이너 삭제](step3)
[4. oc 명령어](#step4)

---

## 1. podman 컨테이너 실행

> :bulb:**안내**: 도커이미지가 CT 서버에 준비되어 있어야 합니다.

1. ssh 명령어를 통한 CT 서버 접속

```아오
ssh -p 10040 [CT_IP]
```

2. podman 명렁어를 통한 image 등록

```아오
podman images
```

* images에 비어 있음을 확인  

```아오
REPOSITORY  TAG         IMAGE ID    CREATED     SIZE
```

* docker image를 압축한 tar파일이 있는 위치로 이동
  
  ```아오
  ls
  nginx.tar
  ```아오
  ```

* 이후 podman 명령어를 통해 tar파일을 다시 docker image로 변환
  
  ```아오
  podman load -i nginx.tar
  ```
  
  ```아오
  Getting image source signatures
  Copying blob 434c6a715c30 done
  Copying blob 9fdfd12bc85b done
  Copying blob b821d93f6666 done
  Copying blob 24839d45ca45 done
  Copying blob f36897eea34d done
  Copying blob 1998c5cd2230 done
  Copying blob 3c9d04c9ebd5 done
  Copying config 021283c8eb done
  Writing manifest to image destination
  Storing signatures
  Loaded image(s): localhost/nginx:latest
  ```
3. 등록된 이미지 확인

```아오
podman images
```

```dkdh
REPOSITORY       TAG         IMAGE ID      CREATED      SIZE
localhost/nginx  latest      021283c8eb95  2 weeks ago  191 MB
```

4. 컨테이너 이미지 생성
* localhost/nginx 이미지를 기반으로 my-nginx라는 새 컨테이너를 생성하고 호스트 시스템에서 포트 8000을 노출시킵니다.
  
  ```아오
  podman run -d -p 8000:80 --name my-nginx localhost/nginx:latest
  ```

* 컨테이너 정보 확인
  
  ```dkdh
  8709140107b42a40470205a451e3f1859549e274ecef87f53f71470889bb8d0a
  ```
5. 컨테이너 실행 확인
   
   ```아오
   podman ps
   ```
   
   ```아오
   CONTAINER ID  IMAGE                   COMMAND               CREATED        STATUS            PORTS                 NAMES
   8709140107b4  localhost/nginx:latest  nginx -g daemon o...  6 minutes ago  Up 6 minutes ago  0.0.0.0:8000->80/tcp  my-nginx
   ```

6. 컨테이너 중지
   
   ```아오
   podman stop my-nginx
   ```
   
   ```dkdh
   my-nginx
   ```
* 컨테이너 중지 확인
  
  ```아오
  podman ps -a
  ```

* STATUS 가 Exited
  
  ```dkdh
  CONTAINER ID  IMAGE                   COMMAND               CREATED        STATUS                         PORTS                 NAMES
  8709140107b4  localhost/nginx:latest  nginx -g daemon o...  8 minutes ago  Exited (0) About a minute ago  0.0.0.0:8000->80/tcp  my-nginx
  ```

---

## 2.podman 컨테이너 접속

1. 컨테이너 실행
   
   ```아오
   podman start my-nginx
   ```

2. my-nginx 접속
   
   ```아오
   podman exec -it my-nginx /bin/bash
   ```
   
   ```아오
   root@8709140107b4:/#
   ```

3. nginx 버전 확인
   
   ```아오
   nginx -V
   ```
   
   ```아옹
   nginx version: nginx/1.25.1
   built by gcc 12.2.0 (Debian 12.2.0-14)
   built with OpenSSL 3.0.9 30 May 2023
   TLS SNI support enabled
   configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-http_v3_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -ffile-prefix-map=/data/builder/debuild/nginx-1.25.1/debian/debuild-base/nginx-1.25.1=. -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'
   ```

4. 컨테이너 로그아웃
   
   ```아오
   exit
   ```

---

## 3. 컨테이너 삭제

1. 컨테이너 중지
   
   ```아오
   podman stop my-nginx
   ```
2. 컨테이너 제거
   
   ```아오
   podman rm my-nginx
   ```
* 제거 확인
  
  ```dkdh
  podman ps -a
  ```
  
  ```아오
  CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES
  ```
3. 컨테이너 이미지 제거
   
   ```아오
   podman rmi localhost/nginx
   ```
   
   ```dkdh
   podman images
   ```
   
   ```dkdh
   REPOSITORY  TAG         IMAGE ID    CREATED     SIZE
   ```

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

> :bulb:**안내**: [image]는 gitlab의 저장된 코드의 URL를 통해서 업로드 가능합니다.

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
    bc/ssgtest source builds http:[gitlab_URL]
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
