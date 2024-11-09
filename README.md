# Telegram Bot with MySQL - Kubernetes Deployment

This repository contains Kubernetes manifests for deploying a Telegram bot application with a MySQL database backend. The bot interacts with a MySQL database to store and manage data, and exposes an HTTP API through a NodePort service.

## Components

1. **MySQL Deployment**: Deploys a MySQL database that stores data for the Telegram bot.
2. **Telegram Bot Deployment**: Deploys the Telegram bot, which connects to the MySQL database.
3. **ConfigMap**: Stores configuration values such as the database name, MySQL credentials, and host.
4. **Secret**: Stores sensitive information such as the MySQL root password, MySQL user password, and Telegram bot token.
5. **Services**: Exposes the MySQL database and Telegram bot to allow communication between the bot and the database, as well as access to the bot's HTTP API.

## Prerequisites

- Kubernetes cluster (local or cloud)
- `kubectl` command-line tool installed and configured
- Docker images available (e.g., `freshr/telegram-bot` for the Telegram bot, `mysql` for MySQL)

## Deployment

### Step 1: Create the Kubernetes Resources

The deployment consists of the following resources:

Pre step: you will need to upload your docker image with spring project into your docker hub, check documentation and my git repository with project: https://github.com/DanyloObukhovskyi/TelegramCurrencyRatesBot

1. **MySQL Deployment**: A single replica of MySQL, with environment variables sourced from a `ConfigMap` and `Secret` for credentials.
2. **Telegram Bot Deployment**: A single replica of the Telegram bot that connects to the MySQL database using the credentials from the `ConfigMap` and `Secret`.
3. **MySQL Service**: Exposes the MySQL database to the bot.
4. **Telegram Bot Service**: Exposes the Telegram bot via a NodePort service for external access.

Run the following command to apply the Kubernetes manifests:

```bash
kubectl create secret docker-registry regcred 
	--docker-server=https://index.docker.io/v1/ 
	--docker-username=your_username 
	--docker-password=your_password 
	--docker-email=your_email
kubectl apply -f mysql-deployment.yaml
kubectl apply -f telegrambot-deployment.yaml
kubectl apply -f telegrambot-configmap.yaml
kubectl apply -f telegrambot-secret.yaml
```

### Step 2: Configuring the Secrets and ConfigMap

- `Secret` (telegrambot-secret.yaml): Contains sensitive data like MySQL root password, MySQL user password, and Telegram bot token.
- `ConfigMap` (telegrambot-configmap.yaml): Contains the configuration for the MySQL database, username, and hostname for the Telegram bot.

Ensure that these configurations are updated to match your environment, particularly:

- mysql-root-password: The root password for the MySQL database.
- mysql-user-password: The password for the MySQL user.
- telegram-token: The token for your Telegram bot.

To encode the secrets in base64, you can use the following command:

```bash
echo -n 'your-value' | base64
```
### Step 3: Accessing the Telegram Bot

After deploying the resources, you can access the Telegram bot in **Telegram Application** (Check documentation at Telegram website). The bot is exposed on port 8080 internally, and externally on port 30000.

## File Overview

- **mysql-deployment.yaml**: Deployment for the MySQL database and Service.
- **telegrambot-deployment.yaml**: Deployment for the Telegram bot and Service.
- **telegrambot-configmap.yaml**: ConfigMap containing MySQL-related environment variables.
- **telegrambot-secret.yaml**: Secret containing MySQL credentials and Telegram bot token.

### Troubleshooting

Pods not starting: Check the logs of the MySQL and Telegram bot pods for errors:

```bash
kubectl logs <pod-name>
```

***Database connection issues***: Ensure the MySQL service is running and the Telegram bot can access it by checking the Kubernetes service configurations.

***Invalid Telegram bot token***: If the bot does not work as expected, ensure that the Telegram bot token in the secret is correct.

### Contributing

Feel free to submit issues and pull requests to improve the deployment or add new features.

### License

This project is licensed under the MIT License - see the LICENSE file for details.

### Notes:

- The `README.md` file is designed to be informative and help users understand how to deploy and manage the application using Kubernetes.