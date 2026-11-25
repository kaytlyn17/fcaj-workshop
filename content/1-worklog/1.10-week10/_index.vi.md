---
title: "1.10. Worklog Tuần 10"

weight: 10
chapter: false
---

16/11/2026 - 20/11/2026

## Mục tiêu

Đóng gói ứng dụng bằng Docker, triển khai với ECS Fargate hoặc EKS và xây dựng pipeline CI/CD cho container.

## Công việc theo ngày

| Ngày | Công việc | Bắt đầu | Hoàn thành | Tài liệu lab |
|---|---|---|---|---|
| Thứ hai | Đóng gói ứng dụng bằng Docker/Compose, đẩy image lên ECR, triển khai ECS cluster, task, service và ALB. | 16/11/2026 | 16/11/2026 | [000015](https://000015.awsstudygroup.com/) · [000016](https://000016.awsstudygroup.com/) |
| Thứ ba | Chuyển Mythical Mysfits sang microservice trên Fargate. Dùng CDK v2 cho ứng dụng Spring Boot. | 17/11/2026 | 17/11/2026 | [000067](https://000067.awsstudygroup.com/) · [000118](https://000118.awsstudygroup.com/) |
| Thứ tư | Xây CI/CD cho ECS bằng GitHub Actions, CodeBuild, Container Insights và FireLens. Thử CodePipeline, CodeDeploy cho EC2. | 18/11/2026 | 18/11/2026 | [000017](https://000017.awsstudygroup.com/) · [000023](https://000023.awsstudygroup.com/) |
| Thứ năm | Tạo EKS bằng eksctl, triển khai microservice với Helm, kiểm tra liveness/readiness, tiếp tục với pipeline EKS. | 19/11/2026 | 19/11/2026 | [000126](https://000126.awsstudygroup.com/) · [000152](https://000152.awsstudygroup.com/) |
| Thứ sáu | Thử EKS Blueprints, CI/CD cho EKS và tìm hiểu ROSA. So sánh ECS, EKS, Lightsail Containers, Fargate, dọn cluster. | 20/11/2026 | 20/11/2026 | [000065](https://000065.awsstudygroup.com/) · [000062](https://000062.awsstudygroup.com/) · [000071](https://000071.awsstudygroup.com/) |

## Kết quả trong tuần

Đóng gói ứng dụng và chạy trên ECS Fargate, triển khai ứng dụng lên EKS, thiết lập CI/CD. So sánh các lựa chọn container và dọn cụm EKS/ROSA sau lab.
