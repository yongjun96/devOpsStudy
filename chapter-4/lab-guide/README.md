## [요구사항]
![Infra Architecture](./chapter4-arch.png)
```
- VPC 자동생성 이용 
  • name prefixName : ecs-prac
  • VPC IPv4 CIDR 블록  : 20.0.0.0/20 ( 4,096 IPs )
  • Subnet
    - public-subnet-a  : 20.0.0.0/24 ( 256 IPs )
    - public-subnet-c  : 20.0.1.0/24 ( 256 IPs )
    - private-subnet-a : 20.0.8.0/24 ( 256 IPs )
    - private-subnet-c : 20.0.9.0/24 ( 256 IPs )
- Public ec2 instance name 
  • name :  ec2-ecr
- docker , git , aws cli 설치 및 aws configure 설정
- SpringBoot 소스 EC2서버에서 ECR에 업로드 
  • repo name : spring-ecr
- Private subnet에 Fargate cluster 구성 
  • name : ecs-far-lab
- ECS cluster name 
  • name :spring-ecs 
- Task 생성 
  • name : ecs-farlab-task
- Service 생성 : ecs-farlab-service
- ALB Target Group 생성 : ecs-farlab-alb-tg
- ALB 생성 : ecs-farlab-alb
```

## Step 1
```
인프라 생성 (vpc등 마법사 이용)
  - vpc  
  - public subnet2 
  - private subnet 2
  - RT(routing table) 
  - 네트워크 acl 
  - IG(internet gateway) 
  - NG(nat gateway) 
  - EIP(탄력적ip) 
  - 보안그룹 생성
    - ssh-security-grp ( 22 port )
    - http-secure-grp ( 80 port )
    - springboot-security-grp ( 8080 port )
```
## Step 2
```
ec2 instance 생성 - 접속할 키페어는 developer
  : Instance Type 
   - EC2 instance : t2.micro  , AMI - Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type
```
## Step 3
```
ec2 instance 필요한 Library 설치
- docker 설치
  : command ( 설치 명령어 )
    sudo su
    yum install -y docker
    systemctl enable --now docker
    usermod -aG docker ec2-user
    exit // sudo logout
    exit // ec2 server logout
    // 다시 ec2-user 로그인후
    docker version

- git install
  : yum install git

- aws cli
  : aws cli 설치
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
    aws --version

- aws configure 설정
  : aws configure (accesskey, secretkey , region 설정)
    - accesskey : 본인 accesskey
    - secretkey : 본인 secretkey
    - region : ap-northeast-2
    - output : json
    
- springboot sample code download
  : git clone https://github.com/azjaehyun/fc-study.git
    cd ./fc-study/chapter-4/spring-boot-thymeleaf-tour
  : docker build  
    docker build -t spring-ecr:latest
  : ecr upload  
    docker tag spring-ecr:latest {yourECRaddress}/spring-ecr:latest
    docker puth {yourECRaddress}/spring-ecr:latest
```
## Step 4
```
ECS fargate cluster 생성 ( private subnet에 생성)
IAM 생성 (ecs-task-rule)
 - clip4 - ECS 기반 컨테이너 애플리케이선 배포 실습 참조 
``` 

## step 5
```
ecs task 생성
```

## step 6
```
ecs service 생성 ( private subnet에 생성 )
```

## step 7
```
- application load balancer target group 생성
- application load balnacer 생성 ( public subnet에 생성 )
```

## step 8
```
http://{application-load-balancer}:8080
서비스 확인
```