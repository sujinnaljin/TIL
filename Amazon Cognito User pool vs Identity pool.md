# Amazon Cognito User pool vs Identity pool
## User pool
* authentication (인증, 즉 신원 확인)을 위한 것
* 이를 통해 로그인하거나 third-party identity provider (IdP) 를 통해 로그인 가능
* 회원 가입 및 로그인을 지원하며, 로그인한 사용자에게 대한 provisioning identity tokens 을 제공

## Identity pool (aka. Federated Identities)
* authorization (인가, 즉 권한 부여 / 액세스 제어)를 위한 것
* 사용자에게 다른 AWS 서비스에 대한 액세스 권한을 부여하기 위해 고유한 ID를 생성할 수 있음
* 일시적이고 제한된 권한의 AWS 자격 증명을 얻어서 Amazon DynamoDB, Amazon S3 및 Amazon API Gateway와 같은 다른 AWS 서비스에 안전하게 접근 가능


# 출처
* https://repost.aws/knowledge-center/cognito-user-pools-identity-pools
* https://sst.dev/archives/cognito-user-pool-vs-identity-pool.html
* https://tutorialsdojo.com/amazon-cognito-user-pools-vs-identity-pools/
