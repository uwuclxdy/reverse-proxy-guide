# Set up reverse proxy on Debian/Ubuntu using Nginx

This is how I got reverse proxy working for me, hope it helps someone else too :3

### 1. Install Nginx

```
sudo apt update
sudo apt install nginx
```

### 2. Write a config file
Create and open the file `/etc/nginx/sites-available/reverse-proxy` using your preferred editor (in my case nano):
```
sudo nano /etc/nginx/sites-available/reverse-proxy
```
then add the following content:
```
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://yourpublicip:port/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
*Make sure to replace the placeholders (`yourdomain.com`, `yourpublicip`, `port`)*

### 3. Create a symbolic link to enable the service
```
sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
```

### 4. Check for errors in config (optional)
```
sudo nginx -t
```

### 5. Restart nginx
```
sudo systemctl reload nginx
```

### 6. Add `A` DNS record
Now just make sure you have a domain / subdomain (`yourdomain.com` placeholder above) pointing at __public IP of your reverse proxy__. Changes *might take some time* to get in effect so be patient.
