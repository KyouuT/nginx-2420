1. Install UFW `sudo pacman -S ufw`
2. Allow ssh and http `sudo ufw allow ssh` `sudo ufw allow http`
3. Enable ufw `sudo ufw enable`
4. Check status `sudo ufw status`
5. Create a backend binary
    - `sudo mkdir /web/html/nginx-2420/backend`
    - In the directory create a `hey` and `echo` binary
6. Transfer the backend to the server using sftp
    - sftp -i ~/.ssh/do-key juan@164.92.106.35
    - upload file using `put <file-path>`
7. Create a service to run that backend
    - sudo touch /etc/systemd/system/backend.service
    ```
    [Unit]
    Description=Backend Service

    [Service]
    Type=notify
    ExecStart=/bin/backend

    [Install]
    WantedBy=multi-user.target

8. Edit the `nginx-2420.conf` in `sites-available`
    ```
    server {
    listen 80;
    listen [::]:80;
    server_name _;
    root /web/html/nginx-2420;
    location / {
        index index.html index.htm;
    }

    location / {
            proxy_pass http://164.92.106.35:8080;
    }

    location /hey {
            proxy_pass http://164.92.106.35:8080/hey;
    }
    location /echo {
            proxy_pass http://164.92.106.35:8080/echo;
    }
}

9. Test backend using curl in host machince using `curl`
    - `curl http://164.92.106.35`
    - `curl http://164.92.106.35/hey`
    - ```curl -X POST -H "Content-Type: application/json" \
    -d '{"message": "Hello from your server"}' \
    http://164.92.106.35/backend/echo