# aws-ecs-alb-service-task
A modified version of https://github.com/cloudposse/terraform-aws-ecs-alb-service-task

# Usage
```hcl
module "ecs-alb-service-task" {
  source = "github.com/globeandmail/aws-ecs-alb-service-task?ref=1.0"

  namespace                 = var.app_name
  stage                     = var.stage_name
  name                      = var.app_name
  alb_target_group_arn      = var.target_group_arn
  container_definition_json = module.container-defs.json
  container_name            = var.app_name
  ecs_cluster_arn           = module.ecs-cluster.ecs_cluster_arn
  launch_type               = "EC2"
  vpc_id                    = var.vpc_id
  alb_security_group        = module.alb_sg.this_security_group_id
  security_group_ids = []
  container_port     = 8080
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| alb\_security\_group | Security group of the ALB | string | n/a | yes |
| alb\_target\_group\_arn | The ALB target group ARN for the ECS service | string | n/a | yes |
| container\_definition\_json | The JSON of the task container definition | string | n/a | yes |
| container\_name | The name of the container in task definition to associate with the load balancer | string | n/a | yes |
| ecs\_cluster\_arn | The ARN of the ECS cluster where service will be provisioned | string | n/a | yes |
| name | Solution name, e.g. 'app' or 'cluster' | string | n/a | yes |
| namespace | Namespace, which could be your organization name, e.g. 'eg' or 'cp' | string | n/a | yes |
| security\_group\_ids | Security group IDs to allow in Service `network\_configuration` | list | n/a | yes |
| stage | Stage, e.g. 'prod', 'staging', 'dev', or 'test' | string | n/a | yes |
| vpc\_id | The VPC ID where resources are created | string | n/a | yes |
| assign\_public\_ip | Assign a public IP address to the ENI \(Fargate launch type only\). Valid values are true or false. Default false. | string | `"false"` | no |
| attributes | Additional attributes \(e.g. `1`\) | list | `[]` | no |
| container\_port | The port on the container to associate with the load balancer | string | `"80"` | no |
| delimiter | Delimiter to be used between `name`, `namespace`, `stage`, etc. | string | `"-"` | no |
| deployment\_controller\_type | Type of deployment controller. Valid values: `CODE\_DEPLOY`, `ECS`. | string | `"ECS"` | no |
| deployment\_maximum\_percent | The upper limit of the number of tasks \(as a percentage of `desired\_count`\) that can be running in a service during a deployment | string | `"200"` | no |
| deployment\_minimum\_healthy\_percent | The lower limit \(as a percentage of `desired\_count`\) of the number of tasks that must remain running and healthy in a service during a deployment | string | `"100"` | no |
| desired\_count | The number of instances of the task definition to place and keep running | string | `"1"` | no |
| health\_check\_grace\_period\_seconds | Seconds to ignore failing load balancer health checks on newly instantiated tasks to prevent premature shutdown, up to 7200. Only valid for services configured to use load balancers | string | `"0"` | no |
| ignore\_changes\_task\_definition | Whether to ignore changes in container definition and task definition in the ECS service | string | `"true"` | no |
| launch\_type | The launch type on which to run your service. Valid values are `EC2` and `FARGATE` | string | `"FARGATE"` | no |
| network\_mode | The network mode to use for the task. This is required to be awsvpc for `FARGATE` `launch\_type` | string | `"awsvpc"` | no |
| tags | Additional tags \(e.g. `map\('BusinessUnit`,`XYZ`\) | map | `{}` | no |
| task\_cpu | The number of CPU units used by the task. If using `FARGATE` launch type `task\_cpu` must match supported memory values \(https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task\_definition\_parameters.html#task\_size\) | string | `"256"` | no |
| task\_memory | The amount of memory \(in MiB\) used by the task. If using Fargate launch type `task\_memory` must match supported cpu value \(https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task\_definition\_parameters.html#task\_size\) | string | `"512"` | no |

## Outputs

| Name | Description |
|------|-------------|
| ecs\_exec\_role\_policy\_id | The ECS service role policy ID, in the form of role\_name:role\_policy\_name |
| ecs\_exec\_role\_policy\_name | ECS service role name |
| service\_name | ECS Service name |
| service\_role\_arn | ECS Service role ARN |
| service\_security\_group\_id | Security Group ID of the ECS task |
| task\_definition\_family | ECS task definition family |
| task\_definition\_revision | ECS task definition revision |
| task\_exec\_role\_arn | ECS Task exec role ARN |
| task\_exec\_role\_name | ECS Task role name |
| task\_role\_arn | ECS Task role ARN |
| task\_role\_id | ECS Task role id |
| task\_role\_name | ECS Task role name |

