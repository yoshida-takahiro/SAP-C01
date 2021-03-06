# AWS System Manager
## AWS System Manager パラメータストア(SSM)
構成データ管理のための安全で階層的なストレージを提供し秘密の管理を行います。パスワード、データベース文字列、マシンイメージ(AMI)ID、およびパラメータ値としてのライセンスコードなどのデータを保管できます。  
プレーンテキストまたは暗号化データの形式で保管でき、参照できます。  
Systems Manager パラメータをスクリプト、コマンド、 SSM ドキュメント、構成、および自動化ワークフローを、 パラメーターの作成時に指定します。

### パラメータストアとSecret managerの違い
設定とシークレットにひとつのストアが欲しい場合、パラメータストア
ライフサイクル管理を備えたシークレット専用のストアが欲しい場合、Secret Manager
パラメータストアは無料、Secret Managerは有料

# NACL（ネットワークACL）
ステートレス
サブネットに対して設定

# セキュリティグループ
ステートフル

# IAM
## タグでの運用制限
IAMグループとIAMポリシーを使って
特定のタグが付与されたインスタンスのみ操作できるなどの運用を実現することができる。

## 外部IDの信頼条件設定
サードパーティのサービスにリソースのアクセス許可をする場合はIAMロールについているIAMポリシーに外部IDを信頼条件として設定し、外部ID＋ロールのARNでAssumeRoleすることで安全に外部アクセスさせることができる。

## リソースベースのIAMポリシーの付与によるクロスアカウントアクセス
ロールによってアクセス許可が付与される場合は元から与えられていたアクセス許可を放棄しないといけないが（元のアクセス許可とは？？）
リソースベースのポリシーで許可した場合は元のアクセス許可は保持されるというメリットがある

# STS(Security Token Service)
IAMの一機能  
一時的セキュリティ認証情報を持つ、信頼されたユーザーを作成および提供することができる。
ユーザーにはロールが紐づいている
## ID フェデレーションでのSTSの使用
### ウェブIDフェデレーション
基本的にはCognitoを使うことで実現できるが、直接AssumeRoleWithWebIdentity APIを呼び出しても実現できる（Cognitoを使う時もAssumeRoleWithWebIdentity APIを叩いている Cognito推奨）
Google・Amazon・Facebook認証とかで使用する。


### エンタープライズIDフェデレーション
組織のネットワークのユーザーを認証し、ユーザーが AWS にアクセス可能にすることができる。
Microsoft Active Directory を活用できる。
#### カスタムフェデレーションブローカー
※社内でSAML 2.0をサポートしていない社内のID認証システムを使う場合（LDAP（Lightweight Directory Access Protocol）による認証など）
組織の認証システムを使用して AWS リソースへのアクセスを許可することができる。

1.AssumeRole
ユーザーを認証するためのカスタムIDブローカーを作成し。AssumeRoleを用いてAWSリソースへのアクセスに使用できる一時的なセキュリティ認証情報のセットを取得し、AWSリソースにアクセス。
これらの一時的な資格情報は、アクセスキーID、シークレットアクセスキー、およびセキュリティトークンで構成される。
通常、AssumeRoleはアカウント内またはクロスアカウントアクセスに使用します

2.GetFederationToken
GetFederationTokenではフェデレーションユーザーの一連の一時的なセキュリティ資格情報（アクセスキーID、シークレットアクセスキー、およびセキュリティトークンで構成される）を返します。IAMユーザーの長期的なセキュリティ認証情報を使用してGetFederationTokenオペレーションを呼び出す必要があります。 その結果、この呼び出しにより、サーバーベースのアプリケーションにおいて、これらの資格情報を安全に保存することができます
AssumeRoleと違ってデフォルトの有効期限が大幅に長い（12時間）

#### SAML2.0を用いたフェデレーション
組織の認証システムとSAMLを使用して、AWSリソースへのアクセスを許可することができる。

AssumeRoleWithSAML

##### IAM ID プロバイダー
外部 ID プロバイダー (IdP) サービスとの連携を設定する場合、IAM IDプロバイダーを作成し、IdPとその設定について AWS に通知します。これにより、AWS アカウントと IdP の間の「信頼」が確立される。

コンソール・CLI・APIで作成可能

# AWS SSO
STSを使ってSSOを実現するのではなくより簡単にSSOを実現する。導入に必要なタスクがより少なくなった

AWS SSO内にADをデフォルトで作れる（外部のADにも接続できる）


- AWS Organizations マスターアカウントから、ユーザー毎のアクセス権限を⼀元管理


### ウェブIDフェデレーション
プロバイダーの認証情報を AWS アカウントのリソースを使用するための一時的なアクセス許可に変換することができる。
Facebook、Google、および任意の OpenID Connect (OIDC) 互換の ID プロバイダーがサポートされている。

# AWS Resource Access Manager(AWS RAM)
所有する特定のAWSリソースを他のAWSアカウントと共有できます。AWS OrganizationsでTrusted Accessを有効にするにはAWS RAM CLIから、enable-sharing-with-aws-organizationsコマンドを使用します。

# ACM
リージョン単位で証明書を発行する

# AWS CloudHSM
AWS上に利用者専用のハードウェアがプロビジョニングされ、サーバーを含めて暗号化キーを利用者が管理することができる。
ハードウェアセキュリティモジュール (HSM) では、不正使用防止策の施されたハードウェアデバイス内での、安全なキー保管と暗号化操作が可能になる。HSM は暗号キーデータを安全に保存し、ハードウェアの暗号境界の外側からは見えないようにキーデータを使用できるよう設計されている。

# AWS Shield
## AWS Shield Standard
インフラストラクチャ (レイヤー 3 および 4) を標的とするすべての既知の攻撃を総合的に保護できる。
自動的に適用されていて追加料金なし
## AWS Shield Advanced
レイヤー7の攻撃も防ぐことができる（DDos）
Amazon CloudWatch でのほぼリアルタイムの通知と、AWS WAFとAWS Shieldマネジメントコンソールあるいは API での詳細な診断によって、DDoS 攻撃に対する完全な可視性を与えてくれる。
AWS WAF and AWS Shieldマネジメントコンソールから以前の攻撃の要約を表示することもできる。

# CloudHSM
クラウドベースのハードウェアセキュリティモジュール (HSM) 
クラウドで暗号化キーを簡単に生成して使用できるようになります。
CloudHSM で、FIPS 140-2のレベル3認証済みのHSMを使用して、暗号化キーを管理できる。

# AWS Organizations
## SCP
制御できるのは各リソースに対しての権限の最大範囲（S3に対する読み取り・書き込み・管理全て）だけで、別にIAMグループ＋IAMロールとかでS3読み取りのみの制限を加える必要がある。

# AWS Cognito
## ユーザープール
認証・認可を制御するためのメイン機能
Cognitoユーザーの管理・サインインサインアップ機能・外部IDプロバイダーやcognito発行のトークン管理などを行う。

Amazon Cognito のユーザディレクトリ
ユーザープールを使用すると、ユーザーは Amazon Cognito を通じてウェブまたはモバイルアプリにログインできる。
また、ユーザーは Google、Facebook、Amazon、Apple などのソーシャル ID プロバイダー、および SAML ベースの ID プロバイダー経由でユーザープールにサインインすることもできる。

## フェデレーティッドアイデンティティ(IDプール)
ユーザープールのアカウントに対して、IAMロールの付与を行う機能

## Cognito Sync
IDプールで管理されるユーザー単位に、デバイス間でアプリケーションデータを同期する機能
新規で開発する場合、AWS AppSyncというがあるので現在非推奨

# Directory Service
## Simple AD
比較的小規模かつ的コストのマネージド型ユーザーディレクトリが必要な場合に利用できる
AWS上に独立したドメインを作成し、Samba4プロトコルに対応したActive Directory互換のディレクトリとして利用できる。
## AD Connector
オンプレミス環境やAWS上にある既存のディレクトリサービスへの認証プロキシのような役割を果たす。
ユーザー情報を保持するわけではない。
AD Connectorを経由することで既存のディレクトリサービスを利用して認証処理を行うことが可能

## AWS Directory Service for Microsoft Active Directory(AWS Managed Microsoft AD)
オンプレミスのADと信頼関係を構築できる。
既存のADインフラストラクチャを使用してAD対応ワークロードをAWSクラウドに移行する場合は、AWS Managed Microsoft AD が適当。
AD 対応アプリケーションと AWS アプリケーションにユーザーはオンプレミス AD 認証情報でアクセスでき、そのためのユーザー、グループ、またはパスワードの同期が不要。
たとえば、ユーザーは既存の AD ユーザー名とパスワードを使用して、AWS マネジメントコンソールとAmazon WorkSpaces にサインインできます。また、SharePoint などの AD 対応アプリケーションに認証情報の再入力なしでアクセスできるようになる。
[AWS Managed Microsoft ADのユースケース](https://docs.aws.amazon.com/ja_jp/directoryservice/latest/admin-guide/ms_ad_use_cases.html)

# 認証・認可の基本知識
# 仕組み・機能
### SSO
1度のログインにより複数のサービスはアクセスすることができる機能  
[AWS 公式　シングルサインオンの運用](https://d1.awsstatic.com/webinars/jp/pdf/services/20200722_AWSBlackbelt_%E3%82%B7%E3%83%B3%E3%82%B0%E3%83%AB%E3%82%B5%E3%82%A4%E3%83%B3%E3%82%AA%E3%83%B3%E3%81%AE%E8%A8%AD%E8%A8%88%E3%81%A8%E9%81%8B%E7%94%A8.pdf)
### IDフェデレーション
SSOを実現するための一つの手段、それぞれのID管理基盤はそのままに、ネットワークドメインをまたいで互いに“連携”させるもの（今までは社内などの単一ネットワークドメインのだけだった）

## プロトコル
OAuth・OpenID Connect・SAMLはSSOを実現するためのプロトコル

OAuthは認可（権限を与える）のプロトコル
OpenID ConnectやSAMLは認証（身元を明らかにする）のプロトコル

OpenID ConnectはOAuthの拡張規格のためSNSのOAuth認証と同時に利用され
SAMLはActive Directory Federation Services (ADFS) などに利用されている。


