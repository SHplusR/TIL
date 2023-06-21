\
1.

window에서 실행 > sercices.msc를 입력해 서비스 창을 열고 mysql , mariadb...등을 삭제함.

작업관리자에 들어가서 백그라운드에서 진행중인 mysqld가 있는지 확인하고 있다면 지움.

[##_Image|kage@RJay0/btskQQxqmhc/3f9tBdnOVvJq6v8UaZWouK/img.png|CDM|1.3|{"originWidth":588,"originHeight":160,"style":"alignCenter"}_##]


2\. 자신의 mysql이 어디있는지 확인하고, cd 명령어 통해 해당 경로로 이동.

C:\\Program File\\MariaDB 10.5\\bin >>보통 이거임.

3\. mysqld.exe --skip-grant 실행. ( 권한을 무시한다는 뜻)

(권한 무시 알람 받기 싫으면 mysqld -uroot --skip-grant-tables 입력)

4\. 다른 cmd창을 열어 2번에서 알게된 경로로 똑같이 이동.

cd C:\\Program File\\MariaDB 10.5\\bin

여기서 mysql.exe 입력.

5. 

use mysql;

Flush Privileges; >> 앞에 권한 무시한것을 끄기위함

set password for 'root'@'localhost' =password('변경할비밀번호');

Flush Privilieges; >> 새로운 비밀번호 적용을 위함.

quit (exit);

이후 다 끄고 새창을 열어

mysql -u root -p등으로 mysql에 접속해 비밀번호를 입력하면 된다. 
