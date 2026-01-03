# AWS Cloud Engineering & Architecture Log

## 📖 Overview
AWS認定ソリューションアーキテクト(SAA)取得および、実務でのクラウド活用に向けたハンズオン学習の記録です。
**Infrastructure as Code (IaC)** を用いた、再現性のある環境構築の実践ログを蓄積します。

* **Goal:** Master AWS Cloud Architecture & DevOps practices.
* **Focus:** Serverless, Container, Networking, and CloudFormation.

## 📂 Project Index
| Dir | Topic | Tech Stack |
| :--- | :--- | :--- |
| `00_init` | Repository Setup | Git, GitHub |
| `01_s3_basic` | S3 Bucket (IaC) & CLI Deploy | CloudFormation, AWS CLI |
| `02_s3_advanced` | **S3 Versioning & Lifecycle Rules** | CloudFormation, S3 |
| `03_vpc_basic` | **Custom VPC Networking** | VPC, Subnet, IGW, RouteTable |
| `04_ec2_basic` | **Web Server & Bootstrapping** | EC2, UserData, SecurityGroup |

## 🛠️ Tech Stack
* **Cloud Provider:** Amazon Web Services (AWS)
* **Infrastructure as Code:** AWS CloudFormation
* **Languages:** Python 3.x, Bash
* **Tools:** Git, VS Code, AWS CLI

## 📝 Key Learnings & Troubleshooting Log

### 🔹 Network & Compute (VPC/EC2)
カスタムVPC環境へのWebサーバー構築を通じて得た、実践的なトラブルシューティングの記録。

#### 1. Region Dependency & AMI
* **Issue:** `us-west-1` (N. California) でデプロイする際、東京リージョン等の AMI ID を指定すると `The image id does not exist` エラーが発生。
* **Solution:** AMI ID はリージョン固有であるため、デプロイ先のリージョンに対応した ID を検索して指定する必要がある。
* **Note:** インスタンスタイプもリージョンによって可用性が異なる（例: `us-west-1` では `t2.micro` が無料枠対象外/在庫切れのため `t3.micro` を使用）。

#### 2. Public IP in Custom VPC
* **Issue:** デフォルトVPCと異なり、自作したサブネット内のEC2には自動でパブリックIPが付与されない。
* **Solution:** `ec2.yaml` 内の `NetworkInterfaces` プロパティを使用し、`AssociatePublicIpAddress: true` を明示的に設定する。

#### 3. Bootstrapping Timing
* **Issue:** EC2作成時、IGWへのルート設定が不完全だと `UserData` (起動スクリプト) の `dnf install` が失敗し、サーバーが空の状態で起動する。
* **Solution:** ネットワーク経路（RouteTable）を確立してからEC2を作成する。失敗した場合は再デプロイ（作り直し）が必要。

#### 4. Security Group & Browser Behavior
* **Issue:** Security Groupでポート80 (HTTP) を許可しているのに接続できない。
* **Cause:** ブラウザが自動的に `https://` (ポート443) へリダイレクトしてしまう挙動によるもの。
* **Solution:** 明示的に `http://<Public-IP>` を指定してアクセスする。

---

## 👨‍💻 Author
**Automotive Engineering Manager | CS Master's Degree**
* Currently based in USA (Global Assignment).
* Bridging the gap between Automotive Engineering and Cloud/SDV technologies.