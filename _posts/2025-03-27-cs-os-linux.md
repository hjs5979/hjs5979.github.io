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

## vi {생성할 또는 열어볼 파일명}

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

## nano

## tail

## head

## net stat

## iptables

## passwd

## nslookup

## ifconfig

## nohup
