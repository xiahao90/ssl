# 可以一键生成，看[auto目录]([http://ace.ajax.org/](https://github.com/xiahao90/ssl/tree/main/auto))

# 双向认证，web系统点击生成，暂未公开

    docker run -e JWT_SK=8VLb5IE3cyOZg7sHWBdYmaFDUn1rt60J -v /ssl:/home/ssl -p 8003:8003 xiahao90/autossl:v0.10

    -e JWT_SK=8VLb5IE3cyOZg7sHWBdYmaFDUn1rt60J #jwt加密密钥，建议替换，不然就默认密钥，32位
    -v /ssl:/home/ssl #目录映射，重启后数据还在。不映射重启就不在了
    -p 8003:8003 #docker内系统的端口是8003
