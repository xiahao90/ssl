# 1. 创建根证书
    #创建根证书私钥：
    #openssl genrsa -out root.key 2048

    #创建根证书请求文件：
    openssl req -new -out root.csr -key root.key

    #创建根证书：
    openssl x509 -req -in root.csr -out root.crt -signkey root.key -CAcreateserial -days 3650

    #在Common Name可填写为root，其余项随意填写保持一致，密码不用填写

# 2. 根据根证书创建服务端证书
    #生成服务器端证书私钥：
    openssl genrsa -out server.key 2048

    #生成服务器证书请求文件，过程和注意事项参考根证书，本节不详述：
    openssl req -new -out server.csr -key server.key

    #生成服务器端公钥证书：
    #openssl x509 -req -in server.csr -out server.crt -signkey server.key -CA root.crt -CAkey root.key -CAcreateserial -days 3650

# 3. 根据根证书创建客户端证书
    #生成客户端证书秘钥：
    openssl genrsa -out client.key 2048

    #生成客户端证书请求文件，过程和注意事项参考根证书，本节不详述：
    openssl req -new -out client.csr -key client.key

    #生客户端证书
    openssl x509 -req -in client.csr -out client.crt -signkey client.key -CA root.crt -CAkey root.key -CAcreateserial -days 3650

    #生客户端p12格式证书，需要输入一个密码（安装证书时需要填写），比如123456
    openssl pkcs12 -export -clcerts -in client.crt -inkey client.key -out client.p12

# 4. nginx配置

    server {
        listen       443 ssl;
        server_name  www.yourdomain.com;# 无域名可填写ip
        ssl                  on;  
        ssl_certificate      /data/sslKey/server.crt;  #server公钥证书
        ssl_certificate_key  /data/sslKey/server.key;  #server私钥
        ssl_client_certificate /data/sslKey/root.crt;  #根证书，可以验证所有它颁发的客户端证书
        ssl_verify_client on;  #开启客户端证书验证  
    }
