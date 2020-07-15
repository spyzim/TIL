## Git 사용에 익숙해지자

> 편리한 명령어들, 내가 git 을 사용하면서 접한 에러들, 그리고 그 원인과 해결방법 등을 정리해보자.



_git 용어 개념 정리_: <https://nanite.tistory.com/39>



* 명령어를 통한 [ .git] 파일의 삭제

  > **find ./ -name ".git" | xargs rm -Rf**

  * git 에 익숙하지 않은 내가 중간과정에 실수가 있어 에러가 발생했을 경우, 초기화 해버릴 때 자주 쓰는 명령어

  * 이 명령어는 git init 을 실행, git remote 로 저장소가 연결이 되어있는 상태에서 시작한다.

  * <find> 명령어로 [ .git] 파일을 삭제하는 것. 하위 디렉토리 내의 [ .git] 파일을 모두 찾아 삭제한다.

    1. git init 을 실행하면 해당 파일에 [ .git] 이 생성되는 것이었다!

       mac 에서는 그 파일을 가시적으로 보여주지 않아서 눈치채지 못했던 것.

    2. 즉, 본질적으로 [ .git] 파일을 삭제하는 것이, git init 명령문을 초기화 시키는 것.

       [ .git] 파일을 수동으로 삭제해도 초기화가 가능하다.

       [https://medium.com/happyprogrammer-in-jeju/git-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-1-%EA%B8%B0%EB%B3%B8-%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8A%B8-81b34f85fe53](https://medium.com/happyprogrammer-in-jeju/git-내부-구조를-알아보자-1-기본-오브젝트-81b34f85fe53)
  
  * **후에 알았지만, .git 을 삭제한다고 모든 기록이 초기화 되는것이 아닌, .git 을 삭제했다는 기록을 commit에 추가해야지 다시 git init 을 하여 push 할 수 있다.** 즉, 되도록이면 쓰지 말자...

​	

* **현재 브랜치 master**

  **커밋할 사항 없음, 작업 폴더 깨끗함.**

  > remote 로 연결 후, 파일을 add 하고 commit 메세지를 입력하려는 순간 나온 에러문

  * 파일을 git 에 push 하려는 시도 중에 마주한 에러문
  * 여러시도 끝에 '커밋할 사항이 없음' 이라는 것은 파일이 제대로 add 가 되지 않았지 때문이란 것을 발견
  *  파일이 제대로 add 되면 commit 은 문제없이 진행된다.





* **힌트: 리모트에 로컬에 없는 사항이 들어 있으므로 업데이트가**
  **힌트: 거부되었습니다. 이 상황은 보통 또 다른 저장소에서 같은**
  **힌트: 저장소로 푸시할 때 발생합니다.  푸시하기 전에
  힌트: ('git pull ...' 등 명령으로) 리모트 변경 사항을 먼저**
  **힌트: 포함해야 합니다.**

  > git init - git remote - git add - git commit 완료 후에 push 하는 중 발생한 에러

  * 파일을 git 에 push 하려는 시도 중에 마주한 에러문
  * 첫 줄 에러문을 보면 리모트와 로컬이 서로 다른 사항이 들어 있으므로(commit이 서로 다르므로),당시에는 remote 에 9개 커밋, local 에 1개 커밋, 업데이트가 거부되었음을 알 수 있다.
  * 로컬에서 pull 을 하여 커밋을 맞추려고 했지만...

* **warning: Pulling without specifying how to reconcile divergent branches is**
  **discouraged. You can squelch this message by running one of the following**
  **commands sometime before your next pull:**

    **git config pull.rebase false  # merge (the default strategy)**
    **git config pull.rebase true   # rebase**
    **git config pull.ff only       # fast-forward only**

  **You can replace "git config" with "git config --global" to set a default**
  **preference for all repositories. You can also pass --rebase, --no-rebase,**
  **or --ff-only on the command line to override the configured default per**
  **invocation.**

  **warning: 공통 커밋 없음**

  > 로컬과 리모트의 커밋을 동기화(pull) 하려는 시도 중 발생한 에러

  * 공동 커밋이 없다는 에러가 뜬다. 즉, 애초에 리모트와 로컬에는 공동 커밋이 없으므로 동기화가 불가능했던 것.

    ** git 은 기록 저장소란 것을 명심하자 **

  * 이 문제를 해결하기 위해서는 remote 의 클론을 만들어서 커밋을 일치시키는 방법을 사용해야 한다.

  * 이 문제의 발단은 로컬을 정리하면서 생긴 문제라고 추측된다. 로컬 정리때,  폴더를 새로 생성하여 파일들을 이동했었음.

  * 그렇다면 commit 이란 무엇일까?! 단순히 메세지 이상의 의미를 갖는 것 같다

    * ##### Commit

      1) 파일 및 폴더의 추가/변경 사항들에 대해 **기록** 하는 것. 저장소 안에 모든 커밋들이 들어있다.

      2) 각 commit 에는 영문/숫자로 이루어진 40자리 고유 이름이 붙는다. 저장소에선 이 이름을 보고 각 커밋을 구분한다.

      3) 커밋을 추가한다는 것은 현재 작업공간의 상태를 커밋으로 만들어서 저장소에 저장한다는 의미.

      4) 커밋이란, 쉽게 말해 작업공간의 어떤 시점의 상태에 대한 스냅샷 이라고 생각할 수 있다.

      ** git status 명령어로 보았던 메세지는 마지막 커밋 이후 작업공간에 변경이 일어난 모든 파일을 나열한 것이었다. 추적되는 파일은 초록색, 그렇지 않은 파일은 빨간색 ( 그 전까지 난 빨간색 파일 이름은 add 되지 않은 것들이구나 정도의 인식만 있었다. )

      5) 로컬 저장소와 원격 저장소의 commit 기록이 일치해야지만 동기화가 가능하다. 그렇지 않다면 원격 저장소로 부터 clone 을 하여 merge 해야 한다.

      

      

    

