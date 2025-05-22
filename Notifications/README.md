Notification System Overview

The notification service consists of three key boundary contexts working together to deliver secure, scalable communications:
Component	Purpose	Responsibilities
Notification Core	->      Orchestration Engine	->    Event validation, routing, workflow management, PII handling
Notification Vendor	->    Delivery Management -> 	    Channel delivery (email, SMS, push), vendor integration, delivery tracking
Notification Template-> 	Content Manement ->	        Template storage, personalization, content validation, localization

How They Work Together
Consumer Services → [Notification Core] → [Notification Vendor] → External Channels
                           ↓
                    [Notification Template]

Consumer services send notification requests to Notification Core
Notification Core validates requests and fetches content from Notification Template
Notification Core orchestrates delivery through Notification Vendor
Notification Vendor handles actual delivery via external channels (SendGrid, etc.)
Vendor listens and report back delivery status and engagement metrics


🎯 Focus: Notification Core BC Threat Model

This presentation focuses specifically on the Notification Core component - the critical orchestration engine that:
  1. Receives and validates notification requests from internal services
  2. Handles sensitive PII data (emails, phone numbers, contact info)
  3. Manages workflow orchestration and business rules
  4. Integrates with Template and Vendor components
  5. Processes 100K-1M notification events

Why Focus on Notification Core?
  1. Highest Risk Component: Handles the most sensitive data and integrations
  2. Central Attack Surface: Single point where most threats converge
  3. Critical Business Function: System failure impacts all notification channels
  4. Complex Data Flows: Multiple integration points requiring security analysis



🏗️ Notification Core Architecture:-
https://lucid.app/lucidchart/02ebcb92-79ff-4daa-873a-4a8e548ef68a/edit?viewport_loc=-2621%2C-807%2C6445%2C3348%2Ci61dwZ2YSURg&invitationId=inv_4477b287-a845-49e5-8045-e7f7f78dbede

AWS Serverless Components
    API Gateway + Lambda: Secure entry points with authentication
    EventBridge: Event routing between Core, Vendor, and Template services
    Step Functions: Workflow orchestration and business logic
    DynamoDB: Notification metadata and delivery status storage
    Cloudwatch: 

External Integrations
    Consumer Services: Internal services requesting notifications
    Notification Vendor BC: Delivery channel management
    Notification Template BC: Content and template management
    Document Management Platform: Attachment handling

🔗 Interactive Data Flow Diagram
https://lucid.app/lucidchart/02ebcb92-79ff-4daa-873a-4a8e548ef68a/edit?invitationId=inv_4477b287-a845-49e5-8045-e7f7f78dbede&page=xmwfD8b1wP~R#
