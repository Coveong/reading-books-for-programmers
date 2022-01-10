# 03장. 사용자 및 권한

MySQL의 사용자 계정은 단순히 사용자의 아이디뿐 아니라, 해당 사용자가 어느 IP에 접속하고 있는지도 확인한다. 8.0부터는 권한을 묶어서 관리하는 역할(role) 기능도 새로 생겼다.

# 3.1 사용자 식별

계정 + 접속 지점도 계정의 일부가 된다.

```yaml
'svc_id'@'127.0.0.1'
```

이 계정만 등록이 되어있다면, 다른 컴퓨터에서는 svc_id로 접속을 할 수 없다.

→ 모든 IP에서 접속을 허용하고 싶다면, ip 부분을 %로 대체한다.

만약 동일한 아이디가 있다면?

```yaml
'svc_id'@'192.168.0.10' // 비밀번호 123
'svc_id'@'%' // 비밀번호 abc
```

192.168.0.10 환경에서 접속했을 때, MySQL은 범위가 가장 작은 것을 먼저 (비밀번호 123) 선택한다.

# 3.2 사용자 계정 관리

## 3.2.1 시스템 계정과 일반 계정

8.0부터 `SYSTEM_USER` 권한의 유/무에 따라 시스템 계정과 일반 계정이 구분된다.

시스템 계정 → DBA

일반 계정 → 개발자

시스템 계정이 가능한 것

- 계정 관리(계정 생성, 삭제, 권한 부여, 권한 제거)
- 실행 중인 쿼리 강제 종료
- 스토어드 프로그램 생성 시 DEFINER를 타 사용자로 설정

내장된 계정들 (root@localhost를 제외한 계정들은 각기 다른 목적으로 사용됨. 삭제 X)

- ‘mysql.sys’@’localhost’
    - MySQL 8.0부터 기본으로 내장된 sys 스키마의 객체(뷰, 함수, 프로시저)들의 DEFINER로 사용되는 계정
- ‘mysql.session’@’localhost’
    - MySQL 플러그인이 서버로 접근할 때 사용되는 계정
- ‘mysql.infoschema’@’localhost’
    - information_schema에 정의된 뷰의 DEFINER로 사용되는 계정
    

## 3.2.2 계정 생성

계정의 생성은 `CREATE USER`

권한 부여는 `GRANT` 로 가능하다.

계정 생성 시에는 다양한 옵션을 설정할 수 있다.

- 계정의 인증 방식과 비밀번호
- 비밀번호 관련 옵션
    - 유효 기간
    - 이력 개수
    - 재사용 불가 기간
    - 기본 역할(Role)
    - SSL 옵션
    - 계정 잠금 여부

```sql
CREATE USER 'user'@'%'
	IDENTIFIED WITH 'mysql_native_password' BY 'password'
	REQUIRE NONE
	PASSWORD EXPIRE INTERVAL 30 DAY
	ACCOUNT UNLOCK
	PASSWORD HISTORY DEFAULT
	PASSWORD REUSER INTERVAL DEFAULT
	PASSWORD REQUIRE CURRENT DEFAULT;
```

### 3.2.2.1 IDENTIFIED WITH

사용자의 인증 방식과 비밀번호 설정

- Native Pluggable Authentication
    - 비밀번호의 해시값을 저장하고, 클라이언트가 보낸 값 == 해시 값인지 비교
    - 5.7 버전까지의 기본 인증 방식
- **Caching SHA-2 Pluggable Authentication**
    - 비밀번호 해시값 생성을 위해 SHA-2 알고리즘 사용
    - 알고리즘 자체가 오래 걸리기 때문에 시간 소모가 많이들기 때문에 캐싱을 추가함
    - SSL/TLS 또는 RSA 키페어를 반드시 사용해야 함
    - 8.0 버전부터의 기본 인증 방식
- PAM Pluggable Authentication
    - 유닉스, 리눅스 패스워드, LDAP 같은 외부 인증을 사용할 수 있게 하는 인증 방식
- LDAP Pluggable Authentication
    - LDAP 외부 인증을 사용할 수 있게 하는 인증 방식
    

### 3.2.2.2 REQUIRE

SSL/TLS 채널 사용 여부

설정하지 않으면 비암호화 채널로 연결한다.

### 3.2.2.3 PASSWORD EXPIRE

비밀번호 유효 기간 설정

설정하지 않으면 default_password_lifetime 시스템 변수에 저장된 기간으로 설정

`PASSOWRD EXPIRE`에 설정 가능한 옵션

- PASSWORD EXPIRE
    - 계정 생성과 동시에 비밀번호의 만료 처리
- PASSWORD EXPIRE NEVER
    - 계정 비밀번호의 만료 기간 없음
- PASSWORD EXPIRE DEFAULT
    - default_password_lifetime에 저장된 기간으로 유효 기간 설정
- PASSWORD EXPIRE INTERVAL n
    - 비밀번호의 유효 기간을 오늘부터 n일자로 설정

### 3.2.2.4 PASSWORD HISTORY

한 번 사용했던 비밀번호 재사용 불가능

`PASSWORD HISTORY`에 설정 가능한 옵션

- PASSWORD HISTORY DEFAULT
    - password_history 시스템 변수에 저장된 개수만큼 재사용 불가능
- PASSWORD HISTORY n
    - n개까지만 저장, 저장 이력에 남아있는 비밀번호 재사용 불가능

예전에 사용한 비밀번호를 저장해두는 테이블은 pasword_history이다.

### 3.2.2.5 PASSWORD REUSE INTERVAL

한 번 사용했던 비밀번호 재사용 금지 기간

설정하지 않으면 `password_reuse_interval` 시스템 변수에 저장된 기간으로 설정

`PASSWORD REUSE INTERVAL`에 설정 가능한 옵션

- PASSWORD REUSE INTERVAL DEFAULT
    - password_reuse_interval에 저장된 기간 설정
- PASSWORD REUSE INTERVAL n DAY
    - n일자 이후에 비밀번호 재사용 가능

### 3.2.2.6 PASSWORD REQUIRE

비밀번호가 만료되어 새로운 비밀번호로 변경할 때 현재 비밀번호를 필요로 할지 말지 설정

설정하지 않으면 `password_require_current` 시스템 변수에 저장된 값으로 설정

`PASSWORD REQUIRE`에 설정 가능한 옵션

- PASSWORD REQUIRE CURRENT
    - 필요
- PASSWORD REQUIRE OPTIONAL
    - 불필요
- PASSWORD REQUIRE DEFAULT
    - password_require_current 시스템 변수에 저장된 값으로 설정

### 3.2.2.7 ACCOUNT LOCK / UNLOCK

계정 생성 하거나 정보를 변경할 때 계정을 사용하지 못하게 잠글지 여부

- ACCOUNT LOCK
    - 잠금
- ACCOUNT UNLOCK
    - 잠긴 계정을 다시 사용 가능 상태로 잠금 해제