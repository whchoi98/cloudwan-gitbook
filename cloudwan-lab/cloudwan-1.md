---
description: 'Update : 2022-08-18'
---

# CloudWAN 연결

## CloudWAN 연결

VPC 와 CloudWAN Segment 와 연결합니다

* VPC - CloudWAN - Global networks - Core network - Attachments - create attachment&#x20;

![](<../.gitbook/assets/image (13).png>)

* Name - Attachment 이름
* Edge Location - 해당 리전
* Attachment type - VPC

![](<../.gitbook/assets/image (3) (1).png>)

* VPC ID - Segment에 연결될 VPC ID
* Subnet - Attachment 될 Subnet

![](<../.gitbook/assets/image (10).png>)

* Tag - Key/Value 타입으로 지정

![](<../.gitbook/assets/image (5).png>)

아래와 같은 Attachment를 구성합니다.&#x20;

| Name          | Edge location | Attachment Type | VPC ID        | Subnet ID         | Tag           |
| ------------- | ------------- | --------------- | ------------- | ----------------- | ------------- |
| NRT-VPC-Blue  | Tokyo         | VPC             | NRT-VPC-Blue  | CloudWANSubnetA,B | segment/Blue  |
| NRT-VPC-Green | Tokyo         | VPC             | NRT-VPC-Green | CloudWANSubnetA,B | segment/Green |
| NRT-VPC-Red   | Tokyo         | VPC             | NRT-VPC-Red   | CloudWANSubnetA,B | segment/Red   |
| NRT-VPC-Black | Tokyo         | VPC             | NRT-VPC-Black | CloudWANSubnetA,B | segment/Black |
| IAD-VPC-Blue  | Virginia      | VPC             | IAD-VPC-Blue  | CloudWANSubnetA,B | segment/Blue  |
| IAD-VPC-Green | Virginia      | VPC             | IAD-VPC-Green | CloudWANSubnetA,B | segment/Green |
| IAD-VPC-Red   | Virginia      | VPC             | IAD-VPC-Red   | CloudWANSubnetA,B | segment/Red   |
| IAD-VPC-Black | Virginia      | VPC             | IAD-VPC-Black | CloudWANSubnetA,B | segment/Black |
| FRA-VPC-Blue  | Frankfurt     | VPC             | FRA-VPC-Blue  | CloudWANSubnetA,B | segment/Blue  |
| FRA-VPC-Green | Frankfurt     | VPC             | FRA-VPC-Green | CloudWANSubnetA,B | segment/Green |
| FRA-VPC-Red   | Frankfurt     | VPC             | FRA-VPC-Red   | CloudWANSubnetA,B | segment/Red   |
| FRA-VPC-Black | Frankfurt     | VPC             | FRA-VPC-Black | CloudWANSubnetA,B | segment/Black |

Segment에 VPC를 Attachment 할 때 Accept 가 필요합니다. Attachment 생성 후 해당 Attachment ID를 선택하고 Accept 합니다

![](<../.gitbook/assets/image (1) (1) (1).png>)

Accept를 수행하면 VPC당 연결시간이 7분 내외 소요 됩니다.&#x20;

![](<../.gitbook/assets/image (1) (1).png>)
