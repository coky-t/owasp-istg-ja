# 3.3. ファームウェア (Firmware) (IOT-FW)

## 目次
* [概要](#overview)
* [情報収集 (Information Gathering) (IOT-FW-INFO)](#information-gathering-iot-fw-info)
  * [ソースコードの開示 (Disclosure of Source Code) (IOT-FW-INFO-001)](#disclosure-of-source-code-iot-fw-info-001)
  * [実装内容の開示 (Disclosure of Implementation Details) (IOT-FW-INFO-002)](#disclosure-of-implementation-details-iot-fw-info-002)
  * [エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-FW-INFO-003)](#disclosure-of-ecosystem-details-iot-fw-info-003)
* [構成とパッチ管理 (Configuration and Patch Management) (IOT-FW-CONF)](#configuration-and-patch-management-iot-fw-conf)
  * [古いソフトウェアの使用 (Usage of Outdated Software) (IOT-FW-CONF-001)](#usage-of-outdated-software-iot-fw-conf-001)
  * [不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-FW-CONF-002)](#presence-of-unnecessary-software-and-functionalities-iot-fw-conf-002)
* [シークレット (Secrets) (IOT-FW-SCRT)](#secrets-iot-fw-scrt)
  * [パブリックストレージに保存されたシークレット (Secrets Stored in Public Storage) (IOT-FW-SCRT-001)](#secrets-stored-in-public-storage-iot-fw-scrt-001)
  * [シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets) (IOT-FW-SCRT-002)](#unencrypted-storage-of-secrets-iot-fw-scrt-002)
  * [ハードコードされたシークレットの使用 (Usage of Hardcoded Secrets) (IOT-FW-SCRT-003)](#usage-of-hardcoded-secrets-iot-fw-scrt-003)
* [暗号技術 (Cryptography) (IOT-FW-CRYPT)](#cryptography-iot-fw-crypt)
  * [脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-FW-CRYPT-001)](#usage-of-weak-cryptographic-algorithms-iot-fw-crypt-001)




## 概要

このセクションではコンポーネントのファームウェアとコンポーネントに特化したインストール済みファームウェア ([IOT-FW[INST]](./installed_firmware.md)) とファームウェア更新メカニズム ([IOT-FW[UPDT]](./firmware_update_mechanism.md)) それぞれのテストケースとカテゴリが含まれます。ファームウェアはこのアクセスがどのように実装されるかの詳細に応じて、すべての物理アクセスレベルでアクセスできる可能性があります。

処理装置に関連するテストケースカテゴリには、以下のものが特定されています。

- **情報収集 (Information Gathering):** ファームウェア内に保存され、適切に保護または削除されていない場合に潜在的な攻撃者に開示される可能性がある情報に焦点を当てています。

- **構成とパッチ管理 (Configuration and Patch Management):** ファームウェアとそのソフトウェアコンポーネントの構成における脆弱性と問題に焦点を当てています。

- **シークレット (Secrets):** ファームウェア内に安全でない方法で保存されているシークレットに焦点を当てています。

- **暗号技術 (Cryptography):** 暗号実装の脆弱性に焦点を当てています。

- **認可 (Authorization) (インストール済みファームウェア):** ファームウェアへの認可されていないアクセスや制限された機能にアクセスするために権限を昇格することを可能にする脆弱性に焦点を当てています。

- **ビジネスロジック (Business Logic) (ファームウェア更新プロセス):** ファームウェア更新プロセスの設計と実装における脆弱性に焦点を当てています。

コンポーネント [IOT-FW](./README.md) のすべてのテストケースとカテゴリはこのコンポーネントに特化した仕様には関係なく、一般的なファームウェア解析の側面に焦点を当てています。



## 情報収集 (Information Gathering) (IOT-FW-INFO)

IoT デバイスのファームウェアはさまざまな情報を含む可能性があり、開示された場合、デバイスの内部動作や周囲の IoT エコシステムに関する詳細を潜在的な攻撃者に明らかにする可能性があります。これにより、さらに高度な攻撃が可能になり、より容易になります。

### ソースコードの開示 (Disclosure of Source Code) (IOT-FW-INFO-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(どのコンポーネントの特殊性をテストすべきか、およびアクセス方法による)</td>
	</tr>
</table>

**要旨**

コンパイルされていないソースコードが開示されると、試行錯誤的な方法でテストを実行する必要なしでコード内の脆弱性を直接特定できるため、ソフトウェア実装の悪用を加速する可能性があります。さらに、残されたソースコードには、生産的な使用を目的としていない内部開発情報、開発者コメント、ハードコードされた機密データを含むかもしれません。

コンパイルされていないソースコードと同様に、コンパイルされたバイナリも関連情報を開示するかもしれません。しかし、有用なデータを取得するにはリバースエンジニアリングを必要とし、かなりの時間がかかる可能性があります。したがって、テスト担当者は、理想的にはファームウェア製造業者と協力して、どのバイナリに解析する価値があるかを評価する必要があります。

**テスト目的**

- コンパイルされていないソースコードがファームウェア内にあるかどうかをチェックしなければなりません。

- コンパイルされていないソースコードが検出された場合、その内容に機密データの存在するかどうかを分析しなければなりません。これは潜在的な攻撃者に有用かもしれません ([IOT-FW-SCRT-003](#usage-of-hardcoded-secrets-iot-fw-scrt-003) も参照してください)。

- ファームウェア実装と機密データの処理に関する有用な情報を取得するには、選択したバイナリのリバースエンジニアリングを実行すべきです。

**対応策**

可能であれば、コンパイルされていないソースコードは生産的な使用を目的としたファームウェアから削除すべきです。ソースコードを含める必要がある場合は、ファームウェアをリリースする前にすべての内部開発データが削除されていることを検証しなければなりません。

リバースエンジニアリングを完全に防ぐことは不可能であるため、ファームウェアへのアクセスを全般的に制限する対策を実装して、攻撃対象領域を減らすべきです。さらに、コードを難読化することなどで、リバースエンジニアリングプロセスを邪魔できます。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### 実装内容の開示 (Disclosure of Implementation Details) (IOT-FW-INFO-002)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

使用されているアルゴリズムや認証手順など、実装に関する詳細が潜在的な攻撃者に入手されると、攻撃を成功させるための欠陥やエントリポイントを検出しやすくなります。このような詳細の開示だけでは脆弱性とはみなされませんが、潜在的な攻撃ベクトルの特定が容易になり、攻撃者が安全でない実装をより早く悪用できるようになります。

たとえば、関連情報が設定ファイル、テキストファイル、システム設定、データベースなどさまざまなタイプのファイルに含まれるかもしれません。

**テスト目的**

- さらなるテストを準備するために、実装に関するアクセス可能な詳細を評価しなければなりません。たとえば、これには以下があります。
  - 使用されている暗号アルゴリズム

  - 認証および認可のメカニズム

  - ローカルパスと環境の詳細


**対応策**

前述のように、そのような情報の開示は脆弱性とはみなされません。しかし、悪用の試みを阻止するために、デバイス動作に必要な情報のみをアクセス可能とすべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-FW-INFO-003)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

デバイスファームウェアの内容は、機密性の高い URL、IP アドレス、使用しているソフトウェアなど、周囲の IoT エコシステムに関する情報を開示するかもしれません。攻撃者はこの情報を使用して、エコシステムに対する攻撃を準備して実行できるかもしれません。

たとえば、関連情報が設定ファイルやテキストファイルなどのさまざまなタイプのファイルに含まれるかもしれません。

**テスト目的**

- 設定ファイルなど、ファームウェア (の一部) が周囲のエコシステムに関する関連情報を含むかどうかを判断しなければなりません。

**対応策**

情報の開示はデバイスの操作に必要な最小限に抑えるべきです。開示された情報は評価され、含まれている不要なデータをすべて削除すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



## 構成とパッチ管理 (Configuration and Patch Management) (IOT-FW-CONF)

IoT デバイスは存続期間が長いため、最新のセキュリティパッチを適用して、デバイスで実行しているソフトウェアが定期的に更新されていることを確保することが重要です。ファームウェア自体の更新プロセスは [IOT-FW[UPDT]](../firmware/firmware_update_mechanism.md) でカバーされます。なお、ファームウェアに含まれるソフトウェアパッケージが最新であることも検証しなければなりません。

### 古いソフトウェアの使用 (Usage of Outdated Software) (IOT-FW-CONF-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

すべてのソフトウェアは潜在的に攻撃に対して脆弱です。たとえば、コーディングエラーにより未定義のプログラム動作を引き起こす可能性があり、攻撃者はこれを悪用して、アプリケーションで処理されるデータへのアクセスを獲得したり、実行時環境のコンテキストでアクションを実行する可能性があります。さらに、使用するフレームワーク、ライブラリ、その他のテクノロジの脆弱性も特定のソフトウェアのセキュリティレベルに影響を及ぼすかもしれません。

通常、開発者はソフトウェアに脆弱性が検出されるとアップデートをリリースします。これらのアップデートはできるだけ早くインストールして、攻撃が成功する可能性を減らすべきです。さもないと、攻撃者は既知の脆弱性を使用して、デバイスに対して攻撃を実行する可能性があります。

**テスト目的**

- インストールされたソフトウェアパッケージ、使用しているライブラリおよびフレームワークのバージョン識別子を判断しなければなりません。

- 検出したバージョン識別子に基づいて、ソフトウェア開発者のウェブサイトやパブリックリポジトリを参照するなどして、使用しているソフトウェアバージョンが最新かどうかを判断しなければなりません。

- NIST の [National Vulnerability Database](https://nvd.nist.gov) などの脆弱性データベースを使用して、検出されたソフトウェアバージョンに既知の脆弱性があるかどうかを確認する必要があります。

**対応策**

ファームウェアには古いソフトウェアパッケージを含めるべきではありません。適切なパッチ管理プロセスを実装して、適用可能なアップデートが利用可能になったらインストールされるようにすべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### 不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-FW-CONF-002)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

ファームウェアに含まれるすべてのソフトウェアは、デバイスに対する攻撃実行に使用されるかもしれないため、攻撃対象領域を広げます。インストールされているソフトウェアが最新であっても、公表されていない脆弱性の影響を受けるかもしれません。ソフトウェアプログラムが、特定のファイルやプロセスへのアクセスを提供するなど、脆弱性なしでの攻撃を容易にする可能性もあります。

**テスト目的**

- ファームウェアに含まれるソフトウェアパッケージのリストを作成する必要があります。

- デバイスのドキュメント、その動作、意図したユースケースに基づいて、利用可能な機能のすべてがデバイス動作に必須かどうかを判断しなければなりません。

**対応策**

攻撃対象領域を可能な限り最小限に抑えるために、デバイス動作に必要のないソフトウェアをすべて削除または無効にすべきです。

特に Windows や Linux システムなどの汎用オペレーティングシステムの場合には、不必要なオペレーティングシステム機能がすべて無効であることを確保しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



## シークレット (Secrets) (IOT-FW-SCRT)

IoT devices are often operated outside of the control space of their manufacturer. Still, they need to establish connections to other network nodes within the IoT ecosystem, e.g., to request and receive firmware updates or to send data to a cloud API. Hence, it might be required that the device has to provide some kind of authentication credential or secret. These secrets need to be stored on the device in a secure manner to prevent them from being stolen and used to impersonate the device.

### パブリックストレージに保存されたシークレット (Secrets Stored in Public Storage) (IOT-FW-SCRT-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

Generally, there are multiple kinds of storage spaces within a file system, some of which are publicly available and some that can only be accessed with a certain level of privileges. If sensitive data or secrets are stored in publicly accessible storage spaces, users who should not have access to this data but who have access to the file system could read or modify it. In case of a successful attack, it is very likely that secrets, stored in public storage, are disclosed.

**テスト目的**

- Files and databases within public storage spaces must be checked for the presence of secrets, such as passwords, symmetric or private keys and tokens.

**対応策**

Access to secrets should only be granted to the accounts or processes with proper privileges. Thus, secrets should be stored in protected storage areas or designated key stores that are only available to certain entities.

**参考情報**

For this test case, data from the following available sources was consolidated:

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets) (IOT-FW-SCRT-002)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

Sensitive data and secrets should be stored in an encrypted manner, so that even if an attacker has managed to get access to it, he has no access to the respective plaintext data.

Contrary to [IOT-FW-SCRT-001](#secrets-stored-in-public-storage-iot-fw-scrt-001), it does not matter if the secrets are stored in public or restricted storage spaces, since it is assumed that the attacker has already gotten access to the data, e.g., by circumventing access restrictions or by exploiting a process with access to the restricted storage. Furthermore, the strength of the cryptographic algorithms in use will be covered by [IOT-FW-CRYPT-001](#usage-of-weak-cryptographic-algorithms-iot-fw-crypt-001) and has no relevance for this test case.

**テスト目的**

- By searching public and restricted storage spaces, it must be determined whether the firmware includes secrets in plaintext form.

**対応策**

Secrets have to be stored using proper cryptographic algorithms. Only the encrypted form of the secret should be stored.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### ハードコードされたシークレットの使用 (Usage of Hardcoded Secrets) (IOT-FW-SCRT-003)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

Sometimes, developers tend to incorporate secrets directly into the source code of their software. This can lead to a variety of security issues like:

- the disclosure of secrets via published source code snippets or decompiled source code,

- endangering all devices that are using the given software since it is very likely that the same secret is used on all devices (otherwise, the source code needs to be changed and compiled for every device individually) and

- impeding reactive measures in case of the secret being compromised since changing the secret requires a software update.

**テスト目的**

- Based on [IOT-FW-INFO-001](#disclosure-of-source-code-iot-fw-info-001), it must be checked if any hard-coded secrets can be identified.

**対応策**

Secrets should not be hard-coded into the source code. Instead, secrets should be stored in a secure manner (see [IOT-FW-SCRT-001](#secrets-stored-in-public-storage-iot-fw-scrt-001) and [IOT-FW-SCRT-002](#unencrypted-storage-of-secrets-iot-fw-scrt-002)) and the software process should dynamically retrieve the secrets from the secure storage during runtime.

**参考情報**

For this test case, data from the following available sources was consolidated:

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



## 暗号技術 (Cryptography) (IOT-FW-CRYPT)

Many IoT devices need to implement cryptographic algorithms, e.g., to securely store sensitive data, for authentication purposes or to receive and verify encrypted data from other network nodes. Failing to implement secure, state of the art cryptography might lead to the exposure of sensitive data, device malfunctions or loss of control over the device.

### 脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-FW-CRYPT-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

Cryptography can be implemented in various ways. However, due to evolving technologies, new algorithms and more computing power becoming available, many old cryptographic algorithms are nowadays considered weak or insecure. Thus, either new and stronger cryptographic algorithms have to be used or existing algorithms must be adapted, e.g., by increasing the key length or using alternative modes of operation.

The usage of weak cryptographic algorithms might allow an attacker to recover the plaintext from a given ciphertext in a timely manner.

**テスト目的**

- The data, stored by or within the firmware, must be checked for the presence of encrypted data segments. In case that encrypted data segments are found, it must be checked whether the cryptographic algorithms in use can be identified.

- Furthermore, based on [IOT-FW-INFO-001](#disclosure-of-source-code-iot-fw-info-001) and [IOT-FW-INFO-002](#disclosure-of-implementation-details-iot-fw-info-002), it must be checked whether any source code, configuration files etc. disclose the usage of certain cryptographic algorithms.

- In case that cryptographic algorithms can be identified, it must be determined whether the algorithms in use and their configuration are providing a sufficient level of security at the time of testing, e.g., by consulting cryptography guidelines like the technical guideline [TR-02102-1](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TG02102/BSI-TR-02102-1.pdf?__blob=publicationFile&v=10) by the BSI.

**対応策**

Only strong, state of the art cryptographic algorithms should be used. Furthermore, these algorithms must be used in a secure manner by setting proper parameters, such as an appropriate key length or mode of operation.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



[owasp_fstm]: https://github.com/scriptingxss/owasp-fstm	"OWASP Firmware Security Testing Methodology"
[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
