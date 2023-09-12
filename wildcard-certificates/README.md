# acme.sh로 발급한 와일드 카드 인증서 CloudPanel사이트에 연결하기

## incrontab 설치하기

`apt install incron`을 통해서 incorn을 설치합니다. inode 변경 사항을 수신하고 다양한 inode 이벤트에서 명령을 실행할 수 있습니다.

## 스크립트 파일 복사하기

`clp-install-certificate` 파일을 `/usr/local/bin/`의 경로로 복사합니다. acme.sh에서 경로를 수정하실 수 있습니다.

```
chmod +x /usr/local/bin/clp-install-certificate
```

위의 명령을 통해서 실행권한을 부여합니다.

## acme.sh 설치하기

[acme.sh](https://github.com/acmesh-official/acme.sh) 를 설치 및 설정하여 인증서를 받습니다. '/usr/local/bin/clp-install-certificate && service nginx force-reload` 명령을 사용하여 새 인증서를 연결하고 새 인증서를 발급한 후 nginx를 다시 로드합니다.

## incron 설정하기

```
echo "root" | sudo tee -a /etc/incron.allow
```

명령으로 root 사용자에 incron사용 권한을 추가합니다.

그리고 `incrontab -e` 을 통해서 다음의 내용을 추가합니다.

```
/etc/nginx/sites-enabled/       IN_CREATE                       /usr/local/bin/clp-install-certificate $#
```

이제, CloudPanel에서 새 사이트가 만들어질 때마다 스크립트가 트리거됩니다. clp-install-certificate는 구성의 파일 이름을 가져오고 파일 이름에서 도메인을 추출하여 해당 도메인에 대한 acme.sh 와일드카드 인증서를 설치합니다.

따라서, 새 사이트를 만드시기 전에 인증서를 우선 발급받으시기 바랍니다.

추가적으로, [Installing a Certificate](https://www.cloudpanel.io/docs/v2/cloudpanel-cli/root-user-commands/#installing-a-certificate)를 참고하세요.
