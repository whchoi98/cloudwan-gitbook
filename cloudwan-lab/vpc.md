---
description: 'Update : 2022-08-18'
---

# VPC 배포

## VPC 배포

Cloud9 터미널을 6개를 신규 열고, 아래와 같이 VPC를 구성합니다.&#x20;

### 1. ap-northeast-1 VPC 배포

Cloud9 터미널에서 ap-northeast-1 Region의 Blue,Green VPC를 아래와 같이 생성합니다.&#x20;

```
## NRT VPC
cd ./cloudwan
aws cloudformation deploy \
  --region ap-northeast-1 \
  --stack-name "NRT-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/NRT-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM &&
aws cloudformation deploy \
  --region ap-northeast-1 \
  --stack-name "NRT-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/NRT-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM


```

### 2. ap-southeast-2 VPC 배포

Cloud9 터미널을 6개를 신규 열고, 아래와 같이 VPC를 구성합니다.&#x20;

Cloud9 터미널에서 ap-southeast-2 Region의 Blue,Green VPC를 아래와 같이 생성합니다.&#x20;

```
## SYD VPC
cd ./cloudwan
aws cloudformation deploy \
  --region ap-southeast-2 \
  --stack-name "SYD-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/SYD-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM &&
aws cloudformation deploy \
  --region ap-southeast-2 \
  --stack-name "SYD-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/SYD-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

### 3. us-east-1 VPC 배포

Cloud9 터미널에서 us-east-1 Region의 Blue,Green VPC를 아래와 같이 생성합니다.&#x20;

```
## IAD VPC
cd ./cloudwan
aws cloudformation deploy \
  --region us-east-1\
  --stack-name "IAD-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/IAD-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM &&   
aws cloudformation deploy \
  --region us-east-1 \
  --stack-name "IAD-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/IAD-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

### 4. us-west-2 VPC 배포

Cloud9 터미널에서 us-west-2 Region의 Blue,Green VPC를 아래와 같이 생성합니다.&#x20;

```
## PDX VPC
cd ./cloudwan
aws cloudformation deploy \
  --region us-west-2\
  --stack-name "PDX-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/PDX-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM &&   
aws cloudformation deploy \
  --region us-west-2 \
  --stack-name "PDX-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/PDX-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

### 5. eu-central-1 VPC 배포

Cloud9 터미널에서 eu-central-1 Region의 Blue,Green VPC를 아래와 같이 생성합니다.&#x20;

```
## FRT VPC ###
cd ./cloudwan
aws cloudformation deploy \
  --region eu-central-1\
  --stack-name "FRA-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/FRA-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM && \
aws cloudformation deploy \
  --region eu-central-1\
  --stack-name "FRA-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/FRA-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM
  

```

### 6. eu-central-1 VPC 배포

Cloud9 터미널에서 eu-central-1 Region의 Blue,Green VPC를 아래와 같이 생성합니다.&#x20;

```
## DUB VPC ###
cd ./cloudwan
aws cloudformation deploy \
  --region eu-west-1\
  --stack-name "DUB-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/DUB-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM && \
aws cloudformation deploy \
  --region eu-west-1\
  --stack-name "DUB-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/DUB-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM
  
```



정상적으로 배포되었는지 각 리전의 Cloudformation 에서 확인합니다.&#x20;

* ap-northeast-1
* ap-southeast-2
* us-east-1
* us-west-2
* eu-central-1
* eu-west-1



