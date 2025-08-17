---
layout: single
title: OSI 계층
categories: 네트워크
tag: [네트워크]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# OSI 계층

네트워크에서 통신이 일어나는 과정을 7단계로 나눈 것

**_A_**pplication **_P_**resentation **_S_**ession **_T_**ransport **_N_**etwork **_D_**atalink **_P_**hysical

아파서티낸다피

## 1계층 : 물리 계층

### 통신단위 : 비트

### 장치

- 리피터 : 신호증폭
- 허브 : 리피터를 여러 개 포트로 확장한 장치, 무조건 모두에게 전송

### 하는일

- 물리적 연결
- 비트 신호 전송

## 2계층 : 데이터링크 계층

### 통신단위 : 프레임

### 장치

- 브리지 : MAC 주소 기억, 같은 네트워크 장비를 연결
- 스위치 : MAC 주소 기억, 프레임의 정확한 전송

### 프로토콜

#### 포인트투포인트 프로토콜

- HDLC : 양방향 통신을 안정적으로 함
- ADCCP : HDLC의 미국버전

#### 근거리 네트워크 프로토콜

- LLC : 네트워크 계층과 통신할 수 있도록 인터페이스 제공

### 하는일

- 프레임에 주소부여
- 에러검출
- 재전송
- 흐름제어

## 3계층 : 네트워크 계층

### 통신단위 : 패킷

### 장치

- 라우터 : 라우팅
- 공유기
- L3 스위치

### 프로토콜

- IP(다른문서에서 설명)
- ICMP : IP 패킷의 전달 상태를 알리는 제어 메시지용 프로토콜
- 라우팅 프로토콜(다른 문서에서 설명)
- ARP : MAC주소와 IP주소 연결

### 하는일

- 주소부여
- 경로설정

## 4계층 : 전송 계층

### 통신단위 : 세그먼트(TCP), 데이터그램(UDP)

### 장치

- 게이트웨이 : 서로 다른 네트워크를 연결해주는 장비
- 로드밸런서 : 여러 서버에 들어오는 요청(트래픽)을 골고루 분산시켜주는 장비

### 프로토콜

- TCP(다른문서에서 설명)
- UDP(다른 문서에서 설명)

### 하는일

- 패킷 생성 및 전송
- 오류검출 및 복구
- 흐름제어
- 중복검사

## 5계층 : 세션 계층

### 통신단위 : 데이터

### 프로토콜

- RPC : 원격 시스템에서 함수/프로시저를 실행할 수 있도록 함
- NetBIOS : 네트워크 기반 입출력을 위한 세션 서비스
- PPTP : VPN 연결에서 세션을 설정하고 관리

### 하는일 : TCP/IP 세션을 만들고 없앰

## 6계층 : 표현 계층

### 프로토콜

- SSL, TLS : 데이터 암호화 및 보안 통신 (TLS는 SSL 후속버전, 현재 SSL은 완전히 퇴출됨. 하지만 계속 TLS이 관습적으로 SSL로 불리는중)
- MIME : 이메일에서 다양한 파일 형식을 처리할 수 있도록 해주는 포맷
- JPEG, GIF, PNG : 이미지 인코딩/디코딩 (데이터 형식 변환)
- MP3, MPEG : 오디오/비디오 압축 형식
- ASCII, EBCDIC : 문자 인코딩 방식

### 하는일 : 데이터의 표현방식을 결정(데이터변환, 압축, 암호화)

## 7계층 : 응용 계층

### 통신단위 : 데이터

### 프로토콜

- HTTP : 웹페이지 전송
- FTP : 파일 전송
- SMTP : 전자메일을 보내기 위한 프로토콜
- POP3 : 메일 서버에 저장된 이메일을 받아오는 프로토콜
- IMAP : 메일을 서버에서 다운로드하지 않고, 서버에 그대로 두고 읽고 관리할 수 있게 해주는 프로토콜
- TELNET : 원격접속
- SNMP : 네트워크 장비 관리 및 모니터링

### 하는일 : 응용서비스 수행

<table style="border: 2px;">
  <thead>
    <tr>
      <th style="border: 1px solid black;"> </th>
      <td style="border: 1px solid black;"> 통신단위 </td>
      <td style="border: 1px solid black;"> 장치 </td>
      <td style="border: 1px solid black;"> 프로토콜 </td>
      <td style="border: 1px solid black;"> 하는일 </td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th style="border: 1px solid black; text-align:center"> 물리 계층 </th>
      <td style="border: 1px solid black;" > 비트 </td>
      <td style="border: 1px solid black;" >
        <ul>
          <li>리피터</li>
          <li>허브</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" > </td>
      <td style="border: 1px solid black;" >
        <ul>
            <li>물리적 연결</li>
            <li>비트 신호 전송</li>
        </ul>
      </td>
    </tr>
    <tr>
      <th style="border: 1px solid black; text-align:center" > 데이터링크 계층 </th>
      <td style="border: 1px solid black;" > 프레임 </td>
      <td style="border: 1px solid black;" >
        <ul>
          <li>브리지</li>
          <li>스위치</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" > 
        <ul>
          <li>HDLC</li>
          <li>ADCCP</li>
          <li>LLC</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" >
        <ul>
            <li>프레임에 주소부여</li>
            <li>에러 검출</li>
            <li>재전송</li>
            <li>흐름제어</li>
        </ul>
      </td>
    </tr>
    <tr>
      <th style="border: 1px solid black; text-align:center" > 네트워크 계층 </th>
      <td style="border: 1px solid black;" > 패킷 </td>
      <td style="border: 1px solid black;" >
        <ul>
          <li>라우터</li>
          <li>공유기</li>
          <li>L3 스위치</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" > 
        <ul>
          <li>IP</li>
          <li>ICMP</li>
          <li>라우팅 프로토콜</li>
          <li>ARP</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" >
        <ul>
            <li>패킷 생성 및 주소부여</li>
            <li>경로 설정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <th style="border: 1px solid black; text-align:center" > 전송 계층 </th>
      <td style="border: 1px solid black;" > 
        <ul>
          <li>세그먼트(TCP)</li>
          <li>데이터그램(UDP)</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" >
        <ul>
          <li>게이트웨이</li>
          <li>로드밸런서</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" > 
        <ul>
          <li>TCP</li>
          <li>UDP</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" >
        <ul>
            <li>세그먼트에 패킷 넣어 전송</li>
            <li>오류검출 및 복구</li>
            <li>흐름제어</li>
            <li>중복검사</li>
        </ul>
      </td>
    </tr>
    <tr>
      <th style="border: 1px solid black; text-align:center" > 세션 계층 </th>
      <td style="border: 1px solid black;" > 
        <ul>
        </ul>
      </td>
      <td style="border: 1px solid black;" >
      </td>
      <td style="border: 1px solid black;" > 
        <ul>
          <li>RPC : 원격 시스템에서 함수/프로시저를 실행할 수 있도록 함.</li>
          <li>NetBIOS : 네트워크 기반 입출력을 위한 세션 서비스</li>
          <li>PPTP : VPN 연결에서 세션을 설정하고 관리</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" >
            TCP/IP 세션을 만들고 없앰
      </td>
    </tr>
    <tr>
      <th style="border: 1px solid black; text-align:left" > 표현 계층 </th>
      <td style="border: 1px solid black;" > </td>
      <td style="border: 1px solid black;" > </td>
      <td style="border: 1px solid black;" >
        <ul>
          <li>SSL, TLD</li>
          <li>MIME</li>
          <li>JPEG, GIF, PNG</li>
          <li>MP3, MPEG</li>
          <li>ASCII, EBCDIC</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" >
        <ul>
            <li>데이터 변환</li>
            <li>데이터 압축</li>
            <li>데이터 암호화</li>
        </ul>
      </td>
    </tr>
    <tr>
      <th style="border: 1px solid black; text-align:center" > 응용 계층 </th>
      <td style="border: 1px solid black;" > </td>
      <td style="border: 1px solid black;" > </td>
      <td style="border: 1px solid black;" >
        <ul>
          <li>HTTP</li>
          <li>FTP</li>
          <li>SMTP</li>
          <li>POP3</li>
          <li>IMAP</li>
          <li>TELNET</li>
          <li>SNMP</li>
        </ul>
      </td>
      <td style="border: 1px solid black;" >응용서비스 수행</td>
    </tr>
  </tbody>
</table>