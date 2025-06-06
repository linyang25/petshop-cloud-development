# Cloud Deployment Code Snippets

This document contains real commands and configurations used in our Pet Shop CRM system deployment on AWS.

---

## 1. EC2 Instance Setup and Backend Deployment

```
# Install Java 17 on Amazon Linux
sudo dnf install java-17-amazon-corretto -y
java -version
```

```
# Upload Spring Boot JAR to EC2 instance
scp -i "petshop_key.pem" target/crm-backend-0.0.1-SNAPSHOT.jar ec2-user@<EC2-PUBLIC-IP>:~
```

```
# Run the JAR file in the background
nohup java -jar crm-backend-0.0.1-SNAPSHOT.jar > app.log 2>&1 &
```

```
# View backend logs
tail -f app.log
```

```
# Find and terminate backend process (if needed)
ps aux | grep java
kill -9 <PID>
```

---

## 2. SSH Configuration for VS Code Remote Access

```
# File: ~/.ssh/config

Host petshop_crm
   HostName 13.236.94.52
   User ec2-user
   IdentityFile C:/Users/your_name/Desktop/petshop_key.pem
```

---

## 3. S3 Bucket Policy – Frontend Hosting

```json
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::petshop-crm-frontend-group26/*"
}
```

---

## 4. S3 Bucket Policy – Pet Image Storage

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::petshop-crm-pet-images-group26/*"
    }
  ]
}
```

---

## 5. IAM Policy – S3 Upload and SES Access

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPetPhotoUploadOnly",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::petshop-crm-pet-images-group26/pet-images/*"
    },
    {
      "Sid": "AllowSESSendEmailOnly",
      "Effect": "Allow",
      "Action": [
        "ses:SendEmail",
        "ses:SendRawEmail"
      ],
      "Resource": "arn:aws:ses:ap-southeast-2:501446828955:identity/*"
    }
  ]
}
```

---

## 6. MySQL RDS Connection Test (Manual)

Connection was tested via public endpoint using tools like MySQL Workbench.

Example:
```
Host: petshop-crm-db.cti28i66oq36.ap-southeast-2.rds.amazonaws.com  
Port: 3306  
User: admin  
Password: [your-password]
```

---

These commands and configurations represent the actual deployment steps we completed, based on AWS EC2, S3, RDS, IAM, and SES services.
