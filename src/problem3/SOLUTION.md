Provide your solution here:

Step 1: Verify Memory Usage
free -h (command to check the total, used, and available memory)
top (command identify processes consuming the most memory)

Step 2: Check NGINX Memory Usage
ps aux | grep nginx (command check the memory usage of NGINX worker processes)

Step 3: Review NGINX Configuration
1. sudo su
2. vi /etc/nginx/nginx.conf
3. then fill with this config template 
worker_processes auto;  # Automatically set based on CPU cores
worker_rlimit_nofile 100000;  # Increase max file descriptors

events {
    worker_connections 8192;  # Set higher worker connections to handle more clients
    use epoll;  # Use efficient event-driven I/O (Linux)
    multi_accept on;  # Accept multiple connections at once
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    keepalive_requests 10000;  # Allow more keepalive requests before closing
    types_hash_max_size 4096;

    # Buffers to reduce memory usage and prevent request overloads
    client_body_buffer_size 512k;
    client_max_body_size 50M;  # Adjust if dealing with large files
    large_client_header_buffers 4 32k;

    # Optimize response buffering
    output_buffers 2 512k;
    postpone_output 1460;

    # Compression to save bandwidth
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_vary on;
    gzip_comp_level 5;
    gzip_buffers 16 8k;
    gzip_min_length 1024;

    # Limit connections & requests to prevent memory overload
    limit_conn_zone $binary_remote_addr zone=addr:10m;
    limit_conn addr 100;  # Limit per IP
    limit_req_zone $binary_remote_addr zone=limit:10m rate=50r/s;
    limit_req zone=limit burst=100 nodelay;

    # Cache static files in memory for faster response
    open_file_cache max=200000 inactive=20s;
    open_file_cache_valid 30s;
    open_file_cache_min_uses 2;
    open_file_cache_errors on;

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend_servers;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_buffers 16 64k;
            proxy_buffer_size 128k;
            proxy_busy_buffers_size 256k;
            proxy_max_temp_file_size 128m;
            proxy_read_timeout 90;
        }
    }
}

note :
If Memory Usage is Too High
a. Reduce worker_connections (example, from 8192 to 4096)
b. Adjust proxy_buffers and client_body_buffer_size
c. Lower gzip_comp_level (example, from 5 to 3)
If requests are slower than 100ms
a. Use fast upstream servers (example, optimize backend API)
b. Enable caching in NGINX
c. Use a CDN for static files

4. nginx -t (Check config nginx)
5. systemctl restart nginx (Restart nginx)

Step 3: Monitor Traffic and Logs
1. Check log for unusual traffic patterns or errors
sudo tail -f /var/log/nginx/access.log 
sudo tail -f /var/log/nginx/error.log
2. Analyze real-time traffic and identify spikes or anomalies
yum  install ngxtop
ngxtop

Step 4: Check for Resource Leaks
1. monitor memory usage over time and identify leaks
vmstat 1 10
2. check for zombie processes that might be consuming resources
ps aux | grep Z

Step 5: Identify External Factors
1. Test upstream service responses
curl -I http://upstream-service
2. check for a high number of connections
sudo netstat -tnp
3. implement rate limiting or IP blocking if abuse is detected

Step 6: Recovery Implementation
1. Scale up the VM or add more load balancers
2. Implement rate limiting or use a CDN to offload traffic

Step 7: System Level Implementation
1. reboot the VM to clear memory.
2. update the kernel or system packages.