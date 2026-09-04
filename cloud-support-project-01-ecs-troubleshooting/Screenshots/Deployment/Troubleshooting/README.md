
# ECS Troubleshooting Labs

This directory contains real-world troubleshooting scenarios reproduced on Amazon ECS.

The objective of these labs was not only to deploy an application but also to intentionally introduce production-like failures, investigate the symptoms, identify the root cause, and restore the service.

---

# Lab 1 – Application Load Balancer Health Check Failure

## Scenario

The Application Load Balancer health check path was intentionally changed from:

```
/
```

to

```
/health
```

Although the application did not expose a `/health` endpoint, the ALB continued performing health checks against that path.

---

## Evidence

### Step 1 – Misconfigured Health Check

![Health Check](lab-01/01-health-check-misconfigured.jpeg)

---

### Step 2 – Target Became Unhealthy

![Target Unhealthy](lab-01/02-target-unhealthy.png)

AWS reported:

```
Health checks failed with these codes: [404]
```

---

### Root Cause

The application did not expose the `/health` endpoint.

Every ALB health check returned HTTP 404, causing the target to transition to the **Unhealthy** state.

---

### Resolution

The health check path was restored to:

```
/
```

After several successful health checks, the target returned to the **Healthy** state.

---

### Recovery

![Recovered Target](lab-01/03-target-recovered.png)

---

# Lab 2 – ECS Container Port Misconfiguration

## Scenario

A new task definition revision was created.

The container port was intentionally changed from:

```
80
```

to

```
8080
```

while the ECS Service and Application Load Balancer were still configured to route traffic to port **80**.

---

### Step 1 – Modified Task Definition

![Task Definition](lab-02/01-container-port-8080.png)

---

### Step 2 – Deployment Failure

![Deployment Error](lab-02/02-port-mapping-error.png)

AWS returned:

```
The container lms-container did not have a container port 80 defined.
```

---

### Root Cause

The ECS Service expected the task definition to expose container port **80**.

However, the updated task definition exposed only port **8080**, creating a mismatch between the ECS Service, Target Group, and Task Definition.

AWS prevented the deployment before starting the task.

---

### Resolution

The task definition was updated to expose the expected container port, restoring compatibility with the ECS Service configuration.

---

# Skills Demonstrated

- Amazon ECS
- Amazon ECR
- Application Load Balancer
- Target Groups
- Docker
- ECS Task Definitions
- ECS Service Deployments
- Health Check Troubleshooting
- Container Port Mapping
- Root Cause Analysis
- Production Incident Investigation
