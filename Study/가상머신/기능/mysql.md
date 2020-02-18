### mysql root 패스워드 분실

> [https://zetawiki.com/wiki/MySQL_root_%ED%8C%A8%EC%8A%A4%EC%9B%8C%EB%93%9C_%EB%B6%84%EC%8B%A4](https://zetawiki.com/wiki/MySQL_root_패스워드_분실)

```
버전 5.5.56

service mysqld stop // 중지

// 인증 생략 옵션 + 안전모드로 데몬 실행
/usr/bin/mysqld_safe --skip-grant &   
/usr/bin/mysqld_safe --skip-grant-tables &

이제 패스워드 없이 접속 가능
/usr/bin/mysql -u root mysql

새 패스워드 지정
UPDATE mysql.user SET password=PASSWORD('패스워드') WHERE user='root' AND Host='localhost';
FLUSH PRIVILEGES;
quit

service mysqld restart


```

