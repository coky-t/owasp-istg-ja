# 3.2. メモリ (Memory) (IOT-MEM)

## 目次
* [概要](#overview)
* [情報収集 (Information Gathering) (IOT-MEM-INFO)](#information-gathering-iot-mem-info)
  * [ソースコードの開示 (Disclosure of Source Code) (IOT-MEM-INFO-001)](#disclosure-of-source-code-iot-mem-info-001)
  * [実装内容の開示 (Disclosure of Implementation Details) (IOT-MEM-INFO-002)](#disclosure-of-implementation-details-iot-mem-info-002)
  * [エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-MEM-INFO-003)](#disclosure-of-ecosystem-details-iot-mem-info-003)
  * [ユーザーデータの開示 (Disclosure of User Data) (IOT-MEM-INFO-004)](#disclosure-of-user-data-iot-mem-info-004)
* [シークレット (Secrets) (IOT-MEM-SCRT)](#secrets-iot-mem-scrt)
  * [シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets) (IOT-MEM-SCRT-001)](#unencrypted-storage-of-secrets-iot-mem-scrt-001)
* [暗号技術 (Cryptography) (IOT-MEM-CRYPT)](#cryptography-iot-mem-crypt)
  * [脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-MEM-CRYPT-001)](#usage-of-weak-cryptographic-algorithms-iot-mem-crypt-001)




## 概要

このセクションにはコンポーネントのメモリに関するテストケースとカテゴリが含まれます。処理装置と同様に、メモリは *PA-4* でのみアクセスできるデバイス内部要素です。メモリへの直接接続を確立するには特定のハードウェア機器 (デバッグボードやテストプローブ) を必要とすることがあります。

メモリに関連するテストケースカテゴリには、以下のものが特定されています。

- **情報収集 (Information Gathering):** メモリチップ上に保存され、適切に保護または削除されていない場合に潜在的な攻撃者に開示される可能性がある情報に焦点を当てています。

- **シークレット (Secrets):** メモリチップ上に安全でない方法で保存されているシークレットに焦点を当てています。

- **暗号技術 (Cryptography):** 暗号実装の脆弱性に焦点を当てています。



## 情報収集 (Information Gathering) (IOT-MEM-INFO)

IoT デバイスのメモリにはさまざまな情報を含む可能性があり、開示された場合、デバイスの内部動作や基盤となる IoT エコシステムに関する詳細を潜在的な攻撃者に明らかにする可能性があります。これにより、さらに高度な攻撃が可能になり、より容易になります。

デバイスメモリのテストはメモリチップに直接アクセスすることで実行されます。したがって、ユーザーアカウントは使用されず (*AA-1*)、侵入的な物理アクセス (*PA-4*) が必要になります。

### ソースコードの開示 (Disclosure of Source Code) (IOT-MEM-INFO-001)
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

コンパイルされていないソースコードが開示されると、試行錯誤的な方法でテストを実行する必要なしでコード内の脆弱性を直接特定できるため、ソフトウェア実装の悪用を加速する可能性があります。さらに、残されたソースコードには、生産的な使用を目的としていない内部開発情報、開発者コメント、ハードコードされた機密データを含むかもしれません。

コンパイルされていないソースコードと同様に、コンパイルされたバイナリも関連情報を開示するかもしれません。しかし、有用なデータを取得するにはリバースエンジニアリングを必要とし、かなりの時間がかかる可能性があります。したがって、テスト担当者は、理想的にはデバイス製造業者と協力して、どのバイナリに解析する価値があるかを評価する必要があります。

**テスト目的**

- コンパイルされていないソースコードがデバイスメモリ内にあるかどうかをチェックしなければなりません。

- コンパイルされていないソースコードが検出された場合、その内容に機密データの存在するかどうかを分析しなければなりません。これは潜在的な攻撃者に有用かもしれません。

- デバイス実装と機密データの処理に関する有用な情報を取得するには、選択したバイナリのリバースエンジニアリングを実行すべきです。

**対応策**

可能であれば、コンパイルされていないソースコードは生産的な使用を目的としたデバイスから削除すべきです。ソースコードを含める必要がある場合は、デバイスをリリースする前にすべての内部開発データが削除されていることを検証しなければなりません。

リバースエンジニアリングを完全に防ぐことは不可能であるため、デバイスメモリへのアクセスを全般的に制限する対策を実装して、攻撃対象領域を減らすべきです。さらに、コードを難読化することなどで、リバースエンジニアリングプロセスを邪魔できます。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta

This test case is based on: [IOT-FW-INFO-001](../firmware/README.md#disclosure-of-source-code-iot-fw-info-001).

### 実装内容の開示 (Disclosure of Implementation Details) (IOT-MEM-INFO-002)
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

使用されているアルゴリズムや認証手順など、実装に関する詳細が潜在的な攻撃者に入手されると、攻撃を成功させるための欠陥やエントリポイントを検出しやすくなります。このような詳細の開示だけでは脆弱性とはみなされませんが、潜在的な攻撃ベクトルの特定が容易になり、攻撃者が安全でない実装をより早く悪用できるようになります。

**テスト目的**

- さらなるテストを準備するために、実装に関するアクセス可能な詳細を評価しなければなりません。たとえば、これには以下があります。
  - 使用されている暗号アルゴリズム

  - 認証および認可のメカニズム

  - ローカルパスと環境の詳細


**対応策**

前述のように、そのような情報の開示は脆弱性とはみなされません。しかし、悪用の試みを阻止するために、デバイス操作に必要な情報のみを保存すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta

このテストケースは [IOT-FW-INFO-002](../firmware/README.md#disclosure-of-implementation-details-iot-fw-info-002) をベースとしています。

### エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-MEM-INFO-003)
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

デバイスメモリの内容は、機密性の高い URL、IP アドレス、使用しているソフトウェアなど、周囲の IoT エコシステムに関する情報を開示するかもしれません。攻撃者はこの情報を使用して、エコシステムに対する攻撃を準備して実行できるかもしれません。

たとえば、関連情報が設定ファイルやテキストファイルなどのさまざまなタイプのファイルに含まれるかもしれません。

**テスト目的**

- 設定ファイルなど、デバイスメモリに保存されるデータが周囲のエコシステムに関する関連情報を含むかどうかを判断しなければなりません。

**対応策**

情報の開示はデバイスの操作に必要な最小限に抑えるべきです。開示された情報は評価され、含まれている不要なデータをすべて削除すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta

このテストケースは [IOT-FW-INFO-003](../firmware/README.md#disclosure-of-ecosystem-details-iot-fw-info-003) をベースとしています。

### ユーザーデータの開示 Disclosure of User Data (IOT-MEM-INFO-004)
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

実行時に、デバイスはそのユーザーの個人データなどのさまざまな種類のデータを蓄積して処理しています。このデータが安全に保存されないと、攻撃者はデバイスからデータを復元できるかもしれません。

**テスト目的**

- ユーザーデータが認可されていない個人によってアクセスできるかどうかをチェックする必要があります。

**対応策**

ユーザーデータへのアクセスは、そのデータにアクセスする必要がある個人およびプロセスにのみ許可されるべきです。認可されていない個人や適切な認可がない個人はユーザーデータにアクセスできるべきではありません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta

このテストケースは [IOT-FW[INST]-INFO-001](../firmware/installed_firmware.md#disclosure-of-user-data-iot-fw[inst]-info-001) をベースとしています。



## シークレット (Secrets) (IOT-MEM-SCRT)

IoT devices are often operated outside of the control space their manufacturer. Still, they need to establish connections to other network nodes within the IoT ecosystem, e.g., to request and receive firmware updates or to send data to a cloud API. Hence, it might be required that the device can provide some kind of authentication credential or secret. These secrets need to be stored on the device in a secure manner to prevent them from being stolen and used to impersonate the device.

### シークレットの暗号化無しでの保存 (Unencrypted Storage of Secrets) (IOT-MEM-SCRT-001)
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

Sensitive data and secrets should be stored in an encrypted manner, so that even if an attacker has managed to get access to the memory, he has no access to the respective plaintext data.

The strength of the cryptographic algorithms in use will be covered by [IOT-MEM-CRYPT-001](#usage-of-weak-cryptographic-algorithms-iot-mem-crypt-001) and has no relevance for this test case.

**テスト目的**

- By searching the contents of the device memory, it must be determined whether it includes secrets in plaintext form.

**対応策**

Secrets have to be stored using proper cryptographic algorithms. Only the encrypted form of the secret should be stored.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta

This test case is based on: [IOT-FW-SCRT-002](../firmware/README.md#unencrypted-storage-of-secrets-iot-fw-scrt-002).



## 暗号技術 (Cryptography) (IOT-MEM-CRYPT)

Many IoT devices need to implement cryptographic algorithms, e.g., to securely store sensitive data, for authentication purposes or to receive and verify encrypted data from other network nodes. Failing to implement secure, state of the art cryptography might lead to the exposure of sensitive data, device malfunctions or loss of control over the device.

### 脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-MEM-CRYPT-001)
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

Cryptography can be implemented in various ways. However, due to evolving technologies, new algorithms and more computing power becoming available, many old cryptographic algorithms are nowadays considered weak or insecure. Thus, either new and stronger cryptographic algorithms have to be used or existing algorithms must be adapted, e.g., by increasing the key length or using alternative modes of operation.

The usage of weak cryptographic algorithms might allow an attacker to recover the plaintext from a given ciphertext in a timely manner.

**テスト目的**

- The data, stored on the device, must be checked for the presence of encrypted data segments. In case that encrypted data segments are found, it must be checked whether the cryptographic algorithms in use can be identified.

- Furthermore, based on [IOT-MEM-INFO-001](#disclosure-of-source-code-iot-mem-info-001) and [IOT-MEM-INFO-002](#disclosure-of-implementation-details-iot-mem-info-002), it must be checked whether any source code, configuration files etc. disclose the usage of certain cryptographic algorithms.

- In case that cryptographic algorithms can be identified, it must be determined whether the algorithms in use and their configuration are providing a sufficient level of security at the time of testing, e.g., by consulting cryptography guidelines like the technical guideline [TR-02102-1](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TG02102/BSI-TR-02102-1.pdf?__blob=publicationFile&v=10) by the BSI.

**対応策**

Only strong, state of the art cryptographic algorithms should be used. Furthermore, these algorithms must be used in a secure manner by setting proper parameters, such as an appropriate key length or mode ofoperation.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta

This test case is based on: [IOT-FW-CRYPT-001](../firmware/README.md#usage-of-weak-cryptographic-algorithms-iot-fw-crypt-001).



[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
