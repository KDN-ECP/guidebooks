# 4단계: Container Terminal 정리

1. `oc`명령어를 통해 CT에서 OpenShift계정 로그아웃을 할 수 있습니다.
   
   ```주절이
   oc logout
   ```
   
   ```주절이
   Logged "[ID]" out on "https:api.ocp4.kdnecp.com:6443"
   ```

2. CT자원을 완전히 삭제하길 원하는 경우 `서비스 변경 및 삭제 신청 > 삭제신청`로 이동
- 프로젝트: 해당`CT`가 있는 프로젝트 선택

- 자원: 가상서버, `CT`의 서버 선택

- `x삭제신청` 버튼 클릭

> :warning:**주의사항**:CT 서비스를 삭제할 경우 OpenShift계정 또한 삭제됩니다.


