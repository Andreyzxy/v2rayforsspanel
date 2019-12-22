## 前情提要  
  参考ocserv443  
  ![](/data/s1.jpg)  

## Nginx网站设置  
  将需要做端口转发的网站中，配置文件添加如下代码    
  ```  
    location /v2ray { # 与 V2Ray 配置中的 path 保持一致
          proxy_redirect off;
          proxy_pass http://127.0.0.1:10000; # 假设v2ray的监听地址是10000
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
          proxy_set_header Host $host;
          # Show real IP in v2ray access.log
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
   ```  
service nginx restart

## 前端sspanel设置  
  * 普通端口设置  
  参考 [节点添加](https://github.com/v2rayv3/pay-v2ray-sspanel-v3-mod_Uim-plugin/wiki/%5B%E9%85%8D%E7%BD%AE%5D-%E4%BD%9C%E4%B8%BA-V2Ray-%E5%90%8E%E7%AB%AF)
  ```  
  // TCP 示例
  非CDN域名或者ip;非0;2;tcp

  // TCP + TLS 示例
  非CDN域名或者ip;非0;2;tcp;tls
  ```  
  
  * 复用端口设置 (本教程设置)  
  ```  
  // WS + TLS
  节点IP;0;2;tls;ws;path=/v2ray|host=Nginx中做端口转发的网站|inside_port=10000|outside_port=443
  ```

## 节点安装V2ray

```  
bash <(curl -L -s  https://raw.githubusercontent.com/RManOfCN/crack-v2ray-sspanel-v3-mod_Uim-plugin/master/install-release.sh) \
--panelurl sspanel网站地址 --panelkey xxx --nodeid xxx \
--downwithpanel 1 --speedtestrate 6 --paneltype 0 --usemysql 0
```  
systemctl start v2ray  
systemctl enable v2ray  
systemctl status v2ray  
