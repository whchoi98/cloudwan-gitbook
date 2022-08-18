---
description: 'Update : 2022-08-18'
---

# VPC 배포

## VPC 배포

### ap-northeast-1 VPC 배포

Cloud9 터미널을 4개를 신규 열고, 아래와 같이 4개의 VPC를 구성합니다.&#x20;

ap-northeast-1 Region의 Blue VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region ap-northeast-1 \
  --stack-name "NRT-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/Tokyo-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

ap-northeast-1 Region의 Green VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region ap-northeast-1 \
  --stack-name "NRT-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/Tokyo-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

ap-northeast-1 Region의 Red VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region ap-northeast-1 \
  --stack-name "NRT-VPC-Red" \
  --template-file "/home/ec2-user/environment/cloudwan/Tokyo-VPC-Red.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM
  
```

ap-northeast-1 Region의 Black VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region ap-northeast-1 \
  --stack-name "NRT-VPC-Black" \
  --template-file "/home/ec2-user/environment/cloudwan/Tokyo-VPC-Black.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

### us-east-1 VPC 배포

Cloud9 터미널을 4개를 신규 열고, 아래와 같이 4개의 VPC를 구성합니다.&#x20;

us-east-1 Region의 Blue VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region us-east-1\
  --stack-name "IAD-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/IAD-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

us-east-1 Region의 Green VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region us-east-1 \
  --stack-name "IAD-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/IAD-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

us-east-1 Region의 Red VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region us-east-1\
  --stack-name "IAD-VPC-Red" \
  --template-file "/home/ec2-user/environment/cloudwan/IAD-VPC-Red.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

us-east-1 Region의 Black VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region us-east-1 \
  --stack-name "IAD-VPC-Black" \
  --template-file "/home/ec2-user/environment/cloudwan/IAD-VPC-Black.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

### eu-central-1 VPC 배포

Cloud9 터미널을 4개를 신규 열고, 아래와 같이 4개의 VPC를 구성합니다.&#x20;

eu-central-1 Region의 Blue VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region eu-central-1\
  --stack-name "FRA-VPC-Blue" \
  --template-file "/home/ec2-user/environment/cloudwan/FRA-VPC-Blue.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

eu-central-1 Region의 Green VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region eu-central-1\
  --stack-name "FRA-VPC-Green" \
  --template-file "/home/ec2-user/environment/cloudwan/FRA-VPC-Green.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM
  
```

eu-central-1 Region의 Red VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region eu-central-1\
  --stack-name "FRA-VPC-Red" \
  --template-file "/home/ec2-user/environment/cloudwan/FRA-VPC-Red.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM

```

eu-central-1 Region의 Black VPC를 아래와 같이 생성합니다.&#x20;

```
aws cloudformation deploy \
  --region eu-central-1 \
  --stack-name "FRA-VPC-Black" \
  --template-file "/home/ec2-user/environment/cloudwan/FRA-VPC-Black.yml" \
  --parameter-overrides "KeyPair=$KeyName" \
  --capabilities CAPABILITY_NAMED_IAM
  
```



정상적으로 배포되었는지 각 리전의 Cloudformation 에서 확인합니다.&#x20;

* ap-northeast-1
* us-east-1
* eu-central-1

![](<../.gitbook/assets/image (1).png>)

