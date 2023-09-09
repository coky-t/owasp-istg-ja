# テストチェックリスト (Testing Checklist)

以下は評価時にテストする項目のリストです。

注: `ステータス` 列には "Pass", "Fail", "N/A" のような値を設定します。


## 処理装置 (Processing Units) (IOT-PROC)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-PROC-AUTHZ**|**認可 (Authorization)**|||
|IOT-PROC-AUTHZ-001|処理装置への認可されていないアクセス (Unauthorized Access to the Processing Unit)|||
|IOT-PROC-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**IOT-PROC-LOGIC**|**ビジネスロジック (Business Logic)**|||
|IOT-PROC-LOGIC-001|インストラクションの安全でない実装 (Insecure Implementation of Instructions)|||
|**IOT-PROC-SIDEC**|**サイドチャネル攻撃 (Side-Channel Attacks)**|||
|IOT-PROC-SIDEC-001|サイドチャネル攻撃に対する不十分な保護 (Insufficient Protection Against Side-Channel Attacks)|||

## メモリ (Memory) (IOT-MEM)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-MEM-INFO**|**情報収集 (Information Gathering)**|||
|IOT-MEM-INFO-001|ソースコードの開示 (Disclosure of Source Code)|||
|IOT-MEM-INFO-002|実装内容の開示 (Disclosure of Implementation Details)|||
|IOT-MEM-INFO-003|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|IOT-MEM-INFO-004|ユーザーデータの開示 (Disclosure of User Data)|||
|**IOT-MEM-SCRT**|**シークレット (Secrets)**|||
|IOT-MEM-SCRT-001|シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets)|||
|**IOT-MEM-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-MEM-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||

## ファームウェア (Firmware) (IOT-FW)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-FW-INFO**|**情報収集 (Information Gathering)**|||
|IOT-FW-INFO-001|ソースコードの開示 (Disclosure of Source Code)|||
|IOT-FW-INFO-002|実装内容の開示 (Disclosure of Implementation Details)|||
|IOT-FW-INFO-003|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|**IOT-FW-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|IOT-FW-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|IOT-FW-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**IOT-FW-SCRT**|**シークレット (Secrets)**|||
|IOT-FW-SCRT-001|パブリックストレージに保存されたシークレット (Secrets Stored in Public Storage)|||
|IOT-FW-SCRT-002|シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets)|||
|IOT-FW-SCRT-003|ハードコードされたシークレットの使用 (Usage of Hardcoded Secrets)|||
|**IOT-FW-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-FW-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||

### インストール済みファームウェア (Installed Firmware) (IOT-FW[INST])
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-FW[INST]-AUTHZ**|**認可 (Authorization)**|||
|IOT-FW[INST]-AUTHZ-001|ファームウェアへの認可されていないアクセス (Unauthorized Access to the Firmware)|||
|IOT-FW[INST]-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**IOT-FW[INST]-INFO**|**情報収集 (Information Gathering)**|||
|IOT-FW[INST]-INFO-001|ユーザーデータの開示 (Disclosure of User Data)|||
|**IOT-FW[INST]-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-FW[INST]-CRYPT-001|ブートローダーシグネチャの不十分な検証 (Insufficient Verification of the Bootloader Signature)|||

### ファームウェア更新メカニズム (Firmware Update Mechanism) (IOT-FW[UPDT])
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-FW[UPDT]-AUTHZ**|**認可 (Authorization)**|||
|IOT-FW[UPDT]-AUTHZ-001|認可されていないファームウェア更新 (Unauthorized Firmware Update)|||
|**IOT-FW[UPDT]-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-FW[UPDT]-CRYPT-001|不十分なファームウェア更新シグネチャ (Insufficient Firmware Update Signature)|||
|IOT-FW[UPDT]-CRYPT-002|不十分なファームウェア更新暗号化 (Insufficient Firmware Update Encryption)|||
|IOT-FW[UPDT]-CRYPT-003|ファームウェア更新の安全でない転送 (Insecure Transmission of the Firmware Update)|||
|IOT-FW[UPDT]-CRYPT-004|ファームウェア更新シグネチャの不十分な検証 (Insufficient Verification of the Firmware Update Signature)|||
|**IOT-FW[UPDT]-LOGIC**|**ビジネスロジック (Business Logic)**|||
|IOT-FW[UPDT]-LOGIC-001|不十分なロールバック保護 (Insufficient Rollback Protection)|||

## データ交換サービス (Data Exchange Services) (IOT-DES)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-DES-AUTHZ**|**認可 (Authorization)**|||
|IOT-DES-AUTHZ-001|データ交換サービスへの認可されていないアクセス (Unauthorized Access to the Data Exchange Service)|||
|IOT-DES-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**IOT-DES-INFO**|**情報収集 (Information Gathering)**|||
|IOT-DES-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|IOT-DES-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|IOT-DES-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**IOT-DES-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|IOT-DES-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|IOT-DES-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**IOT-DES-SCRT**|**シークレット (Secrets)**|||
|IOT-DES-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**IOT-DES-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-DES-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**IOT-DES-LOGIC**|**ビジネスロジック (Business Logic)**|||
|IOT-DES-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**IOT-DES-INVAL**|**入力バリデーション (Input Validation)**|||
|IOT-DES-INVAL-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|IOT-DES-INVAL-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## 内部インタフェース (Internal Interfaces) (IOT-INT)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-INT-AUTHZ**|**認可 (Authorization)**|||
|IOT-INT-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|IOT-INT-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**IOT-INT-INFO**|**情報収集 (Information Gathering)**|||
|IOT-INT-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|IOT-INT-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|IOT-INT-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**IOT-INT-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|IOT-INT-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|IOT-INT-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**IOT-INT-SCRT**|**シークレット (Secrets)**|||
|IOT-INT-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**IOT-INT-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-INT-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**IOT-INT-LOGIC**|**ビジネスロジック (Business Logic)**|||
|IOT-INT-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**IOT-INT-INVAL**|**入力バリデーション (Input Validation)**|||
|IOT-INT-INVAL-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|IOT-INT-INVAL-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## 物理インタフェース (Physical Interfaces) (IOT-PHY)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-PHY-AUTHZ**|**認可 (Authorization)**|||
|IOT-PHY-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|IOT-PHY-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**IOT-PHY-INFO**|**情報収集 (Information Gathering)**|||
|IOT-PHY-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|IOT-PHY-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|IOT-PHY-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**IOT-PHY-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|IOT-PHY-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|IOT-PHY-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**IOT-PHY-SCRT**|**シークレット (Secrets)**|||
|IOT-PHY-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**IOT-PHY-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-PHY-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**IOT-PHY-LOGIC**|**ビジネスロジック (Business Logic)**|||
|IOT-PHY-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**IOT-PHY-INVAL**|**入力バリデーション (Input Validation)**|||
|IOT-PHY-INVAL-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|IOT-PHY-INVAL-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## 無線インタフェース (Wireless Interfaces) (IOT-WRLS)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-WRLS-AUTHZ**|**認可 (Authorization)**|||
|IOT-WRLS-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|IOT-WRLS-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**IOT-WRLS-INFO**|**情報収集 (Information Gathering)**|||
|IOT-WRLS-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|IOT-WRLS-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|IOT-WRLS-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**IOT-WRLS-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|IOT-WRLS-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|IOT-WRLS-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**IOT-WRLS-SCRT**|**シークレット (Secrets)**|||
|IOT-WRLS-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**IOT-WRLS-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-WRLS-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**IOT-WRLS-LOGIC**|**ビジネスロジック (Business Logic)**|||
|IOT-WRLS-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**IOT-WRLS-INVAL**|**入力バリデーション (Input Validation)**|||
|IOT-WRLS-INVAL-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|IOT-WRLS-INVAL-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## ユーザーインタフェース (User Interfaces) (IOT-UI)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**IOT-UI-AUTHZ**|**認可 (Authorization)**|||
|IOT-UI-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|IOT-UI-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**IOT-UI-INFO**|**情報収集 (Information Gathering)**|||
|IOT-UI-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|IOT-UI-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|IOT-UI-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**IOT-UI-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|IOT-UI-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|IOT-UI-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**IOT-UI-SCRT**|**シークレット (Secrets)**|||
|IOT-UI-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**IOT-UI-CRYPT**|**暗号技術 (Cryptography)**|||
|IOT-UI-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**IOT-UI-LOGIC**|**ビジネスロジック (Business Logic)**|||
|IOT-UI-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**IOT-UI-INVAL**|**入力バリデーション (Input Validation)**|||
|IOT-UI-INVAL-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|IOT-UI-INVAL-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||
