# tom-thumb
To demonstrate invoking a long-running Fargate task on demand using Lambda

git clone https://github.com/skarlekar/tom-thumb.git
cd bean-counter-service/pre-requisites/

# Start of Prerequisites
./prereqs-ubuntu.sh
aws configure set default.region us-east-1
aws configure set default.output json

# Create ecsTaskExecutionRole and taskRole in IAM and note down the ARNs
source ./create-roles.sh

# Create VPC, Subnets and Security groups for running Fargate
source ./create-vpc-subnets.sh

# Create the ALB
source ./create-alb.sh

# ------------------ End of prerequisites

# Create the ECR Repository
source ./create-tom-thumb-repository.sh

# Build Docker image and push to ECR repository
./push-to-ecr.sh

# Create the bean counter log group
./create-tom-thumb-log-group.sh

# Create the bean counter cluster
./create-tom-thumb-cluster.sh


# Generate bean counter task definition from the template
./generate-tom-thumb-task-definition.sh <videoFileUrl> <position-in-secs>

# Register the bean counter task definition
./register-tom-thumb-task.sh

# Generate the run params for running the task
./generate-run-tom-thumb-task.sh <videoFileUrl> <position-in-secs>

# Run the task
./run-tom-thumb-task.sh

# Verify the results

