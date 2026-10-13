# AWS-trusted-advisor-monitoring
An automated, event-driven notification network leveraging AWS Trusted Advisor and EventBridge to capture infrastructure cost and security anomalies at zero cost
# AWS Trusted Advisor Monitoring Network

An automated, event-driven notification network leveraging **AWS Trusted Advisor** and **Amazon EventBridge** to capture infrastructure cost and security anomalies at zero cost. This project is engineered to be **100% Free-Tier compatible**, eliminating the need for paid AWS support plans.

##  The Business Case & Problem Statement

### The Problem
During early experimentation with spinning up test cloud resources, a lack of active infrastructure monitoring led to an unmonitored instance, unexpected overspending, and the eventual loss of a sandbox account. 

In a production enterprise environment, this lack of visibility is a catastrophic operational risk. Organizations frequently miss critical infrastructure security vulnerabilities and ballooning operational costs because manual account health checks happen too infrequently or depend on costly premium support plans.

### The Solution
To solve this, I designed and built a programmatic, cost-conscious safety net. By replacing periodic manual audits with an event-driven notification topology, the system instantly identifies infrastructure changes and blasts real-time alerts directly to an operations inbox. 

### Financial Constraints
* **Cost Target:** \$0.00 (100% Free-Tier Compatible)
* **Operational Constraint:** Must operate without requiring a paid AWS Business or Enterprise Support plan.

---

##  System Architecture

The layout relies on a decoupled, real-time alert pipeline using official AWS blocks:

[Project Architecture](architecture.png)

1. **The Guard (AWS Trusted Advisor):** Scans the AWS account automatically for foundational security optimizations and cost-saving metrics.
2. **The Wire (Amazon EventBridge):** An event rule filters and catches status changes from Trusted Advisor instantly at zero cost.
3. **The Loudspeaker (Amazon SNS):** Relays the structured payload and broadcasts an alert to a verified operations email box.
4. **Topology Choice:** Deliberately chose an event-driven EventBridge routing topology over CloudWatch metric alarms to bypass corporate AWS Support Plan requirements, ensuring a production-ready notification network that is 100% Free-Tier compatible.
<img width="2257" height="770" alt="image" src="https://github.com/user-attachments/assets/b7faed26-0b1c-407c-912d-913e059e6cd1" />

---

##  Deployment Guide

### Phase 1: Manual Verification (AWS Console)
1. **Simple Notification Service (SNS):** Created a Standard Topic named `my-first-cloud-alarm` and established an Email protocol subscription.
2. **Subscription Activation:** Verified the endpoint by confirming the official AWS registration link inside the operations inbox.
3. **Amazon EventBridge Configuration:** Deployed a rule named `my-trusted-advisor-wire` utilizing a custom JSON pattern targeting `"source": ["aws.trustedadvisor"]`.
4. **Target Routing:** Bound the rule destination directly to the `my-first-cloud-alarm` SNS topic.

### Phase 2: Infrastructure Automation (CLI)
To move away from point-and-click console configuration, the setup is codified into a repeatable deployment script.

```bash
# Execute the automated environment deployment
bash deploy.sh
```

---

##  Automation Code (`deploy.sh`)

The infrastructure can be initialized programmatically via the AWS CLI. The configuration script handles topic creation, subscription parameters, and event routing rules:

```bash
#!/bin/bash
# 1. Create the SNS alerting topic
TOPIC_ARN=\$(aws sns create-topic --name my-first-cloud-alarm --query 'TopicArn' --output text)

# 2. Subscribe the operational email endpoint
aws sns subscribe --topic-arn "\$TOPIC_ARN" --protocol email --notification-endpoint "your-operations-email@example.com"

# 3. Create the EventBridge filtering rule
aws events put-rule --name "my-trusted-advisor-wire" --event-pattern '{"source": ["aws.trustedadvisor"]}'

# 4. Route the EventBridge rule target to the SNS Topic
aws events put-targets --rule "my-trusted-advisor-wire" --targets "Id"="1","Arn"="\$TOPIC_ARN"
```

---

##  Core Competencies Demonstrated
* **Technical Product Management:** Translating real-world cloud spend failures into highly controlled, cost-bounded system requirements.
* **Event-Driven Architecture:** Utilizing decoupled cloud primitives (EventBridge, SNS) to reduce system overhead.
* **Infrastructure Automation:** Transitioning manual GUI-based console actions into repeatable shell scripts.

