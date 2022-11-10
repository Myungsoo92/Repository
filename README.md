## Window
- Windows 정품 인증 워터 마크 없애기(cmd)
slmgr /rearm

- 문제가 있는 서비스 확인(cmd)
tasklist /FI "SERVICES eq 서비스이름"

- 서비스 강제 종료(cmd)
taskkill /F /FI "SERVICES eq 서비스이름"



## Oracle DBMS
- ORACLE DB export / import
```
exp swweb/swweb@203.236.231.35:1521 file='D:\dump\swweb_DB_220610.dmp' log='D:\dump\swweb_DB_220610.log' OWNER='swweb' full=y
exp userid=system/root@203.236.231.35:1521 file='D:\dump\full.dmp' full=y log=D:\dump\full_log01.log
imp userid=mgadmin/mgadmin file='D:\dump\swweb_DB_220610.dmp' fromuser='swweb' touser='swweb' ignore=y
```

- ORACLE DB 권한부여
```
GRANT CREATE SESSION TO username;
GRANT CONNECT, DBA, RESOURCE TO username; 
```

- ORACLE DB 캐릭터셋 확인
```
select distinct(nls_charset_name(charsetid)) CHARACTERSET,
           decode(type#, 1, decode(charsetform, 1, 'VARCHAR2', 2, 'NVARCHAR2','UNKOWN'),
                         9, decode(charsetform, 1, 'VARCHAR', 2, 'NCHAR VARYING', 'UNKOWN'),
                        96, decode(charsetform, 1, 'CHAR', 2, 'NCHAR', 'UNKOWN'),
                       112, decode(charsetform, 1, 'CLOB', 2, 'NCLOB', 'UNKOWN')) TYPES_USED_IN
       from sys.col$ where charsetform in (1,2) and type# in (1, 9, 96, 112);
```
	   
	   
- ORACLE DB 전체 테이블 삭제
```SELECT 'DROP TABLE "' || TABLE_NAME || '" CASCADE CONSTRAINTS;' FROM user_tables;```

- ORACLE DB 휴지통 비우기
```purge recyclebin;```

- ORACLE 테이블 목록 조회
```
SELECT * FROM all_all_tables;
SELECT * FROM dba_tables;
SELECT * FROM ALL_OBJECTS WHERE OBJECT_TYPE = 'TABLE';
```

- ORACLE 테이블 목록 조회 (접속한 계정)
```
SELECT * FROM tabs;
SELECT * FROM USER_OBJECTS WHERE OBJECT_TYPE = 'TABLE';
SELECT * FROM USER_TABLES;
```

- ORACLE 테이블 코멘트 조회
```
SELECT * FROM ALL_TAB_COMMENTS WHERE TABLE_NAME = '테이블명';
SELECT * FROM USER_TAB_COMMENTS;
```

- ORACLE 컬럼 조회
```
SELECT * FROM COLS WHERE TABLE_NAME = '테이블명';
SELECT * FROM ALL_TAB_COLUMNS WHERE TABLE_NAME = '테이블명';
SELECT * FROM USER_TAB_COLUMNS;
```

- ORACLE 컬럼 코멘트 조회
```SELECT * FROM USER_COL_COMMENTS```

-- ORACLE 특정 칼럼을 갖는 테이블 조회
```SELECT TABLE_NAME, COLUMN_NAME FROM ALL_TAB_COLUMNS WHERE COLUMN_NAME LIKE '칼럼명';```

- ORACLE 길이 비교로 한글 찾거나 제외하기
```
LENGTH(A.G2_ALIAS)=LENGTHB(A.G2_ALIAS) -- 한글포함된것 제외
SELECT * FROM TABLE_NAME WHERE NOT REGEXP_LIKE(COLUMN_NAME, '[가-힣]'); -- 한글 들어간것 제외
```

## Mysql
- mysql 캐릭터셋 확인
```SELECT SCHEMA_NAME, default_character_set_name, DEFAULT_COLLATION_NAME FROM information_schema.SCHEMATA;```

- mysql cmd 접속
```mysql -h서버주소 -u아이디 -p패스워드 ```

- mysql 연결할 수 있는 최대 클라이언트 갯수 확인
```show variables like '%max_connect%';```

- mysql 지속시간 확인
```show variables like 'wait_timeout';```

- mysql 현재 상태 확인
```show status like '%CONNECT%';```

- mysql dump export
```mysqldump dbname -uuser -ppassword -hhost > dir\filename.sql```

- mysql dump import
```mysql -u user -p password (-database=dbname) < dir\filename.sql```

- mysql now()
```DATE_FORMAT(NOW(), '%Y%m%d%H%i%s')```



## Javascript
- JSP 엔터 이벤트 처리
```onKeypress="javascript:if(event.keyCode==13) {}"```
- XSS처리<br>
[1.XSS처리 블로그](https://myhappyman.tistory.com/253)<br>
[2.LucyXSS Git](https://github.com/naver/lucy-xss-servlet-filter)

- ajax 뒤로가기 기능
```
$(window).on("popstate", function (event) {
	var data = event.originalEvent.state;
	if(data != null){
		if(data.url.includes("/gms/road/admin/adminMain.do") || data.url.includes("/gms/road/mainPage.do")) {
			$.post(data.url,data.data,function(res){
				document.write(res);
			});
		} else {
			$.post(data.url,data.data,function(res){
				$("#Contents").html(res);
			});
		}
	}
});
```

- ajax 새로고침 기능
```
function doReload(){
	var data = history.state;
	if(data != null) {
		if(data.url != "/gms/road/mainPage.do" && data.url != "/gms/road/admin/adminMain.do"){
			$.post(data.url, data.data, function(res){
				var contents = $("#Contents");
				if(contents == res) {
					return false;
				}
				$('#Contents').html(res);
			});
		}
	}
}
```

- javascript 자리 채우기
```str.padStart(자리수, 채울문자), str.padEnd(자리수, 채울문자)```


- [Javascript] 브라우저별 unload 이벤트 할당
```
if (window.attachEvent) {
    /*IE and Opera*/
    window.attachEvent("onunload", function() {
    });
} else if (document.addEventListener) {
    /*Chrome, FireFox*/
    window.onbeforeunload = function() {
    };
    /*IE 6, Mobile Safari, Chrome Mobile*/
    window.addEventListener("unload", function() {
    }, false);
} else {
    /*etc*/
    document.addEventListener("unload", function() {
    }, false);
}
```

- window.open 시 열린 창에서 부모찾기
```
opener.parent
ex) opener.parent.location.reload();
```

- 팝업 위치 가운데로
```$("#popup").css('top', Math.max(0, ($(window).height() / 2) + $(window).scrollTop()) + 'px');```

---

## Tomcat
- tomcat context 추가 (server.xml)
```<Context path="/path" docBase="docBase" disableURLRewriting="true" reloadable="false/true"/>```

- tomcat jsessionid 노출 안되도록 설정 (web.xml)
```
<session-config>
	<session-timeout>30</session-timeout>
	<tracking-mode>COOKIE</tracking-mode>
</session-config>
```

- tomcat multi-part설정 (context.xml)
```
<Context allowCasualMultipartParsing="true" path="/">
<Resources cachingAllowed="true" cacheMaxSize="100000" />
```

- tomcat console log 출력 저장 (context.xml)
```
<WatchedResource>${catalina.base}/conf/web.xml</WatchedResource> 아래에
<Context swallowOutput="true"> 추가
```

- tomcat jdk 추가 (catalina.sh)
```set JAVA_HOME=D:\gwanakTest\java-1.8.0-openjdk-1.8.0.332-1.b09.ojdkbuild.windows.x86_64```
- tomcat JRE 추가 (castalina.bat)
```JRE_HOME=D:\gwanakTest\java-1.8.0-openjdk-1.8.0.332-1.b09.ojdkbuild.windows.x86_64```
- tomcat 실행 옵션에 UTF-8 추가 (catalina.bat)
```set "CATALINA_OPTS=-Dfile.encoding=UTF-8"```

- tomcat service 등록 시 jdk 설정 (service.bat)
```set JAVA_HOME=D:\WASTest\java-1.8.0-openjdk-1.8.0.332-1.b09.ojdkbuild.windows.x86_64```
- tomcat startup.bat jdk 설정
```set JAVA_HOME=D:\gwanakTest\java-1.8.0-openjdk-1.8.0.332-1.b09.ojdkbuild.windows.x86_64```



## PowerPoint
- 특정 코드 찾기
```Option Explicit
Dim myShp As Shape
Dim slide_num As Long
Dim myFont As Variant
Dim msg As Long
 
Sub catch_font()
myFont = InputBox("찾을 글꼴을 입력하세요(와일드카드 사용 가능, ex. 모든 고딕체를 찾는다면 *고딕* 입력)", "글꼴 찾기")
On Error Resume Next
Do While Err.Number = 0
slide_num = slide_num + 1
    For Each myShp In ActivePresentation.Slides(slide_num).Shapes
        If myShp.HasTextFrame = True Then
            If myShp.TextEffect.FontName Like myFont Then
                ActivePresentation.Slides(slide_num).Select
                msg = MsgBox(myShp.TextEffect.FontName & _
                          " 글꼴은" & " " & "현재 " & slide_num & "쪽에 있습니다." & Chr(10) & "검색을 계속 할까요?", vbYesNo)
                    If msg = 7 Then
                    Exit Do
                    End If
            End If
        End If
    Next
Loop
slide_num = 0
msg = 0
Set myShp = Nothing
 
MsgBox "검색을 종료합니다."
On Error GoTo 0
End Sub```
