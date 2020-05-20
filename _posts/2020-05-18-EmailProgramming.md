---
layout: post
title:  "Email Programming"
date:   2020-05-18
excerpt: ""
tag:
- Email
- practice
comments: true
---

## 인터넷 전자 메일
메시지는 상대방에게 보내고자 하는 정보, 사진 및 기타 파일 첨부 가능(MIME)

### 핵심요소
* MUA(Mail User Agent)
  사용자가 전자 메일을 송수신할 때 사용하는 클라이언트 프로그램
* MTA(Mail Transfer Agent)
  인터넷 네트워크를 거쳐 컴퓨터->컴퓨터(메일 서버)로 전자 메일을 전송하는 서버 프로그램(gmail, naver mail ...)
* MDA(Mail Delivery Agent)
  메시지를 사용자에 우편함에 전달하기위해 MTA가 사용하는 프로그램
* MRA(Mail Retrieval Agent)
  원격 서버에 있는 우편함에서 사용자의 MUA로 메시지를 가져오는 서비스(POP,IMAP 프로토콜)

***

### 흐름
![flow](/photo/mailsystem/mailflow.jpg)  

## 단순 텍스트 메시지
* 전자메일은 RFC 822에서 기술한 단순 텍스트 메시지 포맷 기반
* 텍스트 메시지는 헤더필드와 본문(+봉투)으로 구성
* 헤더 필드는 key-value 쌍으로 구성, 필수헤더는 메시지의 송신자, 수신자 정보, 날짜정보 제공
* 본문의 행은 RFC822는 글자제한 X, RFC821(SMTP)는 7비트 문자로 작성, 1000문자 미만으로 제한(관행적으로 MTA,MUA는 긴행을 잘 처리하지 못해 한 행을 80문자 미만 작성)

### 헤더
메시지 라우팅에 관한 정보를 가지고 있음  
MUA는 긴 헤더 값을 긴 행(256문자 초과X)으로 보내거나 나눠서(행 당 72문자 초과X) 보낼수 있음

#### 헤더의 순서
* Return-Path : 메일 반송시 주소
* Received : 수신자 정보(서버)
* Date : 날짜
* From : 보낸이(보낸 메일 서버 or 발신자)
* Subject : 제목
* Sender : 발신자
* To : 수신자
* CC : 참조
* Others(Reply-To 등)

#### 필수 헤더
* Date
* From
* To or BCC

#### 옵션 헤더
메시지 배포 횟수, 메일 시스템 부가 정보 제공하기위해 사용
* CC : 참조
* BCC : 숨겨진 참조
* Subject : 제목

#### 동적 헤더
MTA에서 Date 같은 필수 헤더를 만들어 주지만, 사용자가 정의 할수 있는 헤더
* Message-ID : 메시지를 식별하기위한 고유한 메시지 ID 
* Resent-Message-ID : 재발송된 메시지 식별 ID 
* Received : 추적 필드, 수신자 정보
* Return-Path : 추적 필드, 회신자 정보
* Apparently-To : To 헤더가 제공되지 않을때 MTA에서 작성
* Apparently-From : FROM 헤더가 제공되지 않을 때 MTA에서 작성

![mailheader](/photo/mailsystem/mailheader.PNG) 

#### 사용자 정의 헤더
사용자 정의에따라 목적된 메일이 지정된 메일서버에 전송하기위해 만드는 헤더  
다른 헤더와 구분하기위해 X-로 시작
  
***  

## MIME(Multipurpose Internet Mail Extensions)
텍스트 메시지를 확장하여 Binary file 포함 가능  
바이너리 파일을 번역, 7비트 ASCII 텍스트로 인코딩

### MIME 헤더 필드
MIME 메시지 헤더(MIME 호환 메시지, 메시지 구조와 인코딩 정보 제공) + MIME 부분 헤더(메시지 본문, 각 부분의 컨테츠 정보 제공, Content로 시작)  



#### MIME 메시지 헤더
* MIME-Version : MIME 버전 정보
* Content-Type : 메시지 데이터 유형 식별
  ** text  
  ** image  
  ** audio  
  ** video  
  ** application - 특수 데이터 (인쇄) 파일유형을 인식하지 못하는경우 application/octet-steream  
  ** multipart - 다중 첨부파일  
  ** message - 다중메시지  
* Content-Transfer-Encoding : MIME 정보
  ** 7bit  
  ** 8bit  
  ** binary  
  ** quoted-printable  
  ** base64  
* Content-ID : 식별 ID, message/external-body 타입이 사용되면 필수
* Content-Description : 텍스트 설명
* Contnet-Disposition : MUA 디스플레이 힌트

![mimeheader](/photo/mailsystem/mimeheader.PNG)


### MIME 인코딩
* text : 7bit
* 특수문자 : 필요시 quoted-printable
* binary data : base64

7bit : 0~127 ASCII 십진값
(SMTP를 통한 인터넷 사용은 아래의 인코딩 유형 사용 X)
8bit : 127 이상의 ASCII 십진값 허용
 
※ SMTP MTA 작성 시 8bit MIME 첨부파일을 quoted-printable이나 base64로 변환해야함

#### Quoted-Printable
보통의 텍스트, 사람이읽을수 있음, 7bit가 아닌 데이터를 위해 사용  
8bit나 텍스트 데이터를 SMTP로 전송하기위해 7bit로 변환
ASCII문자가 아닌 문자를 "=XX" 와 같은 모양으로 인코딩 7bit ASCII로 표현할수 없는 문자는 한 바이트가 3바이트로 늘어남 대부분 ASCII문자인 영어권에서는 적합, ASCII문자가 없는 한글이나 바이너리 파일에 경우 이점이 없어 base64로 인코딩하는 것이 좋음
  
예)"가나 다" =B0=A1=B3=AA =B4=D9

#### base64
24비트(3바이트)를 입력받아 6비트씩 잘라 4바이트를 출력하는 인코딩방식
각 6비트를 특정 값으로 매핑해 변환
데이터 크기가 33% 증가함  
  
예) "가나다" (2바이트 X 3자 = 6바이트) => sKGzqrTZ (1바이트 X 8자 = 8바이트)

### MIME 경계
![mime](/photo/mailsystem/mime.PNG)


## MIME 호환 메시지 작성하기

### 최소의 MIME 메시지 헤더 요구사항
* MIME-Version 
* Content-Type : US-ASCII 문자집합이 아닐경우
* Content-Transfer-Encoding  : US-ASCII 문자집합이 아닌 경우

### 다중 부분 메시지
![mimemultiple](/photo/mailsystem/mimemultiple.PNG)

* 다중 메시지 헤더는 multipart/mixed, 인코딩은 7비트,8비트, 바이너리만 가능 / 보통 7비트 사용
* MIME 경계 표시기(boundary)가 파라미터로 주어져야함
* 컨텐츠 전송 인코딩은 디폴트로 7비트

***

## ESMTP
TCP/IP 기반의 발전된 SMTP(Simple Mail Transfer Protocol), PORT 25  
SMTP는 보안과 호환성의 문제로 그대로는 사용하지 않음. SMTP-AUTH(송신자 인증 서비스), ESMTP(SASL을 이용한 보안 연결) 등으로 사용  
STMP는 연결지향적, 텍스트 기반으로 작동하는 프로토콜  
교환 명령
MAIL : 수신자 지정, RCPT : 송신자 지정, DATA : 메시지 내용의 시작

![smtpflow](/photo/mailsystem/smtpflow.PNG) {:.aligncenter}


### SMTP 명령어
* HELO [도메인 네임] : 클라이언트를 식별하기 위해 사용
* MAIL FROM [메일 주소] : 보낸 사람의 전자 메일 주소 지정
* RCPT TO [메일 주소] : 받는 사람의 전자 메일 주소 지정
* DATA : 메시지 내용의 전송을 시작
* RSET : 현재 메일 트랜잭션 중단
* VRFY : 서버에 저장된 사용자 이름이나 사서함이 유효한지 확인 요청
* NOOP : 수신자 연결 확인 (오퍼레이션 x)
* QUIT : 연결을 닫기 위해 요청

### ESMP 명령어
* EHLO : HELO와 동일, 클라이언트가 확장 SMTP(ESMTP) 프로토콜을 대신 사용할 수도 있음을 알림
* AUTH : 서버에 대한 클라이언트를 인증하는데 사용
* STARTTLS : 보안을 강화하기위해 통신 암호화 TLS 연결
* SIZE : 클라이언트에 최대 메시지 크기를 알림, 서버에 메일 메시지의 예상된 크기를 알림
* HELP : 서버가 클라이언트에 도움 정보를 보냄
* EXPN : 주소가 메일링 리스트면 전자 메일 주소를 확장할수있는지 서버에 물음
* TURN : 클라이언트와 서버가 역할을 바꾸도록 요청
* VERB : 서버가 상세모드를 사용하도록 요청
* ETRN : 특정한 호스트, 도메인의 메일 큐를 처리

***

## POP
이메일 서버에 도착한 메일을 클라이언트에 가져와 확인할때 사용  
사서함으로부터 클라이언트로 메일을 직접 다운로드, (다운로드 시 서버 메일 삭제)  
인증상태 -> 트랜잭션 상태 -> 갱신 상태

### 인증상태
* 사용자명/비밀번호
* MD5기반의 APOP 명령어

#### 사용자명/비밀번호 인증
USER [사용자명]
PASS [비밀번호]

#### APOP 인증
사용자명 + 서버 타임스탬프 + 비밀번호 MD5 digest 방식의 인증방식

#### AUTH 인증
인증을 검증하기위해 서버와 클라이언트가 협상

### 트랜잭션 상태
* STAT : 우편함의 메시지 수, 옥텟 크기 반환
* LIST : 요청한 메시지의 번호의 크기 반환
* DETR : 서버의 메일을 검색하는데 사용
* DELE : 메시지 삭제 요청
* NOOP : 연결상태 시간 초기화
* REST : 삭제 표시 되었던 메시지 삭제표시 제거
* TOP : 전체 메시지를 다운로드 하기 전 일부 표시
* UIDL : 메시지의 고유한 ID 요청
* QUIT : 종료 (삭제표시된 메시지 삭제 등)

### 갱신상태
QUIT 명령어 입력시 삭제표시된 메시지를 삭제하는 상태


## IMAP
POP와 달리 서버에 메시지 유지  
인증되지 않은 상태 -> 인증상태 -> 선택상태 -> 로그아웃 상태

### 모든 상태
* CAPABILITY : 서버에 프로토콜과 인증 목록 요청
* NOOP
* LOGOUT : 접속 종료

### 인증되지 않은 상태
* AUTHENTICATE : 서버 프로토콜에 따라 인증
* LOGIN : 사용자명, 비밀번호로 인증

### 인증 상태
* SELECT : 우편함을 선택하고 메시지에 접근
* EXAMINE : 읽기전용으로 선택
* CREATE : 새로운 우편함 생성
* DELETE : 지정한 우편함 삭제
* RENMAE : 이름 변경
* SUBSCRIBE : 지정한 우편함을 "가입된" 우편함 목록에 추가(LSUB 명령어로 가져올수 있음)
* UNSUBSCRIBE : 지정한 우편함을 "가입된" 우편함 목록에서 제거
* LIST : 해당되는 값으로 가져올수있는 우편목록을 가져옴
* LSUB : "가입된" 우편함 목록의 부분집합을 가져옴
* STATUS : 지정한 우편함 상태 요청
* APPEND : 메시지를 우편함의 끝에 덧붙임

### 선택 상태
SELECT, EXAMINE 명령어로 우편함선택시 선택상태로 진입  

* STORE : 메시지 데이터 항목을 주어진 값으로 변경
* EXPUNGE 
* SEARCH : 현재 선택된 우편함에서 메시지 검색
* FETCH : 메시지관련 요청데이터 검색
* COPY : 지정한 메시지를 우편함의 끝부분에 붙임
* UID : 검색된 결과 메시지에 고유 ID 반환
* CHECK : NOOP과 같음
* CLOSE : 종료

***