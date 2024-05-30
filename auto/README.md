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
