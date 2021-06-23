# [Github] 업로드하는 기본적인 방법!!

생성일: 2021년 6월 24일 오전 12:40
속성: DONE
태그: Github

# ✔️ 튜토리얼!!

# 1. 레포지토리 생성

Github에서 회원가입을 한 후 Repository를 생성합니다.

# 2.  업로드할 폴더로 이동

커멘더 창을 통해서 업로드할 폴더가 있는 곳으로 이동합니다

```cd 폴더경로```

# 3. init

커멘더 창에서 아래의 명령어를 통해서, 위의 폴더를 git이 추적할 수 있도록 .git 폴더를 생성.

```git init```

# 4. 상태확인

`git`이 버전관리 대상 파일들의 상태를 파악합니다.

```git status```

# 5. add & commit

변경된 모든 파일을 local repository에 추가합니다.

``` git add , git commit ```

# 6. remote 등록

파일을 푸시 할 repository를 등록합니다!

```git remote add origin (remote rep 주소)```

# 7. push

commit 한 내용을 remote repository에 push( 업로드 ) 합니다.

```git push origin main```

# 요약

```basic
1. cd (폴더경로)
2. git init
3. git status
4. git add % commit
5. remote 등록
6. git push origin main
```

# Link

- [https://victorydntmd.tistory.com/53](https://victorydntmd.tistory.com/53)
