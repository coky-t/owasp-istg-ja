# 3.3.1. インストール済みファームウェア (Installed Firmware) (IOT-FW[INST])

## 目次
* [概要](#overview)
* [認可 (Authorization) (IOT-FW-AUTHZ)](#authorization-iot-fw[inst]-authz)
  * [ファームウェアへの認可されていないアクセス (Unauthorized Access to the Firmware) (IOT-FW-AUTHZ-001)](#unauthorized-access-to-the-firmware-iot-fw[inst]-authz-001)
  * [権限昇格 (Privilege Escalation) (IOT-FW-AUTHZ-002)](#privilege-escalation-iot-fw[inst]-authz-002)
* [情報収集 (Information Gathering) (IOT-FW-INFO)](#information-gathering-iot-fw[inst]-info)
  * [ユーザーデータの開示 (Disclosure of User Data) (IOT-FW-INFO-001)](#disclosure-of-user-data-iot-fw[inst]-info-001)
* [暗号技術 (Cryptography) (IOT-FW-CRYPT)](#cryptography-iot-fw[inst]-crypt)
  * [ブートローダーシグネチャの不十分な検証 (Insufficient Verification of the Bootloader Signature) (IOT-FW-CRYPT-001)](#insufficient-verification-of-the-bootloader-signature-iot-fw[inst]-crypt-001)




## 概要

コンポーネントのファームウェアの特化した一つにはファームウェアのインストール形態であり、動的解析の対象となる可能性があります。動的解析により実行時のデータ処理をテストできます。このようにして、ユーザーデータの処理と保存も解析できます。動的解析の前提条件として、ターゲットファームウェアバージョンを実行しているデバイスを提供しなければなりません。



## 認可 (Authorization) (IOT-FW[INST]-AUTHZ)

通常、管理者などの特定の個人のみが実行時にデバイスファームウェアへのアクセスを許可されるべきです。そのため、適切な認証と認可の手順を設け、認可されたユーザーのみがファームウェアへのアクセスを取得できるようにする必要があります。

### ファームウェアへの認可されていないアクセス (Unauthorized Access to the Firmware) (IOT-FW[INST]-AUTHZ-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、ファームウェアやその機能へのアクセスは特定の認可アクセスレベル (*AA-2*, *AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。デバイスファームウェアがアクセスパーミッションを正しく検証できない場合、攻撃者 (*AA-1*) がファームウェアにアクセスできるかもしれません。

**テスト目的**

- ファームウェアへのアクセスに対する認可チェックが実装されているかどうかをチェックしなければなりません。

- 認可チェックが行われている場合、それをバイパスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、ファームウェアへのアクセスは認可された個人のみが可能であることを確保します。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

このテストケースは [IOT-DES-AUTHZ-001](../data_exchange_services/README.md#unauthorized-access-to-the-data-exchange-service-iot-des-authz-001) をベースとしています。

### 権限昇格 (Privilege Escalation) (IOT-FW[INST]-AUTHZ-002)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-3</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、ファームウェアやその機能の一部へのアクセスは特定の認可アクセスレベル (*AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。デバイスファームウェアがアクセスパーミッションを正しく検証できない場合、意図したより低い認可アクセスレベルを持つ攻撃者が制限されたファームウェアの一部にアクセスできるかもしれません。

**テスト目的**

- *IOT-FW-AUTHZ-001* をベースとして、与えられたアクセス権限を昇格して制限された機能やファームウェアの一部にアクセスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、制限されたファームウェアの一部へのアクセスは必要な認可アクセスレベルを持つ個人のみが可能であることを確保します。

**参考情報**

このテストケースは [IOT-DES-AUTHZ-002](../data_exchange_services/README.md#privilege-escalation-iot-des-authz-002) をベースとしています。



## 情報収集 (Information Gathering) (IOT-FW[INST]-INFO)

前述したように、動的解析では、実行時にユーザーデータがデバイスに安全に保存されているかどうかをテストすることも可能です。

### ユーザーデータの開示 (Disclosure of User Data) (IOT-FW[INST]-INFO-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

実行時に、デバイスはそのユーザーの個人データなどのさまざまな種類のデータを蓄積して処理しています。このデータが安全に保存されないと、攻撃者はデバイスからデータを復元できるかもしれません。

**テスト目的**

- ユーザーデータが認可されていない個人によってアクセスできるかどうかをチェックする必要があります。

**対応策**

ユーザーデータへのアクセスは、そのデータにアクセスする必要がある個人およびプロセスにのみ許可されるべきです。認可されていない個人や適切な認可がない個人はユーザーデータに復元できるべきではありません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



## 暗号技術 (Cryptography) (IOT-FW[INST]-CRYPT)

多くの IoT デバイスは、機密データの安全な保存、認証目的、他のネットワークノードからの暗号化データの受信と検証などのために、暗号アルゴリズムを実装する必要があります。安全で最先端の暗号技術を実装しないと、機密データの開示、デバイスの誤動作、デバイスの制御不能につながるかもしれません。

### ブートローダーシグネチャの不十分な検証 (Insufficient Verification of the Bootloader Signature) (IOT-FW[INST]-CRYPT-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(ファームウェアへのアクセス方法による。たとえば、内部/物理デバッグインタフェースを介してや、リモートで SSH を介して。)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(特定のデバイスのアクセスモデルによる) </td>
	</tr>
</table>

**要旨**

ブートローダーのデジタルシグネチャを検証することはブートローダーの操作を検出するための重要な手段であり、その結果、操作されたファームウェアがデバイス上でされることを防ぎます。

**テスト目的**

- ブートプロセス時に、デバイスがブートローダーのシグネチャを適切に検証しているかどうかをチェックしなければなりません。

**対応策**

デバイスはブートローダーを実行する前に、ブートローダーのデジタルシグネチャを適切に検証しなければなりません。有効なシグネチャのないブートローダーは実行すべきではありません。



[owasp_fstm]: https://github.com/scriptingxss/owasp-fstm	"OWASP Firmware Security Testing Methodology"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
