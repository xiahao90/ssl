# 自动化脚本运行直接生成，运行cmd.sh

# 1. 修改cmd.sh中的domain为你的域名，可以随便输，也可以不改。

# 2. 修改pass.txt中的密码。默认123456

# 3. nginx配置
    server {
        listen       443 ssl;
        server_name  www.baidu.com;# 无域名可填写ip
        ssl                  on;  
	    ssl_certificate     /newcerts/www.baidu.com/server.cert.pem;
	    ssl_certificate_key /newcerts/www.baidu.com/server.key.pem;
	    ssl_client_certificate /certs/intermediateca-chain.cert.pem;
	    ssl_verify_client on;  #开启客户端证书验证  
    }
# 4. 修改自项目https://github.com/demokn/OpenSSLCA


# 5. 吊销证书

	#将吊销的客户端证书序列号添加到CA数据库中：
	openssl ca -config openssl.cnf -revoke newcerts/www.baidu.com/client.cert.pem -keyfile private/intermediateca.key.pem -cert certs/intermediateca.cert.pem -passin file:pass.txt
	#更新CRL文件以包含新的吊销信息：
	openssl ca -config openssl.cnf -gencrl -keyfile private/intermediateca.key.pem -cert certs/intermediateca.cert.pem -out crl/intermediateca.crl.pem -passin file:pass.txt
 
 # 5. 吊销证书后更新ngixn，需要重启

	server {
        listen       443 ssl;
        server_name  www.baidu.com;# 无域名可填写ip
        ssl                  on;  
	    ssl_certificate     /newcerts/www.baidu.com/server.cert.pem;
	    ssl_certificate_key /newcerts/www.baidu.com/server.key.pem;
	    ssl_client_certificate /certs/intermediateca-chain.cert.pem;
            ssl_crl /crl/intermediateca.crl.pem; #吊销脚本
	    ssl_verify_client on;  #开启客户端证书验证  
    	}
