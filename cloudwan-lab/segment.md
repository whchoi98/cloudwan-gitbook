---
description: 'Update : 2022-08-18'
---

# Segment 구성 및 확인

### VPC Routing 구성

CloudWAN 구성으로 각 리전의 라우팅을 동적으로 처리가 되지만, VPC 내부의 라우팅은 정적으로 구성해야 합니다

아래와 같은 정보를 기반으로 각 리전에 생성된 VPC에 라우팅을 추가합니다

![](<../.gitbook/assets/image (15).png>)

아래 그림은 NRT-VPC-Blue 의 NRT-VPC-Blue-Private-Subnet-A-RT 라우팅 테이블을 수정하는 예제입니다

* VPC - Route tables - NRT-VPC-Blue-Private-Subnet-A-RT - Edit Route
* Destination : 목적지&#x20;
* Traget : Core Network

![](<../.gitbook/assets/image (2) (2) (2) (1).png>)

정상적으로 추가 되면 아래와 같이 결과가 출력됩니다. &#x20;

![](<../.gitbook/assets/image (6).png>)

aws cli로 일괄 라우팅 적용을 위해, 사전에 CoreNetwork ARN 정보를 확인하고 복사해 둡니다. 그리고 아래에서 처럼 변수에 저장합니다.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

cloud9 terminal에서도 확인이 가능합니다.

```
aws cloudformation describe-stacks --stack-name CoreNetwork --region us-east-1 | jq -r '.Stacks[].Outputs' | grep networkmanager

```

아래에서 처럼 변수에 저장합니다.

```
export core_network_arn='arn_output'
echo "export core_network_arn=${core_network_arn}" | tee -a ~/.bash_profile
source ~/.bash_profile

```

routing table id와 각 VPC CIDR 주소를 변수에 입력해 둡니다.&#x20;

```
## Blue,Green Route Table ID ##
~/environment/cloudwan/green_route_table_id.sh
~/environment/cloudwan/blue_route_table_id.sh
source ~/.bash_profile

##CIDR 주소##
~/environment/cloudwan/cidr_table.sh

```

Blue,Green Segment를 통해서 각 리전의 VPC들이 통신이 가능하도록, Routing Table을 추가합니다.

```
##routing table add##
source ~/.bash_profile
~/environment/cloudwan/blue_routing_add.sh
~/environment/cloudwan/green_routing_add.sh

```

정상적으로 라우팅 테이블들이 추가 되었는지 확인해 봅니다.&#x20;



### EC2 접속 정보 확인



Cloud9 terminal 에서 아래 명령을 실행시켜서, 각 리전의 private Subnet EC2의 id를 저장합니다

```
./aws_ec2_ext_nrt.sh | grep "Blue-Private" >> /home/ec2-user/environment/Blue-Private.txt
./aws_ec2_ext_iad.sh | grep "Blue-Private" >> /home/ec2-user/environment/Blue-Private.txt
./aws_ec2_ext_fra.sh | grep "Blue-Private" >> /home/ec2-user/environment/Blue-Private.txt

./aws_ec2_ext_nrt.sh | grep "Green-Private" >> /home/ec2-user/environment/Green-Private.txt
./aws_ec2_ext_iad.sh | grep "Green-Private" >> /home/ec2-user/environment/Green-Private.txt
./aws_ec2_ext_fra.sh | grep "Green-Private" >> /home/ec2-user/environment/Green-Private.txt

./aws_ec2_ext_nrt.sh | grep "Red-Private" >> /home/ec2-user/environment/Red-Private.txt
./aws_ec2_ext_iad.sh | grep "Red-Private" >> /home/ec2-user/environment/Red-Private.txt
./aws_ec2_ext_fra.sh | grep "Red-Private" >> /home/ec2-user/environment/Red-Private.txt

./aws_ec2_ext_nrt.sh | grep "Black-Private" >> /home/ec2-user/environment/Black-Private.txt
./aws_ec2_ext_iad.sh | grep "Black-Private" >> /home/ec2-user/environment/Black-Private.txt
./aws_ec2_ext_fra.sh | grep "Black-Private" >> /home/ec2-user/environment/Black-Private.txt
```

### Private EC2에서 Segment 확인

위의 결과에서 확인된 EC2로 아래와 같이 명령을 실행해서 Private Subnet에 배치되어 있는 EC2 콘솔에 접속해 봅니다

* ap-northeast-1 Blue Private EC2 접속 예제

```
aws ssm start-session --target ${NRT-Blue-Private-A-01-id} --region ap-northeast-1
```

* ap-northeast-1 Green Private EC2 접속 예제

```
aws ssm start-session --target ${NRT-Green-Private-A-01-id} --region ap-northeast-1
```

* ap-northeast-1 Red Private EC2 접속 예제

```
aws ssm start-session --target ${NRT-Red-Private-A-01-id} --region ap-northeast-1
```

* ap-northeast-1 Black Private EC2 접속 예제

```
aws ssm start-session --target ${NRT-Black-Private-A-01-id} --region ap-northeast-1
```

* ap-northeast-1 Private Subnet A 의 EC2에서 Blue Segment 트래픽을 확인해 봅니다.&#x20;
* ap-northeast-1 Private Subnet A 의 EC2에서 Green,Red,Black Segment 트래픽을 확인해 봅니다.&#x20;

```
sudo -s
ping 10.31.21.101 -c 5
ping 10.41.21.101 -c 5
ping 10.32.21.101 -c 2
ping 10.43.21.101 -c 2
ping 10.24.21.101 -c 2
ping 10.22.21.101 -c 2

```

정상적으로 Segment가 연결된 것을 확인 할 수 있습니다

