.. contents::
   :depth: 3
..

nginx反向代理harbor
===================

::

   worker_processes 1;
   events {
     worker_connections 1024;
   }
   http {
     include mime.types;
     default_type application/octet-stream;
     sendfile on;
     keepalive_timeout 65;
     server {
       listen 80;
       server_name harbor.wd.dolphin-labs.com;
       return 301 https://$host$request_uri;
     }

     server {
       listen 443 ssl;
       server_name harbor.wd.dolphin-labs.com;
       charset utf-8;
       ssl_certificate ssl/harbor.wd.dolphin-labs.com.crt;
       ssl_certificate_key ssl/harbor.wd.dolphin-labs.com.key;
       ssl_session_timeout 5m;
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
       ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
       ssl_prefer_server_ciphers on;
       client_max_body_size 0;
       proxy_connect_timeout      600;
       proxy_send_timeout         600;
       proxy_read_timeout         600;
       proxy_buffer_size          4k;
       proxy_buffers              4 32k;
       proxy_busy_buffers_size    64k;
       proxy_temp_file_write_size 64k;
       proxy_http_version 1.1;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP         $remote_addr; # pass on real client's IP
       proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;

       location / {
           proxy_redirect off;
           proxy_ssl_verify off;
           proxy_ssl_session_reuse on;
           proxy_pass http://36.24.243.91;
       }
     }
   }
