# AWS EKS & Istio Learning Lab 🚀

Welcome to my AWS Kubernetes learning repository! 

This repository documents my journey of learning Amazon Elastic Kubernetes Service (EKS), Terraform, and Istio Service Mesh from beginner to advanced production-grade deployments.

## Current Project: Microservices E-Commerce Application

Currently, this repository holds a production-grade deployment of the **Google Online Boutique** (an 11-tier polyglot microservices application) running on Kubernetes with **Istio**.

### 📊 Architecture & Data Flow

Below is the complete architectural flow showing how traffic moves from the AWS Load Balancer through the Istio Ingress Gateway, and securely communicates between microservices using **mTLS**.

> **Note:** GitHub automatically renders this Mermaid diagram natively!

```mermaid
graph TD
    User([User / Browser]) --> ALB([AWS Application Load Balancer])
    ALB --> Gateway[Istio Ingress Gateway]

    Gateway -->|mTLS| Frontend[Frontend + Envoy]

    Frontend -->|mTLS| AdService[AdService + Envoy]
    Frontend -->|mTLS| CartService[CartService + Envoy]
    Frontend -->|mTLS| CheckoutService[CheckoutService + Envoy]
    Frontend -->|mTLS| CurrencyService[CurrencyService + Envoy]
    Frontend -->|mTLS| ProductCatalogService[ProductCatalogService + Envoy]
    Frontend -->|mTLS| RecommendationService[RecommendationService + Envoy]
    Frontend -->|mTLS| ShippingService[ShippingService + Envoy]

    CheckoutService -->|mTLS| CartService
    CheckoutService -->|mTLS| CurrencyService
    CheckoutService -->|mTLS| EmailService[EmailService + Envoy]
    CheckoutService -->|mTLS| PaymentService[PaymentService + Envoy]
    CheckoutService -->|mTLS| ProductCatalogService
    CheckoutService -->|mTLS| ShippingService

    RecommendationService -->|mTLS| ProductCatalogService

    CartService -->|mTLS| Redis[(Redis Cache)]
```

---

## Detailed Documentation
For a full, step-by-step breakdown of how this was installed, how the data flows, and how Istio was configured via Helm in a production-grade way, please refer to the detailed lab guide:

👉 [**View the Full Lab Documentation (online_boutique_lab.md)**](./online_boutique_lab.md)
