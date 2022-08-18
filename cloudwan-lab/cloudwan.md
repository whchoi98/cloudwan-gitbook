# CloudWAN 배포

## CloudWAN 배포

CloudWAN 구성을 위해서 Code로 배포합니다.&#x20;

{% hint style="info" %}
CloudWAN을 구성하기 위해서는 20분 정도 소요 됩니다.&#x20;

Cloudformation 에서 배포 과정을 확인 할 수 있습니다.&#x20;
{% endhint %}

```
aws cloudformation deploy \
  --region us-east-1 \
  --stack-name "CoreNetwork" \
  --template-file "/home/ec2-user/environment/cloudwan/CoreNetwork.yml" \
  --capabilities CAPABILITY_NAMED_IAM
  
```

CloudWAN YAML 코드는 아래와 같습니다. (참조)&#x20;

```
AWSTemplateFormatVersion: 2010-09-09
Description: CloudWAN Workshop core network
Resources:
  GlobalNetwork:
    Type: "AWS::NetworkManager::GlobalNetwork"
    Properties:
      Description: Cloudwan Cloudformation Demo
      Tags:
        - Key: Env
          Value: MyCloudWAN
        - Key: Name
          Value: MyCloudWAN
  CoreNetwork:
    Type: "AWS::NetworkManager::CoreNetwork"
    Metadata:
      cfn-lint:
        config:
          ignore_checks:
            - E3002
    Properties:
      GlobalNetworkId: !Ref GlobalNetwork
      PolicyDocument:
        version: "2021.12"
        core-network-configuration:
          vpn-ecmp-support: false
          asn-ranges:
            - 64512-64555
          edge-locations:
            - location: us-east-1
#              asn: 64512
            - location: eu-central-1
#              asn: 64513
            - location: ap-northeast-1
#              asn: 64514
        segments:
          - name: Share
            description: shared segment
            require-attachment-acceptance: true
            edge-locations:
              - eu-central-1
              - us-east-1
              - ap-northeast-1
          - name: Blue
            require-attachment-acceptance: true
            edge-locations:
              - eu-central-1
              - us-east-1
              - ap-northeast-1
          - name: Green
            require-attachment-acceptance: true
            edge-locations:
              - eu-central-1
              - us-east-1
              - ap-northeast-1
          - name: Red
            require-attachment-acceptance: true
            edge-locations:
              - eu-central-1
              - us-east-1
              - ap-northeast-1
          - name: Black
            require-attachment-acceptance: true
            edge-locations:
              - eu-central-1
              - us-east-1
              - ap-northeast-1
          - name: VPN
            require-attachment-acceptance: true
            edge-locations:
              - eu-central-1
              - us-east-1
              - ap-northeast-1
        segment-actions:
          - action: share
            mode: attachment-route
            segment: Share
            share-with: "*"
        attachment-policies:
          - rule-number: 100
            condition-logic: or
            conditions:
              - type: tag-value
                operator: equals
                key: segment
                value: Share
            action:
              association-method: constant
              segment: Share
          - rule-number: 200
            condition-logic: or
            conditions:
              - type: tag-value
                operator: equals
                key: segment
                value: Blue
            action:
              association-method: constant
              segment: Blue
          - rule-number: 300
            condition-logic: or
            conditions:
              - type: tag-value
                operator: equals
                key: segment
                value: Green
            action:
              association-method: constant
              segment: Green
          - rule-number: 400
            condition-logic: or
            conditions:
              - type: tag-value
                operator: equals
                key: segment
                value: Red
            action:
              association-method: constant
              segment: Red
          - rule-number: 500
            condition-logic: or
            conditions:
              - type: tag-value
                operator: equals
                key: segment
                value: Black
            action:
              association-method: constant
              segment: Black
          - rule-number: 600
            condition-logic: or
            conditions:
              - type: tag-value
                operator: equals
                key: segment
                value: VPN
            action:
              association-method: constant
              segment: VPN

Outputs:
  CoreNetworkId:
    Value: !Ref CoreNetwork
    Description: Core Netwrok Id
  CoreNetworkArn:
    Value: !GetAtt CoreNetwork.CoreNetworkArn
    Description: Core Network ARN

```



## CloudWAN 배포 확인&#x20;

배포가 완료되면 아래와 같이 CloudWAN을 확인해 봅니다

* &#x20;AWS 관리콘솔에서 **`"VPC"`** 를 입력하고, VPC 콘솔을 실행합니다.&#x20;
* **`AWS Cloud WAN - Network Manager - Get Started`** 를 선택합니다.&#x20;

![](<../.gitbook/assets/image (10) (2).png>)

생성된 Global Network를 확인하고 선택합니다.&#x20;

* **`Global Network`** 선택&#x20;

![](<../.gitbook/assets/image (9).png>)

* **`Overview, Detail, Topology graph, Topology tree` ** 메뉴를 선택하고 확인해 봅니다.&#x20;

![](<../.gitbook/assets/image (2).png>)

* Core Network를 선택하고 메뉴들을 확인해 봅니다
* **`Core Network - Overview, Details, Sharing, Topology graph, Topology tree, Logical, Routes, Events, Monitoring`**&#x20;

![](<../.gitbook/assets/image (7).png>)

Policy versions를 선택합니다. Code로 일괄배포되어 version 1만 생성되어 있습니다

* 생성된 **`Policy version 1`** 을 선택합니다.&#x20;

![](<../.gitbook/assets/image (8).png>)

* ASN ranges

![](../.gitbook/assets/image.png)

* Edge locations , Segments&#x20;

![](<../.gitbook/assets/image (3) (2).png>)

* Attachment Policies&#x20;

![](<../.gitbook/assets/image (4).png>)
