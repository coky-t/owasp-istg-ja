# 3.7. 無線インタフェース (Wireless Interfaces) (IOT-WRLS)

## 目次
- [3.7. 無線インタフェース (Wireless Interfaces) (IOT-WRLS)](#37-wireless-interfaces-iot-wrls)
	- [目次](#table-of-contents)
	- [概要](#overview)
	- [認可 (Authorization) (IOT-WRLS-AUTHZ)](#authorization-iot-wrls-authz)
	  - [インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface) (IOT-WRLS-AUTHZ-001)](#unauthorized-access-to-the-interface-iot-wrls-authz-001)
	  - [権限昇格 (Privilege Escalation) (IOT-WRLS-AUTHZ-002)](#privilege-escalation-iot-wrls-authz-002)
	- [情報収集 (Information Gathering) (IOT-WRLS-INFO)](#information-gathering-iot-wrls-info)
	  - [実装内容の開示 (Disclosure of Implementation Details) (IOT-WRLS-INFO-001)](#disclosure-of-implementation-details-iot-wrls-info-001)
	  - [エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-WRLS-INFO-002)](#disclosure-of-ecosystem-details-iot-wrls-info-002)
	  - [ユーザーデータの開示 (Disclosure of User Data) (IOT-WRLS-INFO-003)](#disclosure-of-user-data-iot-wrls-info-003)
	- [構成とパッチ管理 (Configuration and Patch Management) (IOT-WRLS-CONF)](#configuration-and-patch-management-iot-wrls-conf)
	  - [古いソフトウェアの使用 (Usage of Outdated Software) (IOT-WRLS-CONF-001)](#usage-of-outdated-software-iot-wrls-conf-001)
	  - [不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-WRLS-CONF-002)](#presence-of-unnecessary-software-and-functionalities-iot-wrls-conf-002)
	- [シークレット (Secrets) (IOT-WRLS-SCRT)](#secrets-iot-wrls-scrt)
	  - [機密データへのアクセス (Access to Confidential Data) (IOT-WRLS-SCRT-001)](#access-to-confidential-data-iot-wrls-scrt-001)
	- [暗号技術 (Cryptography) (IOT-WRLS-CRYPT)](#cryptography-iot-wrls-crypt)
	  - [脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-WRLS-CRYPT-001)](#usage-of-weak-cryptographic-algorithms-iot-wrls-crypt-001)
	- [ビジネスロジック (Business Logic) (IOT-WRLS-LOGIC)](#business-logic-iot-wrls-logic)
	  - [意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic) (IOT-WRLS-LOGIC-001)](#circumvention-of-the-intended-business-logic-iot-wrls-logic-001)
	- [入力バリデーション (Input Validation) (IOT-WRLS-INPV)](#input-validation-iot-wrls-inpv)
	  - [不十分な入力バリデーション (Insufficient Input Validation) (IOT-WRLS-INPV-001)](#insufficient-input-validation-iot-wrls-inpv-001)
	  - [コードインジェクションやコマンドインジェクション (Code or Command Injection) (IOT-WRLS-INPV-002)](#code-or-command-injection-iot-wrls-inpv-002)




## 概要

このセクションにはコンポーネントの無線インタフェースに関するテストケースとカテゴリが含まれます。無線インタフェースは *PA-2*, *PA-3*, *PA-4* でアクセスできます。無線インタフェースへの直接接続を確立するには特定のハードウェア機器 (ドングル、ソフトウェア無線など) を必要とすることがあります。

無線インタフェースに関連するテストケースカテゴリには、以下のものが特定されています。

- **認可 (Authorization):** 無線インタフェースへの認可されていないアクセスや制限された機能にアクセスするために権限を昇格することを可能にする脆弱性に焦点を当てています。

- **情報収集 (Information Gathering):** 無線インタフェースよって処理され、適切に保護または削除されていない場合に潜在的な攻撃者に開示される可能性がある情報に焦点を当てています。

- **構成とパッチ管理 (Configuration and Patch Management):** 無線インタフェースとそのソフトウェアコンポーネントの構成における脆弱性と問題に焦点を当てています。

- **シークレット (Secrets):** 無線インタフェースによって安全でない方法で処理されているシークレットに焦点を当てています。

- **暗号技術 (Cryptography):** 暗号実装の脆弱性に焦点を当てています。

- **ビジネスロジック (Business Logic):** 無線インタフェースの実装における脆弱性に焦点を当てています。

- **入力バリデーション (Input Validation):** 信頼できないソースからの入力のバリデーションと処理に関する脆弱性に焦点を当てています。



## 認可 (Authorization) (IOT-WRLS-AUTHZ)

特定のデバイスのアクセスモデルによっては、特定の個人のみが無線インタフェースへのアクセスを許可されるかもしれません。そのため、適切な認証と認可の手順を設け、認可されたユーザーのみがアクセスできるようにする必要があります。

### インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface) (IOT-WRLS-AUTHZ-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、無線インタフェースへのアクセスは特定の認可アクセスレベル (*AA-2*, *AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。デバイスがアクセスパーミッションを正しく検証できない場合、攻撃者 (*AA-1*) がアクセスできるかもしれません。

**テスト目的**

- 無線インタフェースへのアクセスに対する認可チェックが実装されているかどうかをチェックしなければなりません。

- 認可チェックが行われている場合、それをバイパスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、無線インタフェースへのアクセスは認可された個人のみが可能であることを確保します。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-AUTHZ-001](../data_exchange_services/README.md#unauthorized-access-to-the-data-exchange-service-iot-des-authz-001) をベースとしています。

### 権限昇格 (Privilege Escalation) (IOT-WRLS-AUTHZ-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-2</i> - <i>AA-3</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、無線インタフェースを介した一部の機能へのアクセスは特定の認可アクセスレベル (*AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。インタフェースがアクセスパーミッションを正しく検証できない場合、意図したより低い認可アクセスレベルを持つ攻撃者が制限された機能にアクセスできるかもしれません。

**テスト目的**

- [IOT-WRLS-AUTHZ-001](#unauthorized-access-to-the-interface-iot-wrls-authz-001) をベースとして、与えられたアクセス権限を昇格して制限された機能にアクセスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、制限された機能へのアクセスは必要な認可アクセスレベルを持つ個人のみが可能であることを確保します。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-AUTHZ-002](../data_exchange_services/README.md#privilege-escalation-iot-des-authz-002) をベースとしています。



## 情報収集 (Information Gathering) (IOT-WRLS-INFO)

無線インタフェースはさまざまな情報を開示する可能性があり、デバイスの内部動作や周囲の IoT エコシステムに関する詳細を潜在的な攻撃者に明らかにする可能性があります。これにより、さらに高度な攻撃が可能になり、より容易になります。

### 実装内容の開示 (Disclosure of Implementation Details) (IOT-WRLS-INFO-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

使用されているアルゴリズムや認証手順など、実装に関する詳細が潜在的な攻撃者に入手されると、攻撃を成功させるための欠陥やエントリポイントを検出しやすくなります。このような詳細の開示だけでは脆弱性とはみなされませんが、潜在的な攻撃ベクトルの特定が容易になり、攻撃者が安全でない実装をより早く悪用できるようになります。

たとえば、関連情報がサービスバナー、ブロードキャスト、エラーメッセージに含まれるかもしれません。

**テスト目的**

- さらなるテストを準備するために、実装に関するアクセス可能な詳細を評価しなければなりません。たとえば、これには以下があります。
  - 使用されている暗号アルゴリズム

  - 認証および認可のメカニズム

  - ローカルパスと環境の詳細


**対応策**

前述のように、そのような情報の開示は脆弱性とはみなされません。しかし、悪用の試みを阻止するために、デバイス動作に必要な情報のみを表示すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-FW-INFO-002](../firmware/README.md#disclosure-of-implementation-details-iot-fw-info-002) をベースとしています。

### エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-WRLS-INFO-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

無線インタフェースは、機密性の高い URL、IP アドレス、使用しているソフトウェアなど、周囲の IoT エコシステムに関する情報を開示するかもしれません。攻撃者はこの情報を使用して、エコシステムに対する攻撃を準備して実行できるかもしれません。

たとえば、関連情報がサービスバナー、ブロードキャスト、エラーメッセージに含まれるかもしれません。

**テスト目的**

- 無線インタフェースが周囲のエコシステムに関する関連情報を開示するかどうかを判断しなければなりません。

**対応策**

情報の開示はデバイスの操作に必要な最小限に抑えるべきです。開示された情報は評価され、含まれている不要なデータをすべて削除すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-FW-INFO-003](../firmware/README.md#disclosure-of-ecosystem-details-iot-fw-info-003) をベースとしています。

### ユーザーデータの開示 (Disclosure of User Data) (IOT-WRLS-INFO-003)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

実行時に、デバイスはそのユーザーの個人データなどのさまざまな種類のデータを蓄積して処理しています。このデータが開示されると、攻撃者はデータにアクセスできるかもしれません。

**テスト目的**

- ユーザーデータが認可されていない個人によってアクセスできるかどうかをチェックする必要があります。

**対応策**

ユーザーデータへのアクセスは、そのデータにアクセスする必要がある個人およびプロセスにのみ許可されるべきです。認可されていない個人や適切な認可がない個人はユーザーデータにアクセスできるべきではありません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-FW[INST]-INFO-001](../firmware/installed_firmware.md#disclosure-of-user-data-iot-fw[inst]-info-001) をベースとしています。



## 構成とパッチ管理 (Configuration and Patch Management) (IOT-WRLS-CONF)

IoT デバイスは存続期間が長いため、最新のセキュリティパッチを適用して、デバイスで実行しているソフトウェアが定期的に更新されていることを確保することが重要です。ファームウェア自体の更新プロセスは [IOT-FW[UPDT]](../firmware/firmware_update_mechanism.md) でカバーされます。なお、デバイス上で実行され、インタフェースで受信するソフトウェアパッケージが最新であることも検証しなければなりません。

### 古いソフトウェアの使用 (Usage of Outdated Software) (IOT-WRLS-CONF-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

すべてのソフトウェアは潜在的に攻撃に対して脆弱です。たとえば、コーディングエラーにより未定義のプログラム動作を引き起こす可能性があり、攻撃者はこれを悪用して、アプリケーションで処理されるデータへのアクセスを獲得したり、実行時環境のコンテキストでアクションを実行する可能性があります。さらに、使用するフレームワーク、ライブラリ、その他のテクノロジの脆弱性も特定のソフトウェアのセキュリティレベルに影響を及ぼすかもしれません。

通常、開発者はソフトウェアに脆弱性が検出されるとアップデートをリリースします。これらのアップデートはできるだけ早くインストールして、攻撃が成功する可能性を減らすべきです。さもないと、攻撃者は既知の脆弱性を使用して、デバイスに対して攻撃を実行する可能性があります。

**テスト目的**

- インストールされたソフトウェアパッケージ、使用しているライブラリおよびフレームワークのバージョン識別子を判断しなければなりません。

- 検出したバージョン識別子に基づいて、ソフトウェア開発者のウェブサイトやパブリックリポジトリを参照するなどして、使用しているソフトウェアバージョンが最新かどうかを判断しなければなりません。

- NIST の [National Vulnerability Database](https://nvd.nist.gov) などの脆弱性データベースを使用して、検出されたソフトウェアバージョンに既知の脆弱性があるかどうかを確認する必要があります。

**対応策**

デバイス上で古いソフトウェアパッケージを実行すべきではありません。適切なパッチ管理プロセスを実装して、適用可能なアップデートが利用可能になったらインストールされるようにすべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-FW-CONF-001](../firmware/README.md#usage-of-outdated-software-iot-fw-conf-001) をベースとしています。

### 不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-WRLS-CONF-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

デバイス上で利用可能なすべてのソフトウェアは、デバイスに対する攻撃実行に使用されるかもしれないため、攻撃対象領域を広げます。インストールされているソフトウェアが最新であっても、公表されていない脆弱性の影響を受けるかもしれません。ソフトウェアプログラムが、特定のファイルやプロセスへのアクセスを提供するなど、脆弱性なしでの攻撃を容易にする可能性もあります。

**テスト目的**

- インタフェースを介して利用可能な機能のリストを作成する必要があります。

- デバイスのドキュメント、その動作、意図したユースケースに基づいて、利用可能な機能のすべてがデバイス動作に必須かどうかを判断しなければなりません。

**対応策**

攻撃対象領域を可能な限り最小限に抑えるために、デバイス動作に必要のないソフトウェアをすべて削除または無効にすべきです。

特に Windows や Linux システムなどの汎用オペレーティングシステムの場合には、不必要なオペレーティングシステム機能がすべて無効であることを確保しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-FW-CONF-002](../firmware/README.md#presence-of-unnecessary-software-and-functionalities-iot-fw-conf-002) をベースとしています。



## シークレット (Secrets) (IOT-WRLS-SCRT)

IoT devices are often operated outside of the control space of their manufacturer. Still, they need to establish connections to other network nodes within the IoT ecosystem, e.g., to request and receive firmware updates or to send data to a cloud API. Hence, it might be required that the device has to provide some kind of authentication credential or secret. These secrets need to be stored on the device in a secure manner to prevent them from being stolen and used to impersonate the device.

### 機密データへのアクセス (Access to Confidential Data) (IOT-WRLS-SCRT-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

Malfunctions, unintended behavior or improper implementation of a wireless interface might enable an attacker to get access to secrets.

**テスト目的**

- It has to be determined whether secrets can be accessed via the wireless interface.

**対応策**

Access to secrets should only be granted to individuals and processes that need to have access to them. No unauthorized or not properly authorized individual should be able to access secrets.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-SCRT-001](../data_exchange_services/README.md#access-to-confidential-data-iot-des-scrt-001).



## 暗号技術 (Cryptography) (IOT-WRLS-CRYPT)

Many IoT devices need to implement cryptographic algorithms, e.g., to securely store sensitive data, for authentication purposes or to receive and verify encrypted data from other network nodes. Failing to implement secure, state of the art cryptography might lead to the exposure of sensitive data, device malfunctions or loss of control over the device.

### 脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-WRLS-CRYPT-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

Cryptography can be implemented in various ways. However, due to evolving technologies, new algorithms and more computing power becoming available, many old cryptographic algorithms are nowadays considered weak or insecure. Thus, either new and stronger cryptographic algorithms have to be used or existing algorithms must be adapted, e.g., by increasing the key length or using alternative modes of operation.

The usage of weak cryptographic algorithms might allow an attacker to recover the plaintext from a given ciphertext in a timely manner.

**テスト目的**

- The data, processed by the interface, must be checked for the presence of encrypted data segments. In case that encrypted data segments are found, it must be checked whether the cryptographic algorithms in use can be identified.

- Furthermore, based on [IOT-WRLS-INFO-001](#disclosure-of-implementation-details-iot-wrls-info-001), it must be checked whether headers, system messages etc. disclose the usage of certain cryptographic algorithms.

- In case that cryptographic algorithms can be identified, it must be determined whether the algorithms in use and their configuration are providing a sufficient level of security at the time of testing, e.g., by consulting cryptography guidelines like the technical guideline [TR-02102-1](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TG02102/BSI-TR-02102-1.pdf?__blob=publicationFile&v=10) by the BSI.

**対応策**

Only strong, state of the art cryptographic algorithms should be used. Furthermore, these algorithms must be used in a secure manner by setting proper parameters, such as an appropriate key length or mode of operation.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-FW-CRYPT-001](../firmware/README.md#usage-of-weak-cryptographic-algorithms-iot-fw-crypt-001).



## ビジネスロジック (Business Logic) (IOT-WRLS-LOGIC)

Even if all other aspects of the wireless interface are securely implemented and configured, issues in the underlying logic itself might render the device vulnerable to attacks. Thus, it must be verified if the wireless interface and its functionalities are working as intended and if exceptions are detected and properly handled.

### 意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic) (IOT-WRLS-LOGIC-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

Flaws in the implementation of the business logic might result in unintended behavior or malfunctions of the device. For example, if an attacker intentionally misses to provide relevant input data or tries to skip or change important steps in the processing workflow the device might end up in an unknown, potentially insecure state.

**テスト目的**

- Based on the specific business logic implementation, it has to be determined whether deviations from the defined workflows are properly detected and handled.

**対応策**

The device should not end up in an unknown state. Anomalies in the workflow must be detected and exceptions have to be handled properly.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-LOGIC-001](../data_exchange_services/README.md#circumvention-of-the-intended-business-logic-iot-des-logic-001).



## 入力バリデーション (Input Validation) (IOT-WRLS-INPV)

In order to ensure that only valid and well-formed data enters the processing flows of a device, the input from a all untrustworthy sources, e.g., users or external systems, has to be verified and validated.

### 不十分な入力バリデーション (Insufficient Input Validation) (IOT-WRLS-INPV-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

If no input validation is performed or only an insufficient input validation mechanism is in place an attacker might be able to submit arbitrary and malformed data. Thus, the process, which handles the user input, or another downstream component might stop working properly due to not being able to process the data. This could result in malfunctions that might enable an attacker to manipulate the device behavior or render it unavailable.

**テスト目的**

- It must be determined whether input to the wireless interface is validated.

- In case that an input validation mechanism is implemented, it hast to be checked if there is a way to submit data, which does not comply with the intended data structure and value ranges.

**対応策**

The device has to validate all input from untrustworthy sources. Malformed or otherwise invalid input must either be rejected or converted into a proper data structure, e.g., by encoding the input. However, it must be ensured that the input is not interpreted or executed when converting it.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-INPV-001](../data_exchange_services/README.md#insufficient-input-validation-iot-des-inpv-001).

### コードインジェクションやコマンドインジェクション (Code or Command Injection) (IOT-WRLS-INPV-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-2</i> - <i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

If no input validation is performed or only an insufficient input validation mechanism is in place an attacker might be able to submit code or commands, which then might be executed by the system. It strictly depends on the specific implementation of the device and the ireless interface which code and commands are potentially executable. For example, a possible injection attack is OS command injection.

**テスト目的**

- Based on [IOT-WRLS-INPV-001](#insufficient-input-validation-iot-wrls-inpv-001), it must be checked whether it is possible to submit code or commands, which are then executed by the system.

**対応策**

The device has to validate all input from untrustworthy sources. Malformed or otherwise invalid input must either be rejected or converted into a proper data structure, e.g., by encoding the input. However, it must be ensured that the input is not interpreted or executed when converting it.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-INPV-002](../data_exchange_services/README.md#code-or-command-injection-iot-des-inpv-002).



[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
