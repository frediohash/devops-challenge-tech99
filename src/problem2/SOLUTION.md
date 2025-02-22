
Overview Diagram of Services

The architecture is a multi-region, microservices-based system designed for high availability, scalability, and real-time processing. Below is a breakdown of the services and their roles:

DevOps Pipeline:
AWS CodeCommit: Stores and version-controls the application source code.
AWS CodeBuild: Compiles source code, runs tests, and builds Docker images.
Amazon ECR (Elastic Container Registry): Stores Docker images for deployment.
Amazon EKS (Kubernetes): Deploys containerized microservices.

Networking & Security:
VPC with Multi-AZ Subnets: Ensures high availability and isolates resources.
AWS Global Accelerator: Improves latency and ensures failover across regions.
AWS WAF & Shield: Protects against DDoS and other threats.
PrivateLink & API Gateway: Secures API access for microservices.

Compute & Scaling:
Amazon EKS (Kubernetes): Runs containerized trading microservices.
Auto Scaling Groups (ASG): Dynamically scales EC2 instances (Spot & On-Demand).
Lambda: Executes event-driven tasks like WebSocket broadcasting.

Data Storage & Caching:
Amazon Aurora PostgreSQL: Stores trade transactions and balances.
Amazon ElastiCache (Redis): Caches frequently accessed order book data.
S3 + Athena: Stores logs for auditing and trade history analytics.

Real-Time Event Processing:
Kafka (MSK): Manages event-driven trade execution and order book updates.
Lambda: Publishes price changes to clients via WebSockets.

Frontend & User Interaction:
WebSockets: Enables real-time communication for price updates and trade execution.
API Gateway: Handles client requests and integrates with Lambda for real-time trading.

=======================================================================================================================================================================================
=======================================================================================================================================================================================
Why Each Cloud Service is Used

DevOps Pipeline:
AWS CodeCommit: Provides a secure, scalable Git repository for version control.
AWS CodeBuild: Automates the build and testing process, ensuring consistent builds.
Amazon ECR: Stores Docker images securely and integrates seamlessly with EKS for deployment.

Alternatives Considered:
GitHub/GitLab: CodeCommit is preferred for its tight integration with AWS services.
Jenkins: CodeBuild is a fully managed alternative to Jenkins, reducing operational overhead.
Docker Hub: ECR is preferred for its integration with AWS services and security features.

=======================================================================================================================================================================================

Networking & Security:
VPC: Isolates resources and ensures secure communication.
AWS Global Accelerator: Ensures low latency and failover across regions.
AWS WAF & Shield: Provides robust protection against threats.
PrivateLink & API Gateway: Ensures secure and scalable API access.

Alternatives Considered:
Cloudflare: For WAF and CDN, but AWS-native services ensure better integration.

=======================================================================================================================================================================================

Compute & Scaling:
Amazon EKS: Preferred for flexibility and scalability in running microservices.
AWS Fargate: Simplifies running batch jobs without managing servers.
Auto Scaling Groups: Ensures cost-effective scaling with Spot and On-Demand instances.
Lambda: Ideal for event-driven tasks but limited for stateful workloads.

Alternatives Considered:
ECS: EKS is preferred for its flexibility and scalability.
Lambda: Good for event-driven tasks but not suitable for stateful workloads.

=======================================================================================================================================================================================

Data Storage & Caching:
Aurora PostgreSQL: Provides better scalability and replication compared to RDS.
ElastiCache (Redis): Provides low-latency caching for frequently accessed data.
S3 + Athena: Enables cost-effective log storage and analytics.

Alternatives Considered:
RDS PostgreSQL: Aurora provides better scalability and replication.

=======================================================================================================================================================================================

Real-Time Event Processing:
Kafka (MSK): Provides better event ordering and throughput compared to SNS/SQS.

Alternatives Considered:
SNS/SQS: Kafka is preferred for its event ordering and throughput.

=======================================================================================================================================================================================

Frontend & User Interaction:
WebSockets: Enables real-time communication for price updates and trade execution.
API Gateway: Handles client requests and integrates with Lambda for real-time trading.

=======================================================================================================================================================================================
=======================================================================================================================================================================================
Scaling Plan for Growth

Traffic Increase:
Auto Scale EKS: Dynamically scale Kubernetes pods based on traffic.
API Gateway Throttling: Manage traffic spikes with rate limiting.
Global Accelerator: Ensure low-latency access across regions.

Database Load:
Aurora Read Replicas: Distribute read traffic across replicas.
DynamoDB On-Demand Mode: Automatically scale based on demand.

Event Volume:
MSK Partitions: Increase Kafka partitions dynamically to handle higher event volumes.

Regional Expansion:
Multi-Region Setup: Deploy resources in multiple regions.
Route 53 Latency-Based Routing: Route traffic to the nearest region for low latency.