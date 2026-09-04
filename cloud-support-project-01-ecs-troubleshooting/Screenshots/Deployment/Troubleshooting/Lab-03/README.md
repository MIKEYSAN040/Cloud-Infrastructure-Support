
# Lab 03 – ECS Container Startup Failure Investigation

## Objective

This lab demonstrates how to troubleshoot an Amazon ECS task that repeatedly stops because the application inside the container fails during startup.

---

## Technologies Used

- Amazon ECS (Fargate)
- Amazon ECR
- Application Load Balancer
- Amazon CloudWatch Logs
- Docker
- Nginx

---

## Scenario

A new Docker image was deployed with an intentionally invalid `nginx.conf` configuration.

Although the deployment started successfully, the application failed during startup, causing Amazon ECS to stop the task automatically.

---

## Problem Introduced

The terminating semicolon was removed from the `root` directive inside `nginx.conf`.

Incorrect configuration:

```nginx
root /usr/share/nginx/html
```

Correct configuration:

```nginx
root /usr/share/nginx/html;
```

---

## Symptoms

- ECS deployment started successfully.
- New task launched.
- Container exited immediately.
- ECS reported:

```
Stopped | Essential container in task exited
```

---

## Investigation

CloudWatch Logs showed:

```
2026/09/04 09:51:58 [emerg] 1#1: invalid number of arguments in "root" directive in /etc/nginx/nginx.conf:10

nginx: [emerg] invalid number of arguments in "root" directive in /etc/nginx/nginx.conf:10
```

The logs confirmed that Nginx could not start because of an invalid configuration.

---

## Root Cause

An invalid Nginx configuration prevented the web server from starting.

Since Nginx is the main process inside the container, the container exited immediately, causing ECS to stop the task.

---

## Resolution

1. Fixed the Nginx configuration.
2. Rebuilt the Docker image.
3. Pushed the image to Amazon ECR.
4. Created a new ECS task definition revision.
5. Updated the ECS service.
6. Verified successful deployment.

---

## Lessons Learned

- CloudWatch Logs are essential for diagnosing startup failures.
- ECS automatically replaces failed tasks.
- Image deployment success does not guarantee application startup success.

---

# Screenshots

## 1. Healthy Service

![Health Services](Lab-03/01-service-healthy-before-test.jpg)
---

## 2. Updated Task Definition

![Updated Task](Lab-03/02-task-definition-v2.jpeg)
---

## 3. Deployment Started

![Deployment](Lab-03/03-service-deploying.png)
---

## 4. Task Failed

![Task Failed](Lab-03/04-task-stopped.png)
---

## 5. CloudWatch Investigation

![CloudWatch Investigation](Lab-03/05-cloudwatch-error.png)
---

## 6. Service Recovered

![Recovery Service](Lab-03/06-service-recovered.png)
---

## Result

The container startup failure was successfully reproduced, investigated using Amazon CloudWatch Logs, and resolved by correcting the Nginx configuration and redeploying the application.
