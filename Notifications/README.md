# Notification System Overview

This document outlines the architecture and components of the Notification System, with a specific focus on the **Notification Core** boundary context (BC) for threat modeling purposes.

The notification service is a critical component responsible for secure and scalable communications. It comprises three key boundary contexts:

| Component             | Primary Function      | Key Responsibilities                                                                 |
| :-------------------- | :-------------------- | :----------------------------------------------------------------------------------- |
| **Notification Core** | Orchestration Engine  | Event validation, PII handling, routing logic, workflow management, integration hub.   |
| **Notification Vendor** | Delivery Management   | Manages communication channel delivery (e.g., email, SMS, push notifications), integrates with third-party vendors, tracks delivery status and engagement. |
| **Notification Template** | Content Management    | Stores and manages notification templates, handles personalization, content validation, and localization. |

---

## How The System Components Interact

The typical flow of a notification request is as follows:

1.  **Consumer Services** initiate a notification request to the `Notification Core`.
2.  `Notification Core`:
    * Validates the incoming request.
    * Fetches the appropriate content by interacting with the `Notification Template` service.
    * Orchestrates the delivery process.
3.  `Notification Core` then routes the prepared notification to the `Notification Vendor` service.
4.  `Notification Vendor` handles the final delivery to the end-user via external channels (e.g., SendGrid, Twilio, Firebase Cloud Messaging).
5.  `Notification Vendor` also listens for and reports back delivery status updates and engagement metrics to `Notification Core` or a centralized monitoring system.

---

## 🎯 Focus: Notification Core BC Threat Model

This document and the associated threat modeling exercise concentrate on the **Notification Core** component. This central orchestration engine is responsible for:

1.  Receiving and validating all notification requests from internal consumer services.
2.  Securely handling sensitive Personally Identifiable Information (PII) such as email addresses, phone numbers, and other contact details.
3.  Managing complex workflow orchestrations and applying business-specific rules.
4.  Integrating seamlessly with the `Notification Template` and `Notification Vendor` components.
5.  Processing a significant volume of notification events (currently estimated at 100,000 to 1,000,000 events per [Specify time period, e.g., day/hour]).

### Why Focus on Notification Core?

The Notification Core is prioritized for threat modeling due to its:

* **High-Risk Profile:** It directly processes and has access to the most sensitive data (PII) and acts as a central hub for integrations.
* **Central Attack Surface:** As the primary entry and processing point for notifications, it represents a convergence point for numerous potential threats.
* **Critical Business Function:** Any failure, compromise, or significant degradation of the Notification Core would impact all notification channels and potentially disrupt critical business communications.
* **Complex Data Flows:** Its multiple integration points with other internal and external services necessitate a thorough security analysis of data in transit and at rest.

---

## 🏗️ Notification Core Architecture

The Notification Core is built using a serverless architecture on AWS, leveraging the following key components and integrations:

**AWS Serverless Components:**

* **API Gateway + AWS Lambda:** Provides secure, scalable entry points for notification requests, handling authentication and authorization before invoking backend Lambda functions for processing.
* **Amazon EventBridge:** Facilitates event-driven communication and routing between the `Notification Core`, `Notification Vendor`, and `Notification Template` services, enabling decoupled architecture.
* **AWS Step Functions:** Manages complex workflow orchestration, business logic execution, retries, and error handling within the notification processing pipeline.
* **Amazon DynamoDB:** Used for persistent storage of notification metadata, delivery status, audit logs, and potentially temporary PII storage during processing.
* **Amazon CloudWatch:** Provides comprehensive logging, monitoring, metrics, and alerting for all components of the Notification Core, crucial for operational visibility and security incident detection.

**External Integrations:**

* **Consumer Services (Internal):** Various internal applications and services that trigger notification events.
* **Notification Vendor BC:** The separate boundary context responsible for actual delivery via specific channels.
* **Notification Template BC:** The separate boundary context that supplies the content and templates for notifications.
* **Document Management Platform:** Integrated for handling and securing any attachments that may be part of a notification.

**Architectural Diagram:**

* [View Detailed Architecture on Lucidchart](https://lucid.app/lucidchart/02ebcb92-79ff-4daa-873a-4a8e548ef68a/view)

---

## 🔗 Interactive Data Flow Diagram (DFD)

A detailed Data Flow Diagram illustrating how data moves into, through, and out of the Notification Core is available for review. This is essential for understanding potential threat vectors.

* [View Interactive Data Flow Diagram on Lucidchart](https://lucid.app/lucidchart/02ebcb92-79ff-4daa-873a-4a8e548ef68a/view)

---

## 🛠️ Threat Modeling Tool: Threat Composer

This threat model will be developed and documented using Threat Composer.

* **Application Link:** [AWS Threat Composer](https://awslabs.github.io/threat-composer/workspaces/default/application)
* **Documentation:** [Working with Threat Composer from the AWS Toolkit for VS Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/threatcomposer-overview.html)

---
