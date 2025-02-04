## openssl cnf 파일 50x 인증서 SSL/TLS 문제 해결을 위한 파일 세팅

[ v3_ca ]
subjectAltName = @alt_names

[ alt_names ]
IP.52 = 10.10.110.52


## openssl CSR 인증서 파일 생성 및 서명 명령어
- CSR 생성: openssl req -new -newkey rsa:2048 -days 365 -nodes -keyout gitea.key -out gitea.csr 
- CSR 생성: openssl x509 -req -in gitea.csr -signkey gitea.key -out gitea.crt -days 365 -extfile openssl.cnf -extensions v3_ca
- 서명 명령어: openssl x509 -req -in gitea.csr -CA /data/certs/cert.pem -CAkey /data/certs/key.pem -CAcreateserial -out gitea.crt -days 365 -extfile openssl.cnf -extensions v3_ca
- 확인 명령어: openssl x509 -in /etc/ssl/gitea.crt -text -noout
