DB SERVER
=========
sudo apt-get update
sudo apt-get install gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo apt-get install net-tools
sudo vi /etc/mongod.conf
sudo systemctl daemon-reload
sudo systemctl status mongod.service
sudo systemctl start mongod.service
sudo systemctl status mongod.service
sudo netstat -lntp

	
	
APP SERVER
==========

sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
sudo chmod 666 /var/run/docker.sock

git clone https://github.com/JAGANNATHDEVOPS/AquilaCMS.git
cd AquilaCMS/
docker build -t aquila .
docker images
docker run -d -p 3010:3010 aquila
docker ps
sudo apt-get install net-tools
sudo netstat -lntp


WEBS ERVER
==========


sudo vi /etc/nginx/sites-available/myapp.giridevops.online
server {
    server_name myapp.giridevops.online;

    location / {
        proxy_pass http://<APPLICATION_LAYER_SERVER_PRIVATEIP>:3010;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
sudo ln -s /etc/nginx/sites-available/myapp.giridevops.online /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx.service
sudo apt install snapd
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
