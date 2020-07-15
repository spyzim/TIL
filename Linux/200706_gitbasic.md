## git 과 Linux

> 내가 배워 사용할 줄 아는 것 위에 따로 공부를 더 하여 최초 올린 것에 내용을 추가했다.

1. git 명령어

   * git 최초 설정 시

     git init 

     * git 을 사용하기 위해 꼭 먼저 해줘야 하는 설정. 같은 폴더 내에서는 한번 하고나면 이후에는 필요없다.
     * 폴더를 생성 한 후, 그 안에서 실행하는 명령어이다.
     * 새로운 git 저장소를 만드는 명령어.

     ​			

     git remote add origin <github 주소> 

     * github 저장소 url 을 origin 이라는 이름으로 저장, 등록

     * 원격 서버의 주소를 git에게 알려주는 것이다.

       ** origin : git 가 복사해 온 저장소를 가리키기 위해 기본적으로 사용하는 이름

   

   

   * git add <파일명>
     * git 의 기본 작업의 첫 번째 단계

   

   

   * git commit -m "코멘트 남길 메세지" 
     * 이 명령으로 변경 내용이 확정된다.

   

   

   * git push origin master

     - github 에 등록

     * 변경 내용을 원격 서버에 올린다.

   

   

   * git status

     * 현재 파일들의 상태를 볼 수 있다.

     

   * git pull origin master

     	* 다른 사람(혹은 나)의 작업을 받아올 수 있는 명령어

