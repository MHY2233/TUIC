# TUIC部署
## 1.安装证书
```bash
apt install certbot

certbot certonly -d yourdomain.com   --standalone

 - Congratulations! Your certificate and chain have been saved at:
  /etc/letsencrypt/live/yourdomain.com/fullchain.pem
  Your key file has been saved at:
  /etc/letsencrypt/live/yourdomain.com/privkey.pem
  Your certificate will expire on 2023-07-15. To obtain a new or
  tweaked version of this certificate in the future, simply run
  certbot again. To non-interactively renew *all* of your
  certificates, run "certbot renew"
```
