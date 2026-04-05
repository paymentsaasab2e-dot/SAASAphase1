# AWS Hosting Quotation for SAASA Phase 1

Prepared on: 5 April 2026

## Project scope covered in this quotation

This quotation is for hosting the current application on AWS with the existing architecture:

- `FRONTEND1_PROD`: Next.js frontend
- `BACKEND1_PROD`: Node.js / Express backend
- Docker-based deployment
- MongoDB kept on MongoDB Atlas as currently configured
- Cloudinary, Resend, and AI providers kept as external services

This is the lowest-risk approach because the current codebase already depends on MongoDB Atlas, Cloudinary, Resend, OpenAI, Gemini, Mistral, and Anthropic. Replacing those with AWS-native services would increase both cost and implementation scope.

## Assumptions

- Region assumed for deployment: AWS Mumbai (`ap-south-1`) or nearest suitable production region
- Pricing checked against official AWS pricing pages on 5 April 2026
- Taxes are excluded
- Domain registration is excluded
- One retained snapshot per server is included in the estimate
- Route 53 hosted zone is included as an optional standard DNS cost
- Data transfer stays within the bundled Lightsail allowance
- MongoDB Atlas, Cloudinary, Resend, and AI usage charges are not included in the AWS total

## Recommended deployment model

For this project, the most practical AWS setup is Amazon Lightsail rather than a fully managed ECS or Kubernetes stack.

Why this is the best fit:

- Your app already runs as two Docker services
- Lightsail keeps monthly cost predictable
- Deployment is simpler for a client-managed environment
- It is enough for launch, pilot, and early production traffic

## Client quotation options

### Option A: Low-cost launch / pilot

Best for:

- UAT
- pilot rollout
- low to moderate traffic
- fastest launch at minimum cost

Infrastructure:

- 1 x Amazon Lightsail Linux instance, 4 GB RAM, 2 vCPU, 80 GB SSD: `$24/month`
- 1 x instance snapshot, assumed 80 GB at `$0.05/GB`: `$4/month`
- 1 x Route 53 hosted zone: `$0.50/month`

Estimated monthly AWS cost:

- `$28.50/month`
- Approximately `INR 2,631/month`

Estimated annual AWS cost:

- `$342/year`
- Approximately `INR 31,567/year`

Notes:

- Frontend and backend run on the same server using Docker Compose
- Lowest cost option
- Single server, so there is no high availability

### Option B: Recommended production

Best for:

- live client rollout
- better uptime
- safer production deployment

Infrastructure:

- 2 x Amazon Lightsail Linux instances, 4 GB RAM, 2 vCPU, 80 GB SSD: `$48/month`
- 1 x Amazon Lightsail Load Balancer: `$18/month`
- 2 x instance snapshots, assumed 80 GB each: `$8/month`
- 1 x Route 53 hosted zone: `$0.50/month`

Estimated monthly AWS cost:

- `$74.50/month`
- Approximately `INR 6,876/month`

Estimated annual AWS cost:

- `$894/year`
- Approximately `INR 82,516/year`

Notes:

- This is the best balance of cost and production safety
- Supports rolling updates better than a single-server setup
- Suitable option to share with a client as the recommended hosting plan

### Option C: Higher headroom production

Best for:

- heavier traffic
- more concurrent resume parsing / AI-assisted usage
- more operational buffer

Infrastructure:

- 2 x Amazon Lightsail Linux instances, 8 GB RAM, 2 vCPU, 160 GB SSD: `$88/month`
- 1 x Amazon Lightsail Load Balancer: `$18/month`
- 2 x instance snapshots, assumed 80 GB each: `$8/month`
- 1 x Route 53 hosted zone: `$0.50/month`

Estimated monthly AWS cost:

- `$114.50/month`
- Approximately `INR 10,568/month`

Estimated annual AWS cost:

- `$1,374/year`
- Approximately `INR 126,820/year`

Notes:

- Recommended only if usage grows beyond pilot-scale traffic
- Gives more memory headroom for Node.js processes and document-processing workloads

## What is not included in the AWS quotation

The following are outside AWS hosting cost and should be shown separately if the client asks for total platform cost:

- MongoDB Atlas subscription
- Cloudinary storage and bandwidth
- Resend email usage
- OpenAI / Gemini / Mistral / Anthropic API usage
- SSL certificate setup if using non-AWS tooling
- DevOps setup or deployment support charges
- Application maintenance and monitoring support

## Recommendation to share with client

Recommended commercial option:

- `Option B: Recommended production`

Reason:

- It is still cost-effective
- It is much safer than a single-server setup
- It presents well as a proper production environment for a client-facing product

## Source pricing references

- Amazon Lightsail pricing: <https://aws.amazon.com/lightsail/pricing/>
- Amazon Route 53 pricing: <https://aws.amazon.com/route53/pricing/>

Key pricing points verified on 5 April 2026:

- Lightsail Linux 4 GB instance with public IPv4: `$24/month`
- Lightsail Linux 8 GB instance with public IPv4: `$44/month`
- Lightsail Load Balancer: `$18/month`
- Lightsail snapshots: `$0.05/GB/month`
- Route 53 hosted zone: `$0.50/month`

## Final client-ready summary

If you want the cleanest quotation to send immediately, use this line:

`Recommended AWS hosting budget for production: USD 75/month (approx. INR 6,900/month), excluding MongoDB Atlas, Cloudinary, email, AI API usage, taxes, and domain registration.`
