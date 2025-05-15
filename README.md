# Nginx-reverse-proxy
The following repo includes the required nginx config, backend config and front end config

React Web Application with Node.js Backend and Nginx Reverse Proxy 

This project consists of a React frontend, a Node.js Express backend, and a MySQL database. All API requests are routed through Nginx as a reverse proxy . 
🛠️ Requirements 

    Git
    Node.js & npm
    MySQL
    Nginx

1.📦 Set Up the Environment and Install Dependencies
Clone Repositories

git clone https://github.com/kkarki7120/frontend.git 
git clone https://github.com/kkarki7120/backend.git 

Install Frontend Dependencies
cd frontend
npm install

Install Backend Dependencies
cd ../backend
npm install

2. 🔧 Configure the Backend and Connect to MySQL
cp .env.example .env
nano .env

Update with your MySQL credentials
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=react
DB_PORT=3306
PORT=4000

Create MySQL Database and Table
mysql -u root -p

Run these SQL commands:

CREATE DATABASE react;
USE react;

CREATE TABLE formData (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255),
  message TEXT
);
Exit MySQL:
exit;

Start the Backend Server
cd backend
node server.js

3. 🚀 Deploy Frontend and Backend Using Nginx as Reverse Proxy
Build the React App
cd ../frontend
npm run build

The static files will be in the build/ directory.

Configure Nginx
Create a new Nginx config file:
sudo nano /etc/nginx/sites-available/react-app

Paste this configuration:

server {
    listen 80;

    location / {
        root /home/your-username/react-app/frontend/build;
        index index.html;
        try_files $uri $uri/ =404;
    }

    location /api/ {
        proxy_pass http://localhost:4000/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

Enable the site::
sudo ln -s /etc/nginx/sites-available/react-app /etc/nginx/sites-enabled/

Test and reload Nginx:
sudo nginx -t
sudo systemctl reload nginx

Example API Request Through Nginx
curl -X POST http://localhost/api/submit \
     -H "Content-Type: application/json" \
     -d '{"name":"Test","email":"test@example.com","message":"Test message"}'
Expected response:
{"message":"Your response has been saved"}


✅ Final Directory Structure
/home/your-username/react-app/
├── frontend/
│   ├── build/          # Static files served by Nginx
│   └── ...
├── backend/
│   ├── server.js       # Express backend running on port 4000
│   └── ...

🧪 Testing
1. Open browser: http://localhost
2. Submit form
3. Verify data is stored in MySQL:
mysql -u root -p
USE react;
SELECT * FROM formData;

🛠️ Troubleshooting
If Nginx fails to start:

sudo tail -f /var/log/nginx/error.log
Ensure backend is running on port 4000.
Check .env file for correct database credentials.

✅ React app is now live at http://localhost, and all API calls go through Nginx to Node.js backend.
