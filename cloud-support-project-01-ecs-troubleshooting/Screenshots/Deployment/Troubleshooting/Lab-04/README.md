# Lab 04 – ECS Image Pull Failure (CannotPullContainerError)

## Objective

Simulate an Amazon ECS deployment failure caused by referencing a non-existent Docker image tag in Amazon ECR and restore the service.

---

## Scenario

The ECS task definition was intentionally updated to use an invalid image tag (`v99`) that did not exist in the Amazon ECR repository.

---

## Impact

- ECS tasks failed to start
- Deployment remained in progress
- Service could not launch new tasks
- Existing task continued serving traffic until rollback/recovery

---

## Root Cause

The task definition referenced a Docker image tag that was not available in Amazon ECR, resulting in a **CannotPullContainerError** during deployment.

---

## Resolution

- Updated the task definition to use a valid image tag (`v3`)
- Created a new task definition revision
- Forced a new ECS deployment
- Verified successful deployment and healthy application

---

## Screenshots

### 1. Healthy Service Before Deployment

![Healthy Service](01-service-healthy-before-test.jpeg)

---

### 2. Current Task Definition

![Task Definition](02-current-task-definition.jpeg)

---

### 3. Failed Deployment

![Deployment Failed](03-deployment-failed.jpeg)

---

### 4. CannotPullContainerError

![Cannot Pull Container Error](04-cannot-pull-container-error.jpeg)

---

### 5. Incorrect Image Tag

![Incorrect Image Tag](05-incorrect-image-tag.jpeg)

---

### 6. Corrected Task Definition

![Corrected Task Definition](06-corrected-task-definition.jpeg)

---

### 7. Successful Deployment

![Deployment Successful](07-deployment-successful.jpeg)

---

### 8. Running ECS Task

![Running Task](08-running-task.jpeg)

---

### 9. Application Restored

![Application Restored](09-application-restored.jpeg)

---

## Result

Successfully diagnosed and resolved an ECS deployment failure by correcting the Docker image tag in the task definition, allowing the service to pull the image from Amazon ECR and restore application availability.
