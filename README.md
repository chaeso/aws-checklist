# AWS 체크리스트

AWS 서버 운영하면서 놓치지 말아야 할 사항들을 공유하기 위해 만들었습니다.

# 보안
1. Bastion 호스트 접속은 특정 IP에서만 접속 가능하게 하고 생성은 다음 튜토리얼을 기반으로 만든다. 
https://aws.amazon.com/ko/quickstart/architecture/linux-bastion/
2. 관리자 사이트는 외부인의 접속을 원천적으로 막기 위해 VPN 혹은 CloudFront 의 Access 기능을 사용한다.
3. 주요 사이트 접속은 OTP 사용을 의무화 한다.
4. 공개적으로 열려 있는 포트를 최소화 한다.
5. 어플리케이션의 설정 정보는 암호화 해서 보관한다.
6. 사용자/관리자의 모든 접속 로그를 S3, Glacier 에 기록되게 하여 문제 발생시 확인 가능하게 한다.
7. EC2의 디스크는 암호화가 활성화된 상태로 사용한다.
8. IAM 에 등록된 유저는 꼭 필요한 권한만 부여 하며, Access Key 키 생성은 가급적 사용하지 않는다. (대신 Role 에서 특정 인스턴스에 권한 부여)
9. DDoS와 웹공격을 막기 위해 웹사이트 앞단에 CloudFlare, CloudFront 같은 CDN 을 둔다.

# 성능/부하처리

1. 사용자가 급격히 늘어날 수 있다면 오토스케일을 적용 한다.
2. Google PageSpeed를 이용하여 서버 설정이 적절하게 되어 있는지 확인한다.
3. 부하가 많이 발생하는 API가 외부에 노출되는 경우 Rate Limit 을 건다. (API Gateway)


# 장애 예방

1. EC2 볼륨, RDS의 데이터가 항상 날라갈 수 있으므로 RDS는 정기적인 스냅샷을 기록하고, EBS 대신 EFS 를 고려해본다.
2. 헬스체크는 Pingdom, Newrelic 같은 외부 서비스를 이용한다.
3. 크론이 가끔씩 죽는 경우 Cronitor 같은 외부 서비스를 이용하여 죽으면 알림이 오게 한다.


# 관리효율

1. [CloudFormation](https://aws.amazon.com/ko/cloudformation/) 혹은 [Terraform](https://www.terraform.io/)을 이용하여 서버 구성을 코드로 관리한다.
2. 인스턴스의 기반이 되는 이미지를 미리 구워두면 편한데, 이때는 Packer 사용을 고려한다.
3. MSA로 구성할 때에는 설정파일이 하나의 서버에서 받아올 수 있게 하여 설정 정보가 바뀌어도 문제 없도록 한다. (e.g., [Spring Cloud Config](https://cloud.spring.io/spring-cloud-config/), [Consul](https://www.consul.io/))
4. 서버 설정을 할 때 노가다를 하고 있다는 생각이 들면 [Docker](https://www.docker.com/)나 [Ansible](https://www.ansible.com/) 같은 툴을 검토한다.
