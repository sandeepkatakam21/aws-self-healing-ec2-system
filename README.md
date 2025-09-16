# AWS Self-Healing EC2 System

🔄 **A robust, self-healing EC2 autoscaling architecture using AWS Lambda and CloudWatch for automated instance recovery and comprehensive monitoring.**

[![AWS](https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org/)
[![Terraform](https://img.shields.io/badge/Terraform-623CE4?style=for-the-badge&logo=terraform&logoColor=white)](https://terraform.io/)

## 🏗️ Architecture Overview

This project implements a self-healing infrastructure that automatically detects, diagnoses, and recovers from EC2 instance failures using:

- **Auto Scaling Groups** for high availability
- **CloudWatch Alarms** for monitoring and alerting
- **Lambda Functions** for automated remediation
- **SNS** for notifications
- **Systems Manager** for instance management

## 🚀 Features

- ✅ **Automated Instance Recovery**: Failed instances are automatically replaced
- ✅ **Health Monitoring**: Continuous monitoring of CPU, memory, and custom metrics
- ✅ **Smart Alerting**: Multi-tier alerting system with escalation
- ✅ **Cost Optimization**: Intelligent scaling based on demand
- ✅ **Security Compliance**: Built-in security best practices
- ✅ **Multi-AZ Support**: Cross-availability zone redundancy

## 📋 Prerequisites

- AWS CLI configured with appropriate permissions
- Terraform >= 1.0
- Python 3.8+
- AWS Account with necessary service limits

### Required AWS Permissions

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "autoscaling:*",
        "cloudwatch:*",
        "lambda:*",
        "sns:*",
        "iam:*",
        "ssm:*"
      ],
      "Resource": "*"
    }
  ]
}
```

## 🏁 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/sandeepkatakam21/aws-self-healing-ec2-system.git
cd aws-self-healing-ec2-system
```

### 2. Configure Variables

```bash
cp terraform/terraform.tfvars.example terraform/terraform.tfvars
# Edit terraform.tfvars with your specific values
```

### 3. Deploy Infrastructure

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

### 4. Test the System

```bash
# Run health checks
python scripts/health_check.py

# Simulate failure
python scripts/simulate_failure.py
```

## 📁 Project Structure

```
aws-self-healing-ec2-system/
├── terraform/
│   ├── modules/
│   │   ├── autoscaling/
│   │   ├── cloudwatch/
│   │   ├── lambda/
│   │   └── networking/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── lambda/
│   ├── health_checker/
│   ├── instance_manager/
│   └── notification_handler/
├── scripts/
│   ├── health_check.py
│   ├── simulate_failure.py
│   └── deployment.py
├── monitoring/
│   ├── cloudwatch_dashboards/
│   └── custom_metrics/
├── docs/
│   ├── architecture.md
│   ├── deployment.md
│   └── troubleshooting.md
└── tests/
    ├── unit/
    └── integration/
```

## ⚙️ Configuration

### Environment Variables

```bash
export AWS_REGION="us-west-2"
export ENVIRONMENT="production"
export AUTO_SCALING_MIN_SIZE="2"
export AUTO_SCALING_MAX_SIZE="10"
export NOTIFICATION_EMAIL="admin@company.com"
```

### Terraform Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `aws_region` | AWS region for deployment | `us-west-2` | Yes |
| `environment` | Environment name | `dev` | Yes |
| `vpc_cidr` | VPC CIDR block | `10.0.0.0/16` | No |
| `instance_type` | EC2 instance type | `t3.micro` | No |
| `min_size` | ASG minimum size | `2` | No |
| `max_size` | ASG maximum size | `10` | No |

## 🔍 Monitoring & Alerts

### CloudWatch Metrics

- **CPU Utilization**: Triggers scaling events
- **Memory Usage**: Custom metric via CloudWatch agent
- **Disk Space**: Monitor root volume usage
- **Application Health**: Custom health checks

### Alert Thresholds

- 🟢 **Normal**: CPU < 70%, Memory < 80%
- 🟡 **Warning**: CPU 70-85%, Memory 80-90%
- 🔴 **Critical**: CPU > 85%, Memory > 90%

## 🧪 Testing

### Unit Tests

```bash
python -m pytest tests/unit/ -v
```

### Integration Tests

```bash
python -m pytest tests/integration/ -v
```

### Load Testing

```bash
# Install dependencies
pip install locust

# Run load test
locust -f tests/load/locustfile.py
```

## 🔧 Troubleshooting

### Common Issues

1. **Instances not launching**
   - Check IAM permissions
   - Verify security group rules
   - Ensure subnet has available IP addresses

2. **Lambda functions failing**
   - Check CloudWatch logs
   - Verify execution role permissions
   - Test function locally

3. **Scaling not working**
   - Review CloudWatch alarms
   - Check Auto Scaling policies
   - Verify metric thresholds

### Debug Commands

```bash
# Check Auto Scaling Group status
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names my-asg

# View CloudWatch logs
aws logs describe-log-groups
aws logs tail /aws/lambda/health-checker --follow

# Test Lambda function
aws lambda invoke --function-name health-checker output.json
```

## 📊 Cost Optimization

- **Spot Instances**: Use for non-critical workloads
- **Reserved Instances**: For predictable workloads
- **Right-sizing**: Regular instance type analysis
- **Auto Shutdown**: Schedule non-production resources

## 🔒 Security Best Practices

- ✅ Least privilege IAM roles
- ✅ VPC with private subnets
- ✅ Security groups with minimal access
- ✅ Encrypted EBS volumes
- ✅ Regular security patches via SSM
- ✅ CloudTrail logging enabled

## 🚀 Deployment Strategies

### Blue-Green Deployment

```bash
# Deploy to staging
terraform workspace select staging
terraform apply

# Validate deployment
python scripts/validate_deployment.py

# Switch traffic
terraform workspace select production
terraform apply
```

### Rolling Updates

```bash
# Update launch template
terraform apply -target=aws_launch_template.main

# Trigger instance refresh
aws autoscaling start-instance-refresh --auto-scaling-group-name my-asg
```

## 📈 Performance Tuning

### Instance Optimization

- Use latest generation instance types
- Enable enhanced networking
- Optimize EBS volume types
- Configure instance store if needed

### Application Optimization

- Implement health check endpoints
- Use connection pooling
- Enable application-level caching
- Monitor application metrics

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

- 📧 Email: support@company.com
- 💬 Slack: #aws-infrastructure
- 📖 Documentation: [Wiki](https://github.com/sandeepkatakam21/aws-self-healing-ec2-system/wiki)
- 🐛 Issues: [GitHub Issues](https://github.com/sandeepkatakam21/aws-self-healing-ec2-system/issues)

## 🏆 Acknowledgments

- AWS Documentation and Best Practices
- Terraform AWS Provider
- Community contributions and feedback

---

**Made with ❤️ for reliable AWS infrastructure**
