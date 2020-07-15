## Linux 명령어

>  처음에 꼭 알아야 하는 명령어. 이것만은 무조건 기본으로 알고 가야한다.

* pwd ( print working directory ) : 현재 작업중인 디렉토리
* cd ( change directory ) : 디렉토리 변경
* ls ( list segments ) : 파일 목록 표시

> 디렉토리 생성.이동 & 텍스트 생성.수정

* mkdir ( make directory ) : 디렉토리 생성

* pwd, cd, ls

* touch : 텍스트 파일 생성

* vi : 파일 열기

  예시)

  - vi example.txt - text 파일 생성
  - i 눌러서 수정 모드 진입
  - 수정 완료 후 esc + wq + enter
  - 작업 완료



## git 과 Linux

> github 에 파일 공유 (공유가 정확한 용어인지는 모르지만 직관적으로 와닿는 개념으로 공유라고 썼다.)

1. git 명령어

   * git 최초 설정 시

     git init - git 을 사용하기 위해 꼭 먼저 해줘야 하는 설정. 한번 하고나면 이후에는 필요없다.

     git remote add origin github 주소 - github 저장소 url 을 origin 이라는 이름으로 저장, 등록

   * git add 파일명

   * git commit -m "코멘트 남길 메세지" - add 할 때마다 새로운 코멘트를 달자

   * git push origin master - github 에 등록

