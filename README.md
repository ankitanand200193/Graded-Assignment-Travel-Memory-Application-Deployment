# 🌍 TravelMemory MERN Deployment

A full-stack travel memory logging application built using the **MERN stack** and deployed on **Amazon EC2**, complete with **load balancing** and **custom domain setup via Cloudflare**.

---

## 🚀 Project Overview

The **TravelMemory** application enables users to create and share travel memories with photos, descriptions, and locations. This project demonstrates end-to-end deployment and cloud scalability practices.

🔗 **GitHub Repository:**  
[https://github.com/UnpredictablePrashant/TravelMemory](https://github.com/UnpredictablePrashant/TravelMemory)

---

## 🎯 Objectives

- Deploy the Node.js backend on EC2.
- Configure the React frontend.
- Enable frontend-backend communication.
- Scale using multiple EC2 instances and load balancer.
- Connect a custom domain with Cloudflare.

---

## ✅ Tasks & Setup Guide

### 1. ⚙️ Backend Configuration

#### 📅 Clone and Navigate
```bash
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
cd TravelMemory/backend
```

#### 🔧 Install Dependencies
```bash
npm install
```

#### 🛠️ Setup `.env` file
Create a `.env` file in the `backend/` folder:
```env
PORT=3001
MONGO_URI=your_mongodb_connection_string
```

#### 🂀 Run with PM2
```bash
npm install -g pm2
pm2 start index.js --name travel-backend
pm2 save
```

#### 🌐 NGINX Reverse Proxy
Edit `/etc/nginx/sites-available/default`:
```nginx
server {
    listen 80;
    
    location /api {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Restart NGINX:
```bash
sudo systemctl restart nginx
```

---

### 2. 🖼️ Frontend Configuration & Backend Connection

#### 📁 Navigate & Install
```bash
cd ../frontend
npm install
```

#### 🔗 Update `urls.js`
Edit `src/api/urls.js` to point to your backend:
```javascript
// src/api/urls.js
const BASE_URL = "http://your-domain.com/api";
export default BASE_URL;
```

#### 🛠️ Build and Serve Frontend
```bash
npm run build
npm install -g serve
serve -s build -l 80
```

*Or use NGINX to serve the frontend:*
```nginx
server {
    listen 80;
    root /path-to-frontend/build;
    index index.html index.htm;

    location / {
        try_files $uri /index.html;
    }
}
```

---

### 3. 📈 Scale with Load Balancer (AWS)

- Launch multiple **EC2 instances** with the same setup.
- Create an **Application Load Balancer (ALB)**.
- Register your frontend/backends under **Target Groups**.
- Configure **Listeners** for HTTP (and HTTPS via SSL if needed).

---

### 4. 🌐 Domain Setup with Cloudflare

#### 🔗 Steps:
1. Go to [Cloudflare](https://cloudflare.com) and add your domain.
2. **Update Nameservers** in your domain registrar to Cloudflare's.
3. In Cloudflare DNS settings:
   - Create **CNAME** for `www` pointing to the **load balancer DNS**.
   - Create **A record** pointing to the **frontend EC2 IP** (optional if using ALB).
4. Enable **Proxy** (orange cloud) for better performance and security.

---

## 📸 Final Deployment Checklist

- ✅ Backend running on EC2 with PM2 and NGINX.
- ✅ Frontend served via NGINX or `serve`.
- ✅ Communication wired via `urls.js`.
- ✅ Load balancing configured with ALB.
- ✅ Domain connected through Cloudflare DNS.

---

## 🙌 Contributing

Feel free to fork the repo, raise issues, or create PRs to enhance the deployment or add features.

---

## 📜 License

MIT License © [UnpredictablePrashant](https://github.com/UnpredictablePrashant)

