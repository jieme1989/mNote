#ADF前后端 ocelot http代理不是ng配置
	upstream ocelotADFServices {
      server localhost:8000;
      #server localhost:8100;
    }
    server {
        listen 8888;
		server_name localhost;

        location /adfApis/ {
    	    proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            # 后台接口地址
            proxy_pass http://ocelotADFServices/;
    		proxy_redirect default;
    		# 跨域设置
    		#add_header 'Access-Control-Allow-Origin' *;
    		#add_header 'Access-Control-Allow-Credentials' 'true';
            #add_header 'Access-Control-Allow-Methods' *;
            #add_header 'Access-Control-Allow-Headers' *;
           
    		 # 开启gzip
            gzip on;
    
               # 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
            #gzip_min_length 5k;
            
            # gzip 压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
            gzip_comp_level 2;
            
            # 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
            gzip_types text/plain text/json application/javascript application/json application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png application/vnd.ms-fontobject font/ttf font/opentype font/x-woff image/svg+xml;
            
            # 是否在http header中添加Vary: Accept-Encoding，建议开启
            gzip_vary on;
            
            # 禁用IE 6 gzip
            gzip_disable "MSIE [1-6]\.";
            
            # 设置压缩所需要的缓冲区大小     
            gzip_buffers 32 4k;
            
            # 设置gzip压缩针对的HTTP协议版本
            #gzip_http_version 1.0;
        }
    	location /adfWeb {
            alias  projs/adfdist/;
            index  index.html index.htm;
        }
    }