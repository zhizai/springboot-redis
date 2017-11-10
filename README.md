1.设计user表 --对应的key规则
2.注册用户：例如==》set user:userid:1:username zhangsan
                 set user:userid:1:password 111111
3.登录的时候用到用户名来查询是否有该用户，所以需要冗余设计
                 set user:username:zhangsan:userid 1
4.生成userId  incr global:userid(这样就达到了自增长效果)

5.发微博 post（大致上就是这样的。）
post:postid:3:time timestamp
post:postid:3:userid 5
post:postid:3:content 'this is my first plush'
.生成postid  incr global:postid(这样就达到了自增长效果)
6。查询新注册用户的信息（前50个）
通过链表link从最左端插入就可以了,这时就需要在注册的时候就把用户的ID放到lpush链表中
lpush('newUserLink',userid);
查询前50个最新用户
ltrim('newUserLink',0,49)
