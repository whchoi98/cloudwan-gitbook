# Cloud9 설치 및 환경 구성

## Cloud9 소개

AWS Cloud9은 브라우저만으로 코드를 작성, 실행 및 디버깅할 수 있는 클라우드 기반 IDE(통합 개발 환경)입니다. 코드 편집기, 디버거 및 터미널이 포함되어 있습니다. Cloud9은 JavaScript, Python, PHP를 비롯하여 널리 사용되는 프로그래밍 언어를 위한 필수 도구가 사전에 패키징되어 제공되므로, 새로운 프로젝트를 시작하기 위해 파일을 설치하거나 개발 머신을 구성할 필요가 없습니다. Cloud9 IDE는 클라우드 기반이므로, 인터넷이 연결된 머신을 사용하여 사무실, 집 또는 어디서든 프로젝트 작업을 할 수 있습니다. 또한, Cloud9은 서버리스 애플리케이션을 개발할 수 있는 원활한 환경을 제공하므로 손쉽게 서버리스 애플리케이션의 리소스를 정의하고, 디버깅하고, 로컬 실행과 원격 실행 간에 전환할 수 있습니다. Cloud9에서는 개발 환경을 팀과 신속하게 공유할 수 있으므로 프로그램을 연결하고 서로의 입력 값을 실시간으로 추적할 수 있습니다.

### Cloud9 생성&#x20;

Cloud9을 실행하기 위해 아래와 같이 AWS 관리콘솔에서 **`"Cloud9"`** 을 입력하고, Cloud9을 실행합니다

`AWS 관리 콘솔 - Cloud9 - Create environment`를 선택합니다.

![](<../.gitbook/assets/image (3) (1).png>)

Cloud9의 이름을 입력합니다

* name : mycloud9

![](<../.gitbook/assets/image (12).png>)

모든 설정을 기본값으로 사용하고, 인스턴스타입은 t3.small ,Cost-Saving Setting Never로 변경합니다. 절전모드로 변경되는 것을 방지하게 됩니다. 다음 진행 버튼을 계속 누르고 Cloud9을 생성합니다.

* instance type : m5.xlarge
* Cost-saving setting : Never
* 기타 옵션 : 기본

![](<../.gitbook/assets/image (10) (1).png>)

2\~3분 후에 Cloud9 이 동작하는 것을 확인 할 수 있습니다. Cloud9 창에서 "+" 버튼을 누르고 New Terminal을 띄워서 터미널을 생성합니다. 추가로 "+"를 계속 생성하게 되면 Terminal을 다중으로 사용할 수 있습니다.



## Cloud9 환경 구성

### 기본 도구 설치&#x20;

Cloud9 IDE는 이미 AWS CLI가 설치되어 있습니다. 하지만 기본 1.x 버전이 설치되어 있습니다.

아래 명령을 통해 CLI를 2.0으로 업그레이드합니다.

```
### AWS Cli 2.0 Install

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

source ~/.bashrc
aws --version

which aws_completer
export PATH=/usr/local/bin:$PATH
source ~/.bash_profile
complete -C '/usr/local/bin/aws_completer' aws

```

Private Subnet의 EC2는 외부에서 직접 접속이 불가능합니다. Cloud9에서 연결이 가능하도록 Session Manager Plugin을 설치합니다.&#x20;

```
### Session Manager Plugin
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
sudo sudo yum install -y session-manager-plugin.rpm

```

기타 필요한 패키지를 설치합니다.&#x20;

```
### util setup

sudo yum -y install jq gettext bash-completion moreutils
for command in kubectl jq envsubst aws
  do
    which $command &>/dev/null && echo "$command in path" || echo "$command NOT FOUND"
  done

```



### Keypair 만들기

keypair를 Cloud9에서 생성합니다.

```
### SSH Key 생성
cd ~/environment/
ssh-keygen

```

key이름은 mykey 로 설정합니다.

```
mykey
```

아래와 같이 ssh key가 구성됩니다.

```
ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ec2-user/.ssh/id_rsa): mykey
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in gwlbkey.
Your public key has been saved in gwlbkey.pub.
The key fingerprint is:
SHA256:ZId12JDdlSjIuhBym08BKU/EtYbMj9EkCYtTwYpP9sY ec2-user@ip-172-31-63-114.ap-northeast-2.compute.internal
The key's randomart image is:
+---[RSA 2048]----+
|  .+=+=o. +*....o|
|  o++B=..=ooo... |
|.o..*=++* . .    |
|..+  === .       |
| + o .+.S        |
|  . E  o         |
|   .             |
|                 |
|                 |
+----[SHA256]-----+
```

아래와 같이 PrivateKey 권한을 변경합니다

```
mv ~/environment/mykey ~/environment/mykey.pem
chmod 400 ./mykey.pem

```

이제 생성된 Public Key를 계정으로 업로드 합니다. **`"--region {AWS Region}"`** 리전 옵션에서 각 리전을 지정하게 되면 해당 리전으로 생성한 Public Key를 전송합니다. 아래에서는 도쿄,서울, 버지니아, 프랑크푸르트 리전으로 전송하는 예제입니다.

```
## SSH key export
cd ~/environment/
aws ec2 import-key-pair --key-name "mykey" --public-key-material fileb://./mykey.pub --region ap-northeast-2

cd ~/environment/
aws ec2 import-key-pair --key-name "mykey" --public-key-material fileb://./mykey.pub --region ap-northeast-1

cd ~/environment/
aws ec2 import-key-pair --key-name "mykey" --public-key-material fileb://./mykey.pub --region us-east-1

cd ~/environment/
aws ec2 import-key-pair --key-name "mykey" --public-key-material fileb://./mykey.pub --region eu-central-1

```

아래와 같이 업로드가 완료됩니다.

```
whchoi:~/environment $ aws ec2 import-key-pair --key-name "gwlbkey" --public-key-material fileb://gwlbkey.pub --region ap-northeast-2
{
    "KeyFingerprint": "xx:xx:xx:xx:xx:65:3a:70:fb:b1:fa:dd:6c:59:c6:9e",
    "KeyName": "nykey",
    "KeyPairId": "key-xxxxxxxxx"
}
```

CloudWAN Hands On LAB 구성을 위한 파일들을 복제하고, Public Key의 이름을 변수로 저장해 둡니다.&#x20;

```
### CloudWAN 관련 Yaml 파일 복제
cd ~/environment
git clone https://github.com/whchoi98/cloudwan
git clone https://github.com/whchoi98/useful-shell.git

### Public Key Name 환경변수 저장
export KeyName=mykey
echo "export KeyName=${KeyName}" | tee -a ~/.bash_profile
source ~/.bash_profile

```



Cloud9 Role 변경

Lab에서는 Cloud9을 기반으로 대부분의 관리콘솔도구로 사용하기 때문에, Cloud9을 위한 권한부여가 필요합니다

IAM을 실행하기 위해 아래와 같이 AWS 관리콘솔에서 **`"IAM"`** 을 입력하고, IAM을 실행합니다.&#x20;

* **`Role - Create role`** 선택

![](<../.gitbook/assets/image (7) (1).png>)

* **`AWS Service - EC2`** 선택

![](<../.gitbook/assets/image (6) (1).png>)

* **`administratoraccess`** 를 선택합니다.&#x20;

Permission 필터를 위해 아래 administratoraccess 를 필터해서 Policy를 선택합니다.&#x20;

```
administratoraccess
```

![](<../.gitbook/assets/image (4) (1).png>)

Role 이름을 입력하고 , Role을 생성합니다.&#x20;

* Role Name

```
cloud9admin
```

![](<../.gitbook/assets/image (8) (1).png>)

Cloud9의 Role을 변경하기 위해 아래와 같이 AWS 관리콘솔에서 **`"EC2"`** 을 입력하고, EC2 콘솔을 실행합니다.&#x20;

Seoul Region에 생성된 Cloud9 EC2를 선택하고, IAM Role을 변경합니다.&#x20;

* Cloud9 EC2 선택 - Action - Security - Modify IAM Role&#x20;

![](<../.gitbook/assets/image (6).png>)

앞서 생성한 IAM Role (cloud9admin) 로 변경합니다.&#x20;

![](<../.gitbook/assets/image (11).png>)

Cloud9 콘솔에서 Preference - AWS Settings - Credentials 를 비활성화 시킵니다

앞서 선택한 Cloud9admin 의 권한으로 변경됩니다.&#x20;

![](<../.gitbook/assets/image (2) (1).png>)

이제 사전 준비를 위한 모든 구성이 완료 되었습니다.&#x20;
