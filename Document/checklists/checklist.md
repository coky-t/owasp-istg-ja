# テストチェックリスト (Testing Checklist)

以下は評価時にテストする項目のリストです。

注: `ステータス` 列には "Pass", "Fail", "N/A" のような値を設定します。


## 処理装置 (Processing Units) (ISTG-PROC)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-PROC-AUTHZ**|**認可 (Authorization)**|||
|ISTG-PROC-AUTHZ-001|処理装置への認可されていないアクセス (Unauthorized Access to the Processing Unit)|||
|ISTG-PROC-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**ISTG-PROC-LOGIC**|**ビジネスロジック (Business Logic)**|||
|ISTG-PROC-LOGIC-001|インストラクションの安全でない実装 (Insecure Implementation of Instructions)|||
|**ISTG-PROC-SIDEC**|**サイドチャネル攻撃 (Side-Channel Attacks)**|||
|ISTG-PROC-SIDEC-001|サイドチャネル攻撃に対する不十分な保護 (Insufficient Protection Against Side-Channel Attacks)|||

## メモリ (Memory) (ISTG-MEM)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-MEM-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-MEM-INFO-001|ソースコードとバイナリの開示 (Disclosure of Source Code and Binaries)|||
|ISTG-MEM-INFO-002|実装内容の開示 (Disclosure of Implementation Details)|||
|ISTG-MEM-INFO-003|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|ISTG-MEM-INFO-004|ユーザーデータの開示 (Disclosure of User Data)|||
|**ISTG-MEM-SCRT**|**シークレット (Secrets)**|||
|ISTG-MEM-SCRT-001|シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets)|||
|**ISTG-MEM-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-MEM-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||

## ファームウェア (Firmware) (ISTG-FW)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-FW-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-FW-INFO-001|ソースコードとバイナリの開示 (Disclosure of Source Code and Binaries)|||
|ISTG-FW-INFO-002|実装内容の開示 (Disclosure of Implementation Details)|||
|ISTG-FW-INFO-003|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|**ISTG-FW-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|ISTG-FW-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|ISTG-FW-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**ISTG-FW-SCRT**|**シークレット (Secrets)**|||
|ISTG-FW-SCRT-001|パブリックストレージに保存されたシークレット (Secrets Stored in Public Storage)|||
|ISTG-FW-SCRT-002|シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets)|||
|ISTG-FW-SCRT-003|ハードコードされたシークレットの使用 (Usage of Hardcoded Secrets)|||
|**ISTG-FW-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-FW-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||

### インストール済みファームウェア (Installed Firmware) (ISTG-FW[INST])
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-FW[INST]-AUTHZ**|**認可 (Authorization)**|||
|ISTG-FW[INST]-AUTHZ-001|ファームウェアへの認可されていないアクセス (Unauthorized Access to the Firmware)|||
|ISTG-FW[INST]-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**ISTG-FW[INST]-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-FW[INST]-INFO-001|ユーザーデータの開示 (Disclosure of User Data)|||
|**ISTG-FW[INST]-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-FW[INST]-CRYPT-001|ブートローダーシグネチャの不十分な検証 (Insufficient Verification of the Bootloader Signature)|||

### ファームウェア更新メカニズム (Firmware Update Mechanism) (ISTG-FW[UPDT])
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-FW[UPDT]-AUTHZ**|**認可 (Authorization)**|||
|ISTG-FW[UPDT]-AUTHZ-001|認可されていないファームウェア更新 (Unauthorized Firmware Update)|||
|**ISTG-FW[UPDT]-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-FW[UPDT]-CRYPT-001|不十分なファームウェア更新シグネチャ (Insufficient Firmware Update Signature)|||
|ISTG-FW[UPDT]-CRYPT-002|不十分なファームウェア更新暗号化 (Insufficient Firmware Update Encryption)|||
|ISTG-FW[UPDT]-CRYPT-003|ファームウェア更新の安全でない転送 (Insecure Transmission of the Firmware Update)|||
|ISTG-FW[UPDT]-CRYPT-004|ファームウェア更新シグネチャの不十分な検証 (Insufficient Verification of the Firmware Update Signature)|||
|**ISTG-FW[UPDT]-LOGIC**|**ビジネスロジック (Business Logic)**|||
|ISTG-FW[UPDT]-LOGIC-001|不十分なロールバック保護 (Insufficient Rollback Protection)|||

## データ交換サービス (Data Exchange Services) (ISTG-DES)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-DES-AUTHZ**|**認可 (Authorization)**|||
|ISTG-DES-AUTHZ-001|データ交換サービスへの認可されていないアクセス (Unauthorized Access to the Data Exchange Service)|||
|ISTG-DES-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**ISTG-DES-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-DES-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|ISTG-DES-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|ISTG-DES-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**ISTG-DES-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|ISTG-DES-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|ISTG-DES-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**ISTG-DES-SCRT**|**シークレット (Secrets)**|||
|ISTG-DES-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**ISTG-DES-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-DES-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**ISTG-DES-LOGIC**|**ビジネスロジック (Business Logic)**|||
|ISTG-DES-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**ISTG-DES-INPV**|**入力バリデーション (Input Validation)**|||
|ISTG-DES-INPV-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|ISTG-DES-INPV-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## 内部インタフェース (Internal Interfaces) (ISTG-INT)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-INT-AUTHZ**|**認可 (Authorization)**|||
|ISTG-INT-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|ISTG-INT-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**ISTG-INT-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-INT-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|ISTG-INT-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|ISTG-INT-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**ISTG-INT-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|ISTG-INT-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|ISTG-INT-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**ISTG-INT-SCRT**|**シークレット (Secrets)**|||
|ISTG-INT-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**ISTG-INT-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-INT-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**ISTG-INT-LOGIC**|**ビジネスロジック (Business Logic)**|||
|ISTG-INT-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**ISTG-INT-INPV**|**入力バリデーション (Input Validation)**|||
|ISTG-INT-INPV-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|ISTG-INT-INPV-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## 物理インタフェース (Physical Interfaces) (ISTG-PHY)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-PHY-AUTHZ**|**認可 (Authorization)**|||
|ISTG-PHY-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|ISTG-PHY-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**ISTG-PHY-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-PHY-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|ISTG-PHY-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|ISTG-PHY-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**ISTG-PHY-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|ISTG-PHY-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|ISTG-PHY-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**ISTG-PHY-SCRT**|**シークレット (Secrets)**|||
|ISTG-PHY-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**ISTG-PHY-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-PHY-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**ISTG-PHY-LOGIC**|**ビジネスロジック (Business Logic)**|||
|ISTG-PHY-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**ISTG-PHY-INPV**|**入力バリデーション (Input Validation)**|||
|ISTG-PHY-INPV-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|ISTG-PHY-INPV-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## 無線インタフェース (Wireless Interfaces) (ISTG-WRLS)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-WRLS-AUTHZ**|**認可 (Authorization)**|||
|ISTG-WRLS-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|ISTG-WRLS-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**ISTG-WRLS-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-WRLS-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|ISTG-WRLS-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|ISTG-WRLS-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**ISTG-WRLS-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|ISTG-WRLS-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|ISTG-WRLS-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**ISTG-WRLS-SCRT**|**シークレット (Secrets)**|||
|ISTG-WRLS-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**ISTG-WRLS-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-WRLS-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**ISTG-WRLS-LOGIC**|**ビジネスロジック (Business Logic)**|||
|ISTG-WRLS-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**ISTG-WRLS-INPV**|**入力バリデーション (Input Validation)**|||
|ISTG-WRLS-INPV-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|ISTG-WRLS-INPV-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||

## ユーザーインタフェース (User Interfaces) (ISTG-UI)
|テスト ID|テスト名|ステータス|備考|
|-|-|-|-|
|**ISTG-UI-AUTHZ**|**認可 (Authorization)**|||
|ISTG-UI-AUTHZ-001|インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface)|||
|ISTG-UI-AUTHZ-002|権限昇格 (Privilege Escalation)|||
|**ISTG-UI-INFO**|**情報収集 (Information Gathering)**|||
|ISTG-UI-INFO-001|実装内容の開示 (Disclosure of Implementation Details)|||
|ISTG-UI-INFO-002|エコシステム内容の開示 (Disclosure of Ecosystem Details)|||
|ISTG-UI-INFO-003|ユーザーデータの開示 (Disclosure of User Data)|||
|**ISTG-UI-CONF**|**構成とパッチ管理 (Configuration and Patch Management)**|||
|ISTG-UI-CONF-001|古いソフトウェアの使用 (Usage of Outdated Software)|||
|ISTG-UI-CONF-002|不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities)|||
|**ISTG-UI-SCRT**|**シークレット (Secrets)**|||
|ISTG-UI-SCRT-001|機密データへのアクセス (Access to Confidential Data)|||
|**ISTG-UI-CRYPT**|**暗号技術 (Cryptography)**|||
|ISTG-UI-CRYPT-001|脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms)|||
|**ISTG-UI-LOGIC**|**ビジネスロジック (Business Logic)**|||
|ISTG-UI-LOGIC-001|意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic)|||
|**ISTG-UI-INPV**|**入力バリデーション (Input Validation)**|||
|ISTG-UI-INPV-001|不十分な入力バリデーション (Insufficient Input Validation)|||
|ISTG-UI-INPV-002|コードインジェクションやコマンドインジェクション (Code or Command Injection)|||
