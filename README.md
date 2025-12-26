# Pretix Docker-Compose setup
The repository includes a [Pretix](https://pretix.eu/about/de/) docker-compose configuration for local development and production deployment via Coolify.

## Coolify Deployment

This repository is configured for easy deployment on Coolify. Follow these steps:

### 1. Prerequisites
- Ensure your DNS A record is configured:
  - `pretix.weinerlebnistouren-pfalz.de` → `31.97.39.187`
- Have a Coolify instance with the `coolify` network available
- Have a Gmail account with an App Password for sending emails
  - **Important**: Create a Gmail App Password at [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
  - Regular Gmail passwords will NOT work due to security restrictions

### 2. Deployment Steps

1. **Add the repository in Coolify**:
   - Go to your Coolify dashboard
   - Create a new resource → Docker Compose
   - Paste the GitHub repository URL

2. **Configure environment variables** in Coolify:

   **Required variables:**
   - `PRETIX_DOMAIN` = `pretix.weinerlebnistouren-pfalz.de`
   - `POSTGRES_PASSWORD` = (generate a strong random password)
   - `MAIL_FROM` = `noreply@weinerlebnistouren-pfalz.de`
   - `MAIL_HOST` = `smtp.gmail.com`
   - `MAIL_USER` = `your-email@gmail.com` (your Gmail address)
   - `MAIL_PASSWORD` = (your Gmail App Password - NOT your regular Gmail password)

   **Optional variables** (see `.env.example` for full list):
   - `PRETIX_INSTANCE_NAME` = Custom instance name
   - `PRETIX_CURRENCY` = EUR (default)
   - `PRETIX_LOCALE` = de (default)

3. **Deploy**: Coolify will handle SSL certificates via Let's Encrypt and Traefik automatically.

### 3. Initial Setup

After the first deployment, access your Pretix instance at `https://pretix.weinerlebnistouren-pfalz.de` and complete the setup wizard.

## Local Development

You can execute `docker-compose up -d --build --force-recreate` to start and build all related containers.

For local development, copy `.env.example` to `.env` and configure your local environment variables.

### Version information

| **Version** |                         **Description**                          |
|:-----------:|:----------------------------------------------------------------:|
|    1.2.0    |                      Includes PostgreSQL 17                      |
|    1.1.1    | Update the Alpine version and the allocated IPs of the databases |
|    1.1.0    |                      Includes PostgreSQL 16                      |
|    1.0.0    |                      Includes PostgreSQL 13                      |

### Cronjobs

It is possible to adapt the `pretixuser` crontab entries by modifying the [crontab](docker/pretix/crontab) file.

## TLS setup

You can specify the used TLS certificates by adapting the mounted [certificate](docker/pretix/files/config/ssl/domain.crt) and [key](docker/pretix/files/config/ssl/domain.key) e.g. from Let's Encrypt or generating new self-signed certificates by following the [manual](scripts/EXAMPLE-CERT-CREATION.md) and moving the generated files. It is also possible to adapt the [used](docker/pretix/nginx/nginx.conf) Nginx configuration. 

## Contribution
If you would like to contribute something, have an improvement request, or want to make a change inside the code, please open a pull request.

## Support
If you need support, or you encounter a bug, please don't hesitate to open an issue.

## Donations
If you want to support my work, I ask you to take an unusual action inside the open source community. Donate the money to a non-profit organization like Doctors Without Borders or the Children's Cancer Aid. I will continue to build tools because I like them, and I am passionate about developing and sharing applications.

## License
This product is available under the Apache 2.0 license.
