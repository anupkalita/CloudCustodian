# Tokio Marine
#
## _Custodian policies to find unused resources._

Cloud Custodian is a powerful tool for managing and automating cloud resource governance. To find unused resources such as EC2 instances, Load Balancers, or RDS databases, Custodian policies can be crafted to identify and report on these idle resources. These policies typically involve setting filters that detect resources based on criteria like low utilization, lack of recent activity, or specific tags indicating they are no longer in use. Once identified, the policies can be configured to either notify administrators or automatically take action, such as stopping, terminating, or tagging the resources for review.

## Policies

Below are the services and the policy name with respective unused criteria

| Services | Policy | Criteria |
| ------ | ------ | ------- |
| EC2 | unused-ec2 | EC2 instances that has CPU Utlization under 10 % and Network Utilization under 5mb for 30 days. |
| Application Load Balancer | unused-app-elb | Load Balancer ( App & Network ) that has no incoming request for 30 days. |
| RDS | unused-rds-instances | RDS instance with no DB connection for last 30 days and Max CPU Utilization <5% for last 30 days and ReadIOPS & WriteIOPS is <5 count/sec for last 30 days |

## Installation

Steps to install c7n and run custodian policies

###### Ensure Python and pip are Installed: Make sure you have Python 3.6+ and pip installed on your system. You can check your Python version by running:
#
```sh
python3 --version
pip --version
```
###### To install Cloud Custodian:
#
```sh
python3 -m venv custodian
source custodian/bin/activate
pip install c7n
pip install c7n-org
```

###### Configure AWS CLI: Make sure your AWS CLI is configured with the necessary credentials and profiles.
#
```sh
aws configure sso
```
###### Update the `accounts.yml` file with correct details.
#
###### To run the policies and find unused resources:
#
```sh
c7n-org run -c accounts.yml -s . -u policy.yml -v
```
###### To generate unused resources report:
#
```sh
c7n-org report -c accounts.yml -s . -u policy.yml -p unused-ec2 > unused-ec2.csv
c7n-org report -c accounts.yml -s . -u policy.yml -p unused-app-elb > unused-app-elb.csv
c7n-org report -c accounts.yml -s . -u policy.yml -p unused-rds-instances > unused-rds-instances.csv
```