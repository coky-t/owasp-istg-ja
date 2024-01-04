# 3.5. 内部インタフェース (Internal Interfaces) (IOT-INT)

## 目次
- [3.5. 内部インタフェース (Internal Interfaces) (IOT-INT)](#35-internal-interfaces-iot-int)
	- [目次](#table-of-contents)
	- [概要](#overview)
	- [認可 (Authorization) (IOT-INT-AUTHZ)](#authorization-iot-int-authz)
	  - [インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface) (IOT-INT-AUTHZ-001)](#unauthorized-access-to-the-interface-iot-int-authz-001)
	  - [権限昇格 (Privilege Escalation) (IOT-INT-AUTHZ-002)](#privilege-escalation-iot-int-authz-002)
	- [情報収集 (Information Gathering) (IOT-INT-INFO)](#information-gathering-iot-int-info)
	  - [実装内容の開示 (Disclosure of Implementation Details) (IOT-INT-INFO-001)](#disclosure-of-implementation-details-iot-int-info-001)
	  - [エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-INT-INFO-002)](#disclosure-of-ecosystem-details-iot-int-info-002)
	  - [ユーザーデータの開示 (Disclosure of User Data) (IOT-INT-INFO-003)](#disclosure-of-user-data-iot-int-info-003)
	- [構成とパッチ管理 (Configuration and Patch Management) (IOT-INT-CONF)](#configuration-and-patch-management-iot-int-conf)
	  - [古いソフトウェアの使用 (Usage of Outdated Software) (IOT-INT-CONF-001)](#usage-of-outdated-software-iot-int-conf-001)
	  - [不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-INT-CONF-002)](#presence-of-unnecessary-software-and-functionalities-iot-int-conf-002)
	- [シークレット (Secrets) (IOT-INT-SCRT)](#secrets-iot-int-scrt)
	  - [機密データへのアクセス (Access to Confidential Data) (IOT-INT-SCRT-001)](#access-to-confidential-data-iot-int-scrt-001)
	- [暗号技術 (Cryptography) (IOT-INT-CRYPT)](#cryptography-iot-int-crypt)
	  - [脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-INT-CRYPT-001)](#usage-of-weak-cryptographic-algorithms-iot-int-crypt-001)
	- [ビジネスロジック (Business Logic) (IOT-INT-LOGIC)](#business-logic-iot-int-logic)
	  - [意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic) (IOT-INT-LOGIC-001)](#circumvention-of-the-intended-business-logic-iot-int-logic-001)
	- [入力バリデーション (Input Validation) (IOT-INT-INPV)](#input-validation-iot-int-inpv)
	  - [不十分な入力バリデーション (Insufficient Input Validation) (IOT-INT-INPV-001)](#insufficient-input-validation-iot-int-inpv-001)
	  - [コードインジェクションやコマンドインジェクション (Code or Command Injection) (IOT-INT-INPV-002)](#code-or-command-injection-iot-int-inpv-002)



## 概要

このセクションにはコンポーネントの内部インタフェースに関するテストケースとカテゴリが含まれます。処理装置やメモリと同様に、内部インタフェースは *PA-4* でのみアクセスできるデバイス内部要素です。内部インタフェースへの直接接続を確立するには特定のハードウェア機器 (デバッグボード、オシロスコープ、テストプローブなど) を必要とすることがあります。

内部インタフェースに関連するテストケースカテゴリには、以下のものが特定されています。

- **認可 (Authorization):** 内部インタフェースへの認可されていないアクセスや制限された機能にアクセスするために権限を昇格することを可能にする脆弱性に焦点を当てています。

- **情報収集 (Information Gathering):** 内部インタフェースによって処理され、適切に保護または削除されていない場合に潜在的な攻撃者に開示される可能性がある情報に焦点を当てています。

- **構成とパッチ管理 (Configuration and Patch Management):** 内部インタフェースとそのソフトウェアコンポーネントの構成における脆弱性と問題に焦点を当てています。

- **シークレット (Secrets):** 内部インタフェースによって安全でない方法で処理されているシークレットに焦点を当てています。

- **暗号技術 (Cryptography):** 暗号実装の脆弱性に焦点を当てています。

- **ビジネスロジック (Business Logic):** 内部インタフェースの実装における脆弱性に焦点を当てています。

- **入力バリデーション (Input Validation):** 信頼できないソースからの入力のバリデーションと処理に関する脆弱性に焦点を当てています。



## 認可 (Authorization) (IOT-INT-AUTHZ)

特定のデバイスのアクセスモデルによっては、特定の個人のみが内部インタフェースへのアクセスを許可されるかもしれません。そのため、適切な認証と認可の手順を設け、認可されたユーザーのみがアクセスできるようにする必要があります。

### インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface) (IOT-INT-AUTHZ-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、内部インタフェースへのアクセスは特定の認可アクセスレベル (*AA-2*, *AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。デバイスがアクセスパーミッションを正しく検証できない場合、攻撃者 (*AA-1*) がアクセスできるかもしれません。

**テスト目的**

- 内部インタフェースへのアクセスに対する認可チェックが実装されているかどうかをチェックしなければなりません。

- 認可チェックが行われている場合、それをバイパスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、内部インタフェースへのアクセスは認可された個人のみが可能であることを確保します。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-AUTHZ-001](../data_exchange_services/README.md#unauthorized-access-to-the-data-exchange-service-iot-des-authz-001) をベースとしています。

### 権限昇格 (Privilege Escalation) (IOT-INT-AUTHZ-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-2</i> - <i>AA-3</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、内部インタフェースを介した一部の機能へのアクセスは特定の認可アクセスレベル (*AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。インタフェースがアクセスパーミッションを正しく検証できない場合、意図したより低い認可アクセスレベルを持つ攻撃者が制限された機能にアクセスできるかもしれません。

**テスト目的**

- [IOT-INT-AUTHZ-001](#unauthorized-access-to-the-interface-iot-int-authz-001) をベースとして、与えられたアクセス権限を昇格して制限された機能にアクセスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、制限された機能へのアクセスは必要な認可アクセスレベルを持つ個人のみが可能であることを確保します。

**参考情報**

このテストケースは [IOT-DES-AUTHZ-002](../data_exchange_services/README.md#privilege-escalation-iot-des-authz-002) をベースとしています。



## 情報収集 (Information Gathering) (IOT-INT-INFO)

内部インタフェースはさまざまな情報を開示する可能性があり、デバイスの内部動作や周囲の IoT エコシステムに関する詳細を潜在的な攻撃者に明らかにする可能性があります。これにより、さらに高度な攻撃が可能になり、より容易になります。

### 実装内容の開示 (Disclosure of Implementation Details) (IOT-INT-INFO-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

使用されているアルゴリズムや認証手順など、実装に関する詳細が潜在的な攻撃者に入手されると、攻撃を成功させるための欠陥やエントリポイントを検出しやすくなります。このような詳細の開示だけでは脆弱性とはみなされませんが、潜在的な攻撃ベクトルの特定が容易になり、攻撃者が安全でない実装をより早く悪用できるようになります。

たとえば、関連情報がサービスバナー、コンソール出力、エラーメッセージに含まれるかもしれません。

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

### エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-INT-INFO-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br> 特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

内部インタフェースは、機密性の高い URL、IP アドレス、使用しているソフトウェアなど、周囲の IoT エコシステムに関する情報を開示するかもしれません。攻撃者はこの情報を使用して、エコシステムに対する攻撃を準備して実行できるかもしれません。

たとえば、関連情報がサービスバナー、コンソール出力、エラーメッセージに含まれるかもしれません。

**テスト目的**

- 内部インタフェースが周囲のエコシステムに関する関連情報を開示するかどうかを判断しなければなりません。

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

### ユーザーデータの開示 (Disclosure of User Data) (IOT-INT-INFO-003)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
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



## 構成とパッチ管理 (Configuration and Patch Management) (IOT-INT-CONF)

IoT デバイスは存続期間が長いため、最新のセキュリティパッチを適用して、デバイスで実行しているソフトウェアが定期的に更新されていることを確保することが重要です。ファームウェア自体の更新プロセスは [IOT-FW[UPDT]](../firmware/firmware_update_mechanism.md) でカバーされます。なお、デバイス上で実行され、インタフェースで受信するソフトウェアパッケージが最新であることも検証しなければなりません。

### 古いソフトウェアの使用 (Usage of Outdated Software) (IOT-INT-CONF-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
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

### 不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-INT-CONF-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
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



## シークレット (Secrets) (IOT-INT-SCRT)

IoT デバイスは製造業者の制御空間の外で操作されることがよくあります。さらに、ファームウェアアップデートのリクエストおよび受信やクラウド API へのデータ送信などのために、IoT エコシステム内の他のネットワークノードへの接続を確立する必要があります。そのため、デバイスが何らかの認証情報やシークレットを提供する必要があるかもしれません。これらのシークレットは安全な方法でデバイスに保存し、そのデバイスになりすますために盗まれて使用されることを防ぐ必要があります。

### 機密データへのアクセス (Access to Confidential Data) (IOT-INT-SCRT-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

内部インタフェースの誤動作、意図しない挙動、不適切な実装により、攻撃者がシークレットにアクセスできるかもしれません。

**テスト目的**

- 内部インタフェースを介してシークレットにアクセスできるかどうかを判断する必要があります。

**対応策**

シークレットへのアクセスは、そのシークレットにアクセスする必要がある個人およびプロセスにのみ許可すべきです。認可されていない個人や適切な認可がない個人はシークレットにアクセスできるべきではありません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-SCRT-001](../data_exchange_services/README.md#access-to-confidential-data-iot-des-scrt-001) をベースとしています。



## 暗号技術 (Cryptography) (IOT-INT-CRYPT)

多くの IoT デバイスは、機密データの安全な保存、認証目的、他のネットワークノードからの暗号化データの受信と検証などのために、暗号アルゴリズムを実装する必要があります。安全で最先端の暗号技術を実装しないと、機密データの開示、デバイスの誤動作、デバイスの制御不能につながるかもしれません。

### 脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-INT-CRYPT-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

暗号はさまざまな方法で実装できます。しかし、テクノロジの進化、新しいアルゴリズム、より多くの計算能力が利用できるようになったため、多くの古い暗号アルゴリズムは現在では脆弱または安全でないと考えられています。そのため、新しく強力な暗号アルゴリズムを使用するか、鍵長を増やしたり代替動作モードを使用するなどして既存のアルゴリズムを適応させなければいけません。

脆弱な暗号アルゴリズムを使用すると、攻撃者が特定の暗号テキストからプレーンテキストをタイムリーに復元できるかもしれません。

**テスト目的**

- インタフェースによって処理されるデータは暗号化されたデータセグメントの存在をチェックしなければなりません。暗号化されたデータセグメントが見つかった場合、使用する暗号アルゴリズムが特定できるかどうかをチェックしなければなりません。

- さらに、[IOT-INT-INFO-001](#disclosure-of-implementation-details-iot-int-info-001) をベースとして、ヘッダ、システムメッセージなどが特定の暗号アルゴリズムの使用を開示しているかどうかをチェックしなければなりません。

- 暗号アルゴリズムを特定できる場合、BSI による技術ガイドライン [TR-02102-1](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TG02102/BSI-TR-02102-1.pdf?__blob=publicationFile&v=10) などの暗号ガイドラインを参照するなどして、使用するアルゴリズムやその構成がテスト時に十分なレベルのセキュリティを提供しているかどうかを判断しなければなりません。

**対応策**

強力で最先端の暗号アルゴリズムのみを使用すべきです。さらに、これらのアルゴリズムは適切な鍵長や動作モードなどの適切なパラメータを設定することによって、安全な方法で使用しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-FW-CRYPT-001](../firmware/README.md#usage-of-weak-cryptographic-algorithms-iot-fw-crypt-001) をベースとしています。



## ビジネスロジック (Business Logic) (IOT-INT-LOGIC)

内部インタフェースの他のすべての側面が安全に実装および構成されていたとしても、基盤となるロジック自体に問題があると、デバイスが攻撃に対して脆弱になるかもしれません。そのため、内部インタフェースとその機能が意図したように動作しているかどうか、例外を検出して適切に処理しているかどうかを検証しなければなりません。

### 意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic) (IOT-INT-LOGIC-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

ビジネスロジックの実装に欠陥があると、デバイスの意図しない動作や誤動作を引き起こすかもしれません。たとえば、攻撃者が意図的に関連する入力データの提供しなかったり、処理ワークフローの重要なステップをスキップしたり変更しようとすると、デバイスは未知の潜在的に安全でない状態になるかもしれません。

**テスト目的**

- 特定のビジネスロジックの実装に基づき、定義されたワークフローからの逸脱が適切に検出され処理されるかどうかを判断する必要があります。

**対応策**

デバイスは未知の状態に陥るべきではありません。ワークフローの異常を検出しなければならず、例外を適切に処理する必要があります。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-LOGIC-001](../data_exchange_services/README.md#circumvention-of-the-intended-business-logic-iot-des-logic-001) をベースとしています。



## 入力バリデーション (Input Validation) (IOT-INT-INPV)

有効かつ整形式のデータのみをデバイスの処理フローに入力することを確保するために、ユーザーや外部システムなどのすべての信頼できないソースからの入力を検証およびバリデートする必要があります。

### 不十分な入力バリデーション (Insufficient Input Validation) (IOT-INT-INPV-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

入力バリデーションが実行されないか、不十分な入力バリデーションメカニズムしかない場合、攻撃者は任意の不正なデータを送信できるかもしれません。そのため、ユーザー入力を処理するプロセスや別のダウンストリームコンポーネントは、データを処理できないために、適切に動作しなくなるかもしれません。これにより、攻撃者がデバイスの動作を操作したり、デバイスを利用できなくするような誤動作を引き起こす可能性があります。

**テスト目的**

- 内部インタフェースへの入力がバリデートされるかどうかを判断しなければなりません。

- 入力バリデーションメカニズムが実装されている場合、意図したデータ構造と値範囲に準拠しないデータを送信する方法があるかどうかをチェックする必要があります。

**対応策**

デバイスは信頼できないソースからのすべての入力をバリデートする必要があります。不正な入力あるいは無効な入力は、拒否するか、入力をエンコードするなどして適切なデータ構造に変換しなければなりません。ただし、変換時に入力が解釈されたり実行されることがないように確保しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-INPV-001](../data_exchange_services/README.md#insufficient-input-validation-iot-des-inpv-001) をベースとしています。

### コードインジェクションやコマンドインジェクション (Code or Command Injection) (IOT-INT-INPV-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる)</tr>
</table>

**要旨**

入力バリデーションが実行されないか、不十分な入力バリデーションメカニズムしかない場合、攻撃者はコードやコマンドを送信して、システムによって実行されるかもしれません。どのコードやコマンドが潜在的に実行可能であるかは、デバイスと内部インタフェースの特定の実装に完全に依存します。たとえば、考えられるインジェクション攻撃には OS コマンドインジェクションがあります。

**テスト目的**

- [IOT-INT-INPV-001](#insufficient-input-validation-iot-int-inpv-001) をベースとして、コードやコマンドを送信して、システムによって実行できるかどうかをチェックしなければなりません。

**対応策**

デバイスは信頼できないソースからのすべての入力をバリデートする必要があります。不正な入力あるいは無効な入力は、拒否するか、入力をエンコードするなどして適切なデータ構造に変換しなければなりません。ただし、変換時に入力が解釈されたり実行されることがないように確保しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-INPV-002](../data_exchange_services/README.md#code-or-command-injection-iot-des-inpv-002) をベースとしています。



[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
