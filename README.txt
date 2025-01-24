HOW TO POINT A SUBDOMAIN TO PYTHON FLASK NGINX
FOLLOW STEP BY STEP 


1st STEP 

OPEN CLOUDFLARE THEN GO TO YOUR DOMAIN 
THEN GO TO DNS SETTING AND CREATE RECORD

TYPE "A"
NAME "@"
IPV4 ADDRESS "<VPS PUBLIC IP ADDRESS>"
PROXY STATUS "ON"
TTL "Auto"





2nd STEP

OPEN CLOUDFLARE AND GO TO YOUR DOMAIN
THEN GO TO SSL/TLS SETTING
THEN SWITCH ENCRYPTION MODE TO "Full"





3rd STEP

LOGIN YOUR UBUNTU VPS AND RUN BELOW COMMANDS

"sudo apt update
sudo apt install nginx"

"sudo systemctl start nginx"

"sudo systemctl enable nginx"

"sudo systemctl status nginx"

TO CONFIRM NGINX INSTALLED, GO TO "http://<your vps ip>"





4th STEP

G0 TO YOUR PYTHON SCRIPT AND IMPORT BELOW PAKEAGE 

"import os"

THEN REPLACE YOUR CODE TO BELOW CODE, FOR RUN YOUR APPLICATION ON YOUR CHOOSED PORT

"if __name__ == '__main__':
    port = int(os.environ.get('PORT', <PORT>))
    app.run(host='0.0.0.0', port=port, debug=True)"

REPLACE <PORT> TO YOUR SPECIFIC PORT





5th STEP

RUN YOUR PYTHON FLASK WITH NOHUP

"nohup python3 app.py &"

THEN YOUR APPLICATION HOSTED ON YOUR SPECIFIC PORT
'http://<your vps public ip>:<your specific port>"





6th STEP

FOR HOST YOUR PYTHON APPLICATION ON YOUR SUBDOMAIN

RUN BELOW COMMANDS

"sudo nano /etc/nginx/sites-available/<YOUR SUBDOMAIN WITHOUT http/https>"

AFTER RUN THIS COMMAND, YOUR EDITOR WILL OPEN, SO PASTE BELOW CODE IN EDITOR

"server {
    server_name <YOUR SUBDOMAIN WITHOUT http/https>;

    location / {
        proxy_pass http://127.0.0.1:<your specific port>;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    listen 80;
}"

AFTER PASTE THIS CODE IN EDITOR, THEN SAVE IT
HOW TO SAVE? Below instruction

ctrl+x
Then Press "Y" in your keyboard
Then Press Enter in your keyboard





7th STEP

THEN ENABLE YOUR NGINX BY BELOW CODE

"sudo ln -s /etc/nginx/sites-available/<YOUR SUBDOMAIN WITHOUT http/https> /etc/nginx/sites-enabled/"

THEN REMOVE OLD CONFIGRATION BY RUN THIS COMMAND

"sudo rm /etc/nginx/sites-enabled/default"

THEN RELOAD YOUR NGINX BY RUN BELOW COMMANDS

"sudo systemctl reload nginx"





8th STEP

SSL INSTALL IN YOUR SUBDOMAIN BY RUN BELOW COMMANDS

"sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d <YOUR SUBDOMAIN WITHOUT http/https>"

TO CHECK, SUBDOMAIN SSL RENEWABLE OR NOT, RUN BELOW COMMAND

"sudo certbot renew --dry-run"

TO RENEW YOUR SUBDOMAIN SSL, RUN BELOW COMMAND

"sudo certbot renew"





9th STEP

IF YOU FOLLOW ALL STEPS CAREFULLY
THEN BOOOOOM!  YOUR PYTHON APPLICATION RUN ON YOUR SUBDOMAIN

https://<yoursubdomain>"





PRO TIP

FOR GUNICORN SETUP, RUN BELOW COMMANDS

"pip install gunicorn"

THEN RUN YOUR PYTHON FILE BY RUN THIS COMMANDS

"nohup gunicorn -w 4 -b 0.0.0.0:<YOUR SPECIFIC PORT> app:app &"





2 PRO TIP

HOW TO REMOVE SUBDOMAIN, FOLLOW BELOW COMMAND, RUN BOTH COMMAND

"sudo rm /etc/nginx/sites-available/<YOUR SUBDOMAIN WITHOUT http/https>"
"sudo rm /etc/nginx/sites-enabled/<YOUR SUBDOMAIN WITHOUT http/https>"





3 PRO TIP

IF YOU RUN ANY PLAYWRIGHT FLASK SCRIPT
THEN RUN WITH BELOW COMMAND

"nohup gunicorn -w 4 --bind 0.0.0.0:<PORT> --timeout 120 app:app > output.log 2>&1 &"





Traffic Allow By below command (Optional) 

sudo ufw allow 'Nginx Full'

To enable UFW, Run Below Command

sudo ufw enable

To check UFW Status run Below command

sudo ufw status






TEXT TUTORIAL WRITED BY
@VZR7X MATRIX DEVELOPER 2024-25
