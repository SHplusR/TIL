사실 난 이미 설치한적이 있다...

하지만 .... 비밀번호를 까먹었고, 이를 수정하려 명령어를 치고 들어가자 무한오류가 뜨는것을 보고 그냥 새로 깔자고 생각했다.

내개 생겼던 오류는 아래와 같다.

우선  **brew install mysql**  명령어를 치면, 짧게는 몇초 길게는 몇분 뒤 아래와 같은 메세지가 뜨며 install이 완료된다.

```
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -u root

To start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
==> Summary
🍺  /opt/homebrew/Cellar/mysql/8.0.33_1: 318 files, 300.0MB
==> Running `brew cleanup mysql`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```

여기서 중요한 내용은, 

mysql이 root 비번 없이 설치되었고, 이를 수정하고 싶으면

**mysql\_secure\_installation을 입력.**

그냥 접속만 하기 위해서는

**mysql -u root를 입력..**

근데 내 경우에는, 삭제후 재설치했음에도 불구하고 

**mysql\_secure\_installation 입력시 >> Access denied for user 'root'@'localhost' (using password:yes)**

라고 나오고

**mysql -u root 입력시 >> enter를 누르면 **Access denied for user 'root'@'localhost' (using password:no)****

라고 나왔다... 서치를 약 2시간동안 했는데 다들 나와 경우가 달랐다.

사실 더 많은 오류가 발생했다.(무한오류발생, 서버가 이미 시작되었다고 해놓고 brew status하면 mysql은 none인 상황 등..)

아래는 내 해결방법이다.

\---------------------------------------------

1\. brew uninstall mysql 

\>> mysql 을 삭제한다.

하지만 이 명령어만 친다고 mysql 이 완전히 삭제되는것이 아니다. 

Finder > 찾기를 통해 mac내에 있는 모든 mysql을 삭제한다.

[##_Image|kage@bpWeQY/btsjbqZYxgs/IxCKrTey6bpB402apveTxk/img.png|CDM|1.3|{"originWidth":1818,"originHeight":816,"style":"alignCenter"}_##]

2\. 터미널을 통해 모든 mysql관련 파일을 삭제한다.

**sudo su rm -rf /usr/local/mysql\***

**brew list...등**

\>> 이전에는 brew list만 확인하고 없다고 생각해서 그냥 넘어갔는데, 정말 놀랍게도...local에 어딘가에 숨어있는 mysql을 삭제하지 않아서 계속 이런일이 있던것같다. 저렇게 모든 파일을 다 삭제하니 해결이 되었다.

\--------------------------------------

이후 모두가 아는것처럼, 다시 mysql을 설치한다.

1\. brew install mysql

2\. mysql\_secure\_installation 

[##_Image|kage@LdF2k/btsjaK5z3za/4MDrayfcxBz1gGVvLMBLZ0/img.png|CDM|1.3|{"originWidth":1010,"originHeight":330,"style":"alignCenter"}_##]

악!!!!!! 드디어 나왔다..............

드디어 새 비밀번호를 설정할 수 있다......

진행하다보면 y/n로 대답해야하는 설정들이 나오는데 해당 내용은 아래와 같다.

-   VALIDATE PASSWORD plugin? : '가이드' 대로 비밀번호를 설정할것인가?

\>> 난 no 했다. 검색해보니 내용이 꽤나 길어서....또 까먹을것같았다...

-   Remove anonymous users? : 익명사용자를 삭제할것인가?

\>> 난 yes했다. 문제 생기면 나중에 추가할 수 있겠지

-   Disallow root login remotely? : localhost 외 ip에서 root 아이디로 접속가능을 허락할지? 

\>> 난 no 했다. yes하면 원격접속이 안되니까

-   Remove test database and access to it? : test db를 삭제할것인가?

\>> 난 yes했다. 어차피 필요없음

-   Reload privilege tables now? :

\>> 난 yes했다. 권한변경한건없지만 혹시 모르니

[##_Image|kage@b7S4VP/btsi4cirAp3/dnWTkjzpO9tHKbSHeegL41/img.png|CDM|1.3|{"originWidth":1526,"originHeight":124,"style":"alignCenter"}_##]


\--------------------------------------

6/9 내용추가

사용자 등록방법

1. mysql -u root -p 입력 후 비밀번호 입력
2. show databases; 로 현재 db목록 보기
3. 사용할 db로  use 사용할db명; 적기.
4. CREATE USER '사용자명'@'%' IDENTIFIED BY '비밀번호'; 로 사용자 계정 만들기
5. grant all privileges on 권한부여할 db명.* to '사용자명'@'%'; 로 권한부여하기.

