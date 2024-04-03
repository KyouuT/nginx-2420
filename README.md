1. First start by installing vim and nginx:
    `sudo pacman -Sy vim`
    `sudo pacman -s nginx`
2. Go to the nginx.conf file in the nginx folder:
    `cd /etc/nginx/`
    `sudo vim nginx.conf`
3. In the configuration file, inside the http block create an include line followed by the seperate .conf file that you are about to make. In my case:
    `include /home/juan/web/html/nginx-2420/nginx-2420.conf`
4. Once you added the include line, save and quit the nginx.conf file and start the nginx using systemctl to see if the nginx is working. It should show the default html that nginx provide:
    `sudo systemctl start nginx`
5. Then create the seperate conf file if you don't have one:
    `sudo touch /home/juan/web/html/nginx-2420/nginx-2420.conf`
6. Go into that folder and vim the .conf file, inside the .conf create a new server block and include the necessary line:
    `sudo vim nginx-2420.conf`
    ```
    server {
        listen      80;
        server_name 164.92.106.35;

        root /home/juan/web/html/nginx-2420;
        index nginx-2420.html;
    }
    ```
7. Save the .conf file, then create the .html file inside the same folder:
    `sudo vim nginx-2420.html`
8. Then copy and paste an html content inside and save the file.
9. Check for any error in the nginx configuration file:
    `sudo nginx -t`
10. If there's no error start the nginx:
    `sudo systemctl reload nginx`

