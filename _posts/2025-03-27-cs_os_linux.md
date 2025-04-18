---
layout: single
title: 리눅스 명령어
categories: OS
tag: [OS]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 리눅스 명령어

## ls

현재 위치의 파일 목록 조회하는 명령어

###### ls -a : 숨긴파일을 포함한 모든 항목 표시

###### ls -l : 각 항목의 상세 정보들을 함께 표시

###### ls -R : 하위 디렉토리의 내용들도 표시

## cd {경로}

뒤에 덧붙여진 경로로 이동하는 명령어

###### cd ~ : 홈 디렉토리로 이동

###### cd / : 최상위 디렉토리로 이동

###### cd . : 현재 디렉토리

###### cd .. : 상위 디렉토리로 이동

###### cd - : 이전 경로로 이동

## mkdir

디렉토리를 생성하는 명령어

###### mkdir dir : dir 디렉토리 생성

###### mkdir dir1 dir2 : dir1, dir2 생성

###### mkdir -p dir1/dir2 : dir1 디렉토리와 dir1의 하위에 dir2 디렉토리 생성

###### mkdir -m 700 dir1 : 권한을 부여한 디렉토리 생성

## cp {복사할대상} {붙여넣을 경로 또는 새 파일명}

파일을 복사하는 명령어, 디렉토리 복사 시에는 -r을 사용

## mv {옮길 대상} {대상 디렉토리 또는 새 파일명}

파일이나 디렉톨를 옮기거나 이름을 변경할 때 사용

## rm {삭제할 대상}

파일이나 디렉토리를 삭제, 디렉토리를 삭제할 때, -r을 사용

## cat

###### cat file : file 파일 내용 출력

###### car file1 file2 > file1_2 : file1, file2를 file1_2로 합치기

###### car newFile : newFile 파일 생성

###### car file1 >> file2 : file1 내용을 file2 에 붙이기

## > , >>, <

###### > : 기존에 있는 파일 내용을 지우고 저장

###### >> : 기존 파일 내용 뒤에 덧붙여서 저장

###### < : 파일의 데이터를 명령에 입력

## touch

###### touch newFile : newFile 파일 생성

###### touch -c file1 : file1 의 최근 업데이트 일자를 현재 시간으로 변경

###### touch -t 202503272025 file1 : file1의 최근 업데이트 일자를 변경

## vim {생성할 또는 열어볼 파일명}

##### i : 입력모드

##### ESC : 입력모드 종료

##### :w : 저장

##### :q : 종료

##### :wq : 저장 후 종료

##### :q! : 저장 없이 강제 종료

##### h, l : 좌, 우로 한 글자 이동

##### j, k : 아래, 위로 한 줄 이동

##### 0, $ : 현재 줄의 맨 앞으로, 맨 뒤로

##### gg, G : 문서 맨 처음으로, 맨 끝으로

##### :숫자 : 해당 줄로 이동

##### x : 커서 위치 문자 삭제

##### dd : 한 줄 삭제

##### yy : 한 줄 복사

##### p : 붙여넣기

##### u : 실행 취소

##### ctrl + r : 다시 실행

## echo

###### echo “Hello World” : 문자열 출력

###### echo “Hello World” > hello.txt : 파일에 텍스트 작성

## grep

###### grep “pattern” file.txt : file 에서 특정 패턴의 문자열 검색

###### ls | grep “pattern” : ls한 결과에서 특정 패턴과 일치하는 결과만 표시

## chmod {mode} {대상}

###### 사용자 : u(파일을 소유한 사용자), g(그룹에 소속된 사용자), o(그 외 사용자), a(모든 사용자)

###### 권한 : r(읽기), w(쓰기), x(실행)

###### chmod u=rw file1 : 소유자에게 file1 읽기, 쓰기 권한 지정

###### chmod g+w file1 : 그룹 사용자에게 file1 쓰기 권한 추가

##### chmod 751 file1

###### 755

(7=소유자)(5=그룹)(1=그외)

4 : 읽기 / 2 : 쓰기 / 1 : 실행

7 = 4 + 2 + 1 => 소유자에게 모든 권한

5 = 4 + 1 => 그룹 사용자에게 읽기와 실행 권한

1 = 1 => 그 외 사용자에게 실행 권한

## chown {소유자}:{그룹} {대상}

대상 파일의 소유자와 그룹을 지정

## ps

###### -a : 터미널과 연관된 프로세스를 출력

###### -e : 커널 프로세스를 제외한 모든 프로세스를 출력

###### -f : 출력을 풀 포맷으로 표기

###### -l : 출력을 긴 포맷으로 표기

###### -p : 프로세스 ID 지정하여 출력

###### -u : 프로세스의 소유자를 기준으로 출력

###### -x : 데몬 프로세스처럼 터미널에 종속되지 않은 프로세스를 출력

###### ps -ef : 동작중인 모든 프로세스를 풀 포맷으로 출력

###### ps -aux : 모든 프로세스를 소유자 정보와 함께 출력

###### ps -el : 모든 프로세스 긴 포맷으로 출력

###### ps -p {프로세스ID}

## kill

###### kill -9 {PID} : 강제종료

###### kill -15 {PID} : 작업종료

## systemctl {명령어} {서비스명}

###### systemctl start {서비스명} : 서비스 시작

###### systemctl restart {서비스명} : 서비스 재시작

###### systemctl status {서비스명} : 서비스 상태 확인

###### systemctl reload {서비스명} : 서비스 중지하지 않고 설정 반영

###### systemctl enable {서비스명} : 시스템 재부팅 시, 자동으로 서비스 실행하도록 설정

###### systemctl disable : enable 취소

## crontab

배치 실행 서비스

##### crontab -e : 크론탭 생성 및 설정

명령어 입력 시, 편집창이 나옴

- - - - - 명령어
          ┬ ┬ ┬ ┬ ┬
          │ │ │ │ └───── 요일 (0~7) 0 또는 7 = 일요일
          │ │ │ └──────── 월 (1~12)
          │ │ └────────── 일 (1~31)
          │ └──────────── 시 (0~23)
          └────────────── 분 (0~59)

예시)

1. \* \* \* \* \* /home/user/backup.sh => 매 분마다 실행
2. 0 \* \* \* \* /home/user/backup.sh => 매 정각마다 실행
3. 0 0 \* \* \* /home/user/backup.sh => 매일 자정에 실행
4. 0 9 \* \* 1-5 /home/user/backup.sh => 월~금 오전 9시에
5. \*/10 \* \* \* \* /home/user/backup.sh => 10분마다 실행
6. 0 0 1 \* \* /home/user/backup.sh => 매달 1일 자정에
7. 0 3 \* \* 0 /home/user/backup.sh => 매주 일요일 새벽 3시에

##### crontab -l : 크론탭 내용 보기

##### crontab -r : 크론탭 삭제

## nano

##### ctrl + O : 저장

##### ctrl + X : 종료

##### crtl + K : 잘라내기

##### ctrl + U : 붙여넣기

##### crtl + W : 단어 찾기

##### ctrl + C : 줄번호 보기

##### ctrl + \_ : 줄번호 입력해서 바로 이동

## tail

## head

## netstat {옵션}

네트워크 연결 상태를 확인할 수 있는 명령어

###### -a : 모든 연결
###### -t : TCP만
###### -u : UDP만
###### -n : 호스트명 대신 IP주소, 포트명 대신 포트번호로 표시
###### -l : 수신 대기 중인 소켓만 표시
###### -p : 연결한 프로그램과 PID 표시
###### -r : 라우팅 테이블 출력
###### -i : 네트워크 인터페이스 정보 표시

###### netstat -an : 현재 열려있는 모든 연결 확인
###### netstat -ltn : 수신 대기 중인 포트 확인
###### netstat -tulpn : 어떤 프로그램이 어떤 포트를 사용 중인지 확인

## iptables

방화벽(Firewall) 기능을 설정하고 제어하는 명령어


## passwd

## nslookup

## ifconfig

## nohup {파일} &

백그라운드로 실행
