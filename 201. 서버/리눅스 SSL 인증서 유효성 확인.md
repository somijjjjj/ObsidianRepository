
```shell
openssl s_client -connect inssider.kro.kr:443
```

```
CONNECTED(00000003)
depth=1 C = US, O = (STAGING) Let's Encrypt, CN = (STAGING) Counterfeit Cashew R10
verify error:num=20:unable to get local issuer certificate
verify return:1
depth=0 CN = inssider.kro.kr
verify return:1
---
Certificate chain
 0 s:CN = inssider.kro.kr
   i:C = US, O = (STAGING) Let's Encrypt, CN = (STAGING) Counterfeit Cashew R10
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jan 21 12:34:44 2025 GMT; NotAfter: Apr 21 12:34:43 2025 GMT
 1 s:C = US, O = (STAGING) Let's Encrypt, CN = (STAGING) Counterfeit Cashew R10
   i:C = US, O = (STAGING) Internet Security Research Group, CN = (STAGING) Pretend Pear X1
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Mar 13 00:00:00 2024 GMT; NotAfter: Mar 12 23:59:59 2027 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIFJDCCBAygAwIBAgISK4YT4T9EC/1lrtq2dvNWlQmlMA0GCSqGSIb3DQEBCwUA
MFoxCzAJBgNVBAYTAlVTMSAwHgYDVQQKExcoU1RBR0lORykgTGV0J3MgRW5jcnlw
dDEpMCcGA1UEAxMgKFNUQUdJTkcpIENvdW50ZXJmZWl0IENhc2hldyBSMTAwHhcN
MjUwMTIxMTIzNDQ0WhcNMjUwNDIxMTIzNDQzWjAaMRgwFgYDVQQDEw9pbnNzaWRl
ci5rcm8ua3IwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCadArlOjvB
+rNBKmPAc87Km6T/4BmeuC9ne9Xb0NV87qgcaP5Rqk2XHWsuDUcyqbT52oe9wz9H
aQ8HhMxsHp7FIY0orpmvDcxoni4PQbKRa+vbqzGU1pWFgX6LzEv7KtVYusVGGGC+
BCACyJgBWDepYh0B2IlTGoexPmyMO3cRIB2JG3YU0e7UU1lY2fh97w3kRVgc2otg
Vukm8a3Y1OXuNoYQSy/INztPMTw6G7cctHqzoGnC7srPYg6NuEbwDgUDYh8uYhxR
SkI9zNgCXkYbF7cWO8AcXRT5mYXNt3dnGju2jt4M7lFx9jq67+BmBE6aiTab7duH
wbJSRq78IFQDAgMBAAGjggIiMIICHjAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYw
FAYIKwYBBQUHAwEGCCsGAQUFBwMCMAwGA1UdEwEB/wQCMAAwHQYDVR0OBBYEFNOF
TNGWUaBDZRZLSv7DOaGAJ5mEMB8GA1UdIwQYMBaAFKRSRupYqI9o2LexkNFKQkqP
ayhxMF8GCCsGAQUFBwEBBFMwUTAmBggrBgEFBQcwAYYaaHR0cDovL3N0Zy1yMTAu
by5sZW5jci5vcmcwJwYIKwYBBQUHMAKGG2h0dHA6Ly9zdGctcjEwLmkubGVuY3Iu
b3JnLzAaBgNVHREEEzARgg9pbnNzaWRlci5rcm8ua3IwEwYDVR0gBAwwCjAIBgZn
gQwBAgEwggELBgorBgEEAdZ5AgQCBIH8BIH5APcAdQCwzIPlpfl9a698CcwoSQSH
KsfoixMsY1C3xv0m4WxsdwAAAZSJETmpAAAEAwBGMEQCIBUhro23vBOVw1kEkxRK
KyayTvH11TElDQdDQVGzl1EQAiBDLV9tl3a0APHo5tGOu6qfMFFu5cXKYJdSHr1O
0HlqDwB+AAnKhPUK+EtCP5PJ/2sO2W+dLxSyvDelUw5JSGJymiK1AAABlIkRPJ4A
CAAABQBIdJfxBAMARzBFAiAKja9keVDN8QzM25GfpQWt/UScx6riTGrdS6MqRIMm
0AIhAPrTI5QQez8SDrrin+uB5QNwRZALE90GxNMe1xM4ofcjMA0GCSqGSIb3DQEB
CwUAA4IBAQB33ZCzkrUZcsonMUwJUXLHEP0/Y4c8iUWhspRIEP1UpQNFWHVEzN8O
x1YYqs6BX7A2CLX6JPFKoMdHOey8nYyLXYzbn7KgLg6yhWYSfVoIP4F1hQJlOucp
R0MI/OOEajBi3t7C52WrLJLJHHvKTTRVwi7ijW+Aqt8ZvGwu620cpznDb4seLhl8
AcYP1UIew6VdCe/Ojd9Sv2u5aAJ99kK4CBg8qRj9t1O9iOUIJscfRD+L6C1mHyyk
TszA4sZCAeHoStcoboQFejvtpxVTDcINJEUGXmjDMZr+Ut7jEq3l7867xozd7lTC
Rj80+08hOhKU2PIc+Ag3K/SIlCA3mD8c
-----END CERTIFICATE-----
subject=CN = inssider.kro.kr
issuer=C = US, O = (STAGING) Let's Encrypt, CN = (STAGING) Counterfeit Cashew R10
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 3246 bytes and written 397 bytes
Verification error: unable to get local issuer certificate
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 20 (unable to get local issuer certificate)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: BD0F9D58AB71F14B23592B4DAA235F14C9569DE9749C335D0715900F132B10FD
    Session-ID-ctx:
    Resumption PSK: 361E34FC700842F84CC50C5F93DE4EA13B58927761FEDCD7EC94C2D4B37ACF7E03B7C9C62BB30E2D5D6EBA87D74FA0D5
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 86400 (seconds)
    TLS session ticket:
    0000 - 9d 22 c5 d0 ac 37 2a 22-3d cb c6 74 fc df 4f ff   ."...7*"=..t..O.
    0010 - 40 9f 68 67 cf ca aa 4b-ab ff a5 98 9d ab cc ff   @.hg...K........

    Start Time: 1737549369
    Timeout   : 7200 (sec)
    Verify return code: 20 (unable to get local issuer certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 2E901B2B7744BCADAC81E7844A1951B8C0B43E1A99C5D9A71D16574D3C7FA607
    Session-ID-ctx:
    Resumption PSK: ED5B523F2EBC381614C2F9B2282C275A5265478DBE6660797D57130035867D20636B4CD965CB336E7F49E08662C357DF
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 86400 (seconds)
    TLS session ticket:
    0000 - 9f 34 e9 06 e0 46 c7 27-61 cd 3e d1 c6 6d 8e 9a   .4...F.'a.>..m..
    0010 - 30 ec 99 a7 a8 9b 5e 89-28 22 c3 49 47 17 c4 0d   0.....^.(".IG...

    Start Time: 1737549369
    Timeout   : 7200 (sec)
    Verify return code: 20 (unable to get local issuer certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK


```


로그를 보면 현재 SSL 인증서가 **Let’s Encrypt STAGING 환경**에서 발급된 것으로 보입니다. 이 환경은 테스트 용도로 사용되며, 브라우저에서 유효한 SSL 인증서로 인식되지 않습니다. 따라서 "ERR_CERT_AUTHORITY_INVALID" 오류가 발생하는 것입니다.

### 주요 원인

1. **Staging 환경 인증서 사용**: Let’s Encrypt의 Staging 환경은 테스트용이며, 신뢰할 수 있는 인증 기관(CA)으로 간주되지 않습니다.
2. **SSL Chain 문제**: "unable to get local issuer certificate" 메시지는 인증서 체인에 문제가 있음을 나타냅니다. 클라이언트가 루트 인증서까지 인증 경로를 찾을 수 없습니다.