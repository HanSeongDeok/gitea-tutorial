## openssl cnf 파일 50x 인증서 SSL/TLS 문제 해결을 위한 파일 세팅

[ v3_ca ]
subjectAltName = @alt_names

[ alt_names ]
IP.52 = 10.10.110.52


## openssl CSR 인증서 파일 생성 및 서명 명령어
- CSR 생성: openssl req -new -newkey rsa:2048 -days 365 -nodes -keyout gitea.key -out gitea.csr 
- 서명 명령어(SELF) : openssl x509 -req -in gitea.csr -signkey gitea.key -out gitea.crt -days 365 -extfile openssl.cnf -extensions v3_ca
- 서명 명령어(CA) : openssl x509 -req -in gitea.csr -CA /data/certs/cert.pem -CAkey /data/certs/key.pem -CAcreateserial -out gitea.crt -days 365 -extfile openssl.cnf -extensions v3_ca
- 확인 명령어: openssl x509 -in /etc/ssl/gitea.crt -text -noout

## 도커 서버에서 인증서 복사
- docker cp gitea_container:/data/certs/gitea.crt /tmp/gitea.crt
- docker cp /tmp/gitea.crt gitea_container:/data/certs/gitea.crt

## 도커 Terminal 에서 /bin/bash 로 sudo 사용 가능 명령어
- docker exec -it --user root <container_name> /bin/bash

## Linux 서버 내 인증서 신뢰 설정
cp /path/to/gitea.crt /usr/local/share/ca-certificates/gitea.crt
update-ca-certificates

## Window 인증서 자체 인증 하는 방법
- 만들어진 .crt 파일을 준비한다.

## 도커 내에 Docker Daemon이 외부에서 오는 Docker API 요청을 수락하도록 하는 json 작성
```
{
  "hosts": [
    "tcp://0.0.0.0:2375",          # 외부 연결을 위한 TCP 포트 (모든 IP에서 연결 허용)
    "unix:///var/run/docker.sock"  # 로컬 소켓 연결 (Linux에서 docker의 소켓 통신을 위한 파일)
  ]
}
```
