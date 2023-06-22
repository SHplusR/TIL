1\. 
[http://www.h2database.com/html/download-archive.html](http://www.h2database.com/html/download-archive.html)

에서 1.4.200 다운로드~~

압축을 풀고 터미널을 켜서 해당 파일이 있는 경로로 이동한다~

그리고 h2폴더에서 ./bin/h2.sh을 입력하여 shell을 실행한다.

2\. 그러면 

[##_Image|kage@R9X3b/btsk1yvyEAo/lUdi2KEBkS7aPYI8FPPWik/img.png|CDM|1.3|{"originWidth":1314,"originHeight":850,"style":"alignCenter"}_##]

다음과 같이 보인다. 

여기서 주의점~~

기존에 적혀있던 url은 지우고 jdbc:h2:~/test 로 연결한다.( 비밀번호 입력없이 그냥 들어가면 됨)

이후에 연결할때는 기존의 jdbc:h2:tcp..... 로 입력하여 들어간다.

\*\* 만약 오류가 날 경우

기존의 user의 test.mv.db를 삭제하고 

cat >> test.mv.db 

명령어를 입력하여 임의로 만들어줌.

[##_Image|kage@bwlV69/btsk2BSZsXB/D0Aa3YnjmgqRmJY6aKUNd0/img.png|CDM|1.3|{"originWidth":1712,"originHeight":714,"style":"alignCenter"}_##]

그러면 접속 완료~

인텔리제이에 접속정보를 넣는 방법은

persistence.yml에 밑과 같은 정보를 넣는다.

```
<property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
<property name="javax.persistence.jdbc.user" value="sa"/>
<property name="javax.persistence.jdbc.password" value=""/>
<property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
<property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect" />
```

user, password, url의 value는  본인이 설정한대로 바꾸기