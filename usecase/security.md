# LDAP（Lightweight Directory Access Protocol）による認証
カスタムIDブローカーによって、LDAPによるユーザー認証を実施することで、ユーザーはAWSリソースにアクセスすることができる。

# Storage gatewayにおけるDDoS攻撃や不正アクセスからの防御
※AWS Storage Gatewayのボリュームゲートウェイを使用し、iSCSIを介したハイブリッド構成のデータ共有システムを利用している場合

iSCSI接続を実施するためにCHAP（Challenge Handshake Authentication Protocol）を構成し、iSCSIとイニシエーター接続を認証することで、ボリュームと VTL（仮想テープライブラリ）デバイスターゲットへのアクセスの認証時に、iSCSI イニシエータのアイデンティティを定期的に確認してプレイバック攻撃から保護することができます。

AWS Storage Gateway はCHAPを使用して、ゲートウェイと iSCSI イニシエータの間の認証を行うことができます。 CHAP は、ボリュームと VTL デバイスターゲットへのアクセスの認証時に、iSCSI イニシエータのアイデンティティを定期的に確認することにより、プレイバック攻撃から保護します。 CHAP を設定するには、AWS Storage Gateway コンソールと、ターゲットへの接続に使用される iSCSI イニシエータソフトウェアの両方で、設定を行う必要があります。Storage Gateway では相互 CHAP が使用され、イニシエータがターゲットを認証するときに、ターゲットもイニシエータを認証します。

## その他の付属情報
### iSCSI
SCSIコマンド、データの転送をIPに変換して通信する方式で、イーサネットのインフラが使用できるため、安価にストレージネットワークを構築でき、FC-SANのような専門知識が必要な管理者が不要
SCSIのネットワークを使った版
### SCSI
コンピュータと周辺機器を接続するための規格のひとつ
SCSIではホスト側をイニシエータ、デバイス側をターゲットと呼ぶ。
### CHAP
CHAP（Challenge Handshake Authentication Protocol）とは、PPPなどで使用されるユーザ認証の一方式

# STSで間違ったアクセス権限をユーザーに渡してしまった時
ユーザーが長いセッションの有効期間 (12 時間など) を使用して AWS マネジメントコンソール にアクセスできるようにすると、時間内であれば一時認証情報がすぐに期限切れになることはない。
取り消すためには、IAMダッシュボードにおいて、ユーザーに提供した特定のロールを選択し、取り消し設定を利用して許可ロールを取り消しをすれば解決

# 外部攻撃からの対策
## Route53での対策
Amazon Route 53 のシャッフルシャーディングとanycastルーティングにより、DDoS 攻撃を受けていても連携してエンドユーザーがアプリケーションにアクセスできるような機能を提供している。

# 認証
## 社内アカウントを使用して既にサインインしているオンプレミスユーザーが、個別のIAMユーザーを作成せずにAWSリソースを管理する場合
SAML 2.0 IDプロバイダーを使用してユーザーに対して、AWSリソースへのフェデレーションアクセスを提供し、オンプレミスのシングルサインオン（SSO）エンドポイントを使用してユーザーを認証し、フェデレーションアクセスを提供する前にアクセストークンを付与することでシングルサインオンを実現することができます

## ソーシャルログイン
AssumeRoleWithWebIdentityはWeb Identity Federation（Facebook、Google、およびその他のソーシャルログイン）専用の認証方式

## 複数アカウントがある場合のリソース共有
AWS Resource Access Manager(RAM)を使用することで、所有する特定のAWSリソースを他のAWSアカウントと共有できる。 AWS OrganizationsでTrusted Accessを有効にするにはAWS RAM CLIから、enable-sharing-with-aws-organizationsコマンドを使用する。

