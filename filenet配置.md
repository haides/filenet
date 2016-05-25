filenet服务器   192.168.1.60    root    密码: ******

was服务器   wasadmin    密码: ******    暂时没有打补丁(记得先停机)

navigator   192.168.1.212   root    密码: ******

filenet账号密码 p8admin 密码: ******

    域: p8domain
	
    filenet 文件放置在  /image/fnos/
	
        之后需要把文件挂在到这个目录，并且把原有文件拷贝进去
		
        filenet 服务器设置连接池    最小10, 最大50
		
        filenet 对象存储库名称  ceos
		
# ldap    192.168.1.61    root    密码: ******

## 启动 ldap
    cd /opt/IBM/ldap/V6.3/sbin/
    ./ibmslapd
## 停止
    ./ibmslapd -k

## ldap 管理员账号密码
p8admin 密码: ******

## 链接ldap的

# oracle rac  192.168.1.56    orcl


    管理员账号: sys, 密码: ******


    数据库账号  gcd 密码: ******

        fnso    密码: ******
	
权限控制选最后一个，但是不知道其意思
