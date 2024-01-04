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

前述のように、そのような情報の開示は脆弱性とはみなされません。しかし、悪用の試みを阻止するために、デバイス動作に必要な情報のみを保存すべきです。

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

IoT デバイスは製造業者の制御空間の外で操作されることがよくあります。さらに、ファームウェアアップデートのリクエストおよび受信やクラウド API へのデータ送信などのために、IoT エコシステム内の他のネットワークノードへの接続を確立する必要があります。そのため、デバイスが何らかの認証情報やシークレットを提供する必要があるかもしれません。これらのシークレットは安全な方法でデバイスに保存し、そのデバイスになりすますために盗まれて使用されることを防ぐ必要があります。

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

機密データやシークレットは暗号化して保存すべきであり、その結果、たとえ攻撃者がメモリにアクセスできたとしても、それぞれのプレーンテキストデータにはアクセスできなくなります。

使用する暗号アルゴリズムの強度は [IOT-MEM-CRYPT-001](#usage-of-weak-cryptographic-algorithms-iot-mem-crypt-001) でカバーされ、このテストケースには関係しません。

**テスト目的**

- デバイスメモリの内容を検索して、プレーンテキスト形式のシークレットが含まれているかどうかを判断しなければなりません。

**対応策**

シークレットは適切な暗号アルゴリズムを使用して保存する必要があります。暗号形式のシークレットのみを保存すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta

このテストケースは [IOT-FW-SCRT-002](../firmware/README.md#unencrypted-storage-of-secrets-iot-fw-scrt-002) をベースとしています。



## 暗号技術 (Cryptography) (IOT-MEM-CRYPT)

多くの IoT デバイスは、機密データの安全な保存、認証目的、他のネットワークノードからの暗号化データの受信と検証などのために、暗号アルゴリズムを実装する必要があります。安全で最先端の暗号技術を実装しないと、機密データの開示、デバイスの誤動作、デバイスの制御不能につながるかもしれません。

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

暗号はさまざまな方法で実装できます。しかし、テクノロジの進化、新しいアルゴリズム、より多くの計算能力が利用できるようになったため、多くの古い暗号アルゴリズムは現在では脆弱または安全でないと考えられています。そのため、新しく強力な暗号アルゴリズムを使用するか、鍵長を増やしたり代替動作モードを使用するなどして既存のアルゴリズムを適応させなければいけません。

脆弱な暗号アルゴリズムを使用すると、攻撃者が特定の暗号テキストからプレーンテキストをタイムリーに復元できるかもしれません。

**テスト目的**

- デバイス上に保存されるデータは暗号化されたデータセグメントの存在をチェックしなければなりません。暗号化されたデータセグメントが見つかった場合、使用する暗号アルゴリズムが特定できるかどうかをチェックしなければなりません。

- さらに、[IOT-MEM-INFO-001](#disclosure-of-source-code-iot-mem-info-001) と [IOT-MEM-INFO-002](#disclosure-of-implementation-details-iot-mem-info-002) をベースとして、ソースコード、設定ファイルなどが特定の暗号アルゴリズムの使用を開示しているかどうかをチェックしなければなりません。

- 暗号アルゴリズムを特定できる場合、BSI による技術ガイドライン [TR-02102-1](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TG02102/BSI-TR-02102-1.pdf?__blob=publicationFile&v=10) などの暗号ガイドラインを参照するなどして、使用するアルゴリズムやその構成がテスト時に十分なレベルのセキュリティを提供しているかどうかを判断しなければなりません。

**対応策**

強力で最先端の暗号アルゴリズムのみを使用すべきです。さらに、これらのアルゴリズムは適切な鍵長や動作モードなどの適切なパラメータを設定することによって、安全な方法で使用しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta

このテストケースは [IOT-FW-CRYPT-001](../firmware/README.md#usage-of-weak-cryptographic-algorithms-iot-fw-crypt-001) をベースとしています。



[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
