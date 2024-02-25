# 3.3.2. ファームウェア更新メカニズム (Firmware Update Mechanism) (ISTG-FW[UPDT])

## 目次
* [概要](#overview)
* [認可 (Authorization) (ISTG-FW-AUTHZ)](#authorization-istg-fw[updt]-authz)
  * [認可されていないファームウェア更新 (Unauthorized Firmware Update) (ISTG-FW-AUTHZ-001)](#unauthorized-firmware-update-istg-fw[updt]-authz-001)
* [暗号技術 (Cryptography) (ISTG-FW-CRYPT)](#cryptography-istg-fw[updt]-crypt)
  * [不十分なファームウェア更新シグネチャ (Insufficient Firmware Update Signature) (ISTG-FW-CRYPT-001)](#insufficient-firmware-update-signature-istg-fw[updt]-crypt-001)
  * [不十分なファームウェア更新暗号化 (Insufficient Firmware Update Encryption) (ISTG-FW-CRYPT-002)](#insufficient-firmware-update-encryption-istg-fw[updt]-crypt-002)
  * [ファームウェア更新の安全でない転送 (Insecure Transmission of the Firmware Update) (ISTG-FW-CRYPT-003)](#insecure-transmission-of-the-firmware-update-istg-fw[updt]-crypt-003)
  * [ファームウェア更新シグネチャの不十分な検証 (Insufficient Verification of the Firmware Update Signature) (ISTG-FW-CRYPT-004)](#insufficient-verification-of-the-firmware-update-signature-istg-fw[updt]-crypt-004)
* [ビジネスロジック (Business Logic) (ISTG-FW-LOGIC)](#business-logic-istg-fw[updt]-logic)
  * [不十分なロールバック保護 (Insufficient Rollback Protection) (ISTG-FW-LOGIC-001)](#insufficient-rollback-protection-istg-fw[updt]-logic-001)



## 概要 <a name="overview"></a>

デバイスファームウェアのもう一つの重要な側面はファームウェア更新メカニズムです。安全な更新メカニズムの実装に失敗すると、攻撃者がデバイスにカスタムの操作されたファームウェアをインストールすることが可能になり、その結果、デバイスを完全に制御できるようになるかもしれません。

以下のカテゴリは特化した [ISTG-FW[UPDT]](./firmware_update_mechanism.md) には継承されません。

- **構成とパッチ管理 (Configuration and Patch Management) ([ISTG-FW-CONF](./README.md#configuration-and-patch-management-istg-fw-conf))**: このカテゴリはファームウェアファイルの構成とパッチ管理の側面に焦点を当てています。[ISTG-FW[UPDT]](./firmware_update_mechanism.md) は特定のファームウェアファイルではなくファームウェア更新メカニズムに焦点を当てているため、それぞれのテストケースは適用できません。

- **シークレット (Secrets) ([ISTG-FW-SCRT](./README.md#secrets-istg-fw-scrt))**: このカテゴリはファームウェアファイル内のシークレットの処理に焦点を当てています。[ISTG-FW[UPDT]](./firmware_update_mechanism.md) は特定のファームウェアファイルではなくファームウェア更新メカニズムに焦点を当てているため、それぞれのテストケースは適用できません。



## 認可 (Authorization) (ISTG-FW[UPDT]-AUTHZ) <a name="authorization-istg-fw[updt]-authz"></a>

ファームウェア更新メカニズムのテストは動的解析でもあるため、認可された個人のみが初期化および更新を実行できるかどうかをチェックすべきです。

### 認可されていないファームウェア更新 (Unauthorized Firmware Update) (ISTG-FW[UPDT]-AUTHZ-001) <a name="unauthorized-firmware-update-istg-fw[updt]-authz-001"></a>

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

特定のデバイスの具体的な実装によって、ファームウェア更新を実行するパーミッションは特定の認可アクセスレベル (*AA-2*, *AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。デバイスファームウェアがこれらのパーミッションを正しく検証できない場合、攻撃者 (*AA-1*) や意図したよりも低い認可アクセスレベルを持つ攻撃者が意図しないファームウェア更新を実行できるかもしれません。

**テスト目的**

- ファームウェア更新の実行に対する認可チェックが実装されているかどうかをチェックしなければなりません。

- 認可チェックが行われている場合、それをバイパスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、ファームウェア更新は特定の認可アクセスレベルを持つ個人のみが実行できることを確保します。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



## 暗号技術 (Cryptography) (ISTG-FW[UPDT]-CRYPT) <a name="cryptography-istg-fw[updt]-crypt"></a>

ファームウェア更新プロセスでは、暗号アルゴリズムを使用して、新しいファームウェアの完全性を検証して、転送時に機密データが漏洩しないように確保します。

### 不十分なファームウェア更新シグネチャ (Insufficient Firmware Update Signature) (ISTG-FW[UPDT]-CRYPT-001) <a name="insufficient-firmware-update-signature-istg-fw[updt]-crypt-001"></a>

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

デバイスを操作する方法の一つは操作されたファームウェアパッケージをインストールすることです。改変の検出を有効にするには、ファームウェア更新パッケージはデジタル署名されている必要があります。このようにして、インストールやアップデートプロセス時にパッケージの妥当性を検証できます。

**テスト目的**

- ファームウェア更新パッケージのデジタルシグネチャが利用可能かどうかを判断しなければなりません。

- デジタルシグネチャが利用可能な場合、シグネチャの妥当性を検証できるかどうかをチェックしなければなりません。

- [ISTG-FW-CRYPT-001](./README.md#usage-of-weak-cryptographic-algorithms-istg-fw-crypt-001) をベースとして、デジタルシグネチャの生成に使用される暗号アルゴリズムは、脆弱な時代遅れのアルゴリズムが使用されているかどうかを判断するために評価する必要があります。

**対応策**

ファームウェア更新パッケージには有効なデジタルシグネチャが利用可能でなければなりません。さらに、デジタルシグネチャの妥当性を検証できなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### 不十分なファームウェア更新暗号化 (Insufficient Firmware Update Encryption) (ISTG-FW[UPDT]-CRYPT-002) <a name="insufficient-firmware-update-encryption-istg-fw[updt]-crypt-002"></a>

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

ファームウェア更新パッケージにはソフトウェアおよびハードウェア開発者の知的財産などの機密データを含むかもしれません。そのため、パッケージ自体を暗号化することが必要になるかもしれません。

**テスト目的**

- ファームウェア更新パッケージを暗号化する必要があるかどうかをファームウェア開発者と明確にする必要があります。

- 暗号化が必要な場合、パッケージが暗号化されているかどうかを判断しなければなりません。

- [ISTG-FW-CRYPT-001](./README.md#usage-of-weak-cryptographic-algorithms-istg-fw-crypt-001) をベースとして、暗号化に適切なアルゴリズムが使用されているかどうかを判断する必要があります。

**対応策**

ファームウェア更新パッケージは最先端の暗号アルゴリズムを使用して暗号化すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### ファームウェア更新の安全でない転送 (Insecure Transmission of the Firmware Update) (ISTG-FW[UPDT]-CRYPT-003) <a name="insecure-transmission-of-the-firmware-update-istg-fw[updt]-crypt-003"></a>

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

ファームウェア更新パッケージが安全なチャネルを介して実行されない場合や、機密性を確保して送信データの改変を検出するためのさらなる対策を講じていない場合、通信チャネルにアクセスできる攻撃者は更新プロセスを妨害できるかもしれません。

**テスト目的**

- ファームウェア更新が安全なチャネルで実行されているかどうかを判断する必要があります。

- ファームウェア更新がインターネットなどの安全でないチャネルで実行される場合、機密性と完全性に関して適切な対策を講じているかどうかをチェックしなければなりません。

- たとえば、通信チャネルが TLS を使用して保護されている場合、どの暗号スイートがサポートされているか、サーバー証明書がクライアントによってバリデートされているかをチェックしなければなりません。

**対応策**

可能であれば、ファームウェア更新は安全なチャネルを介して実行すべきです。そうでない場合、適切な対策を実装して、潜在的な攻撃者による妨害を防いだり検出する必要があります。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

### ファームウェア更新シグネチャの不十分な検証 (Insufficient Verification of the Firmware Update Signature) (ISTG-FW[UPDT]-CRYPT-004) <a name="insufficient-verification-of-the-firmware-update-signature-istg-fw[updt]-crypt-004"></a>

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

ファームウェア更新パッケージがデジタル署名されていたとしても、デジタルシグネチャが適切にバリデートされていない場合、攻撃者は操作されたファームウェアパッケージをデバイスにインストールする可能性があります。たとえば、シグネチャが提供されない場合、デバイスは更新を拒否しないかもしれません。

**テスト目的**

- [ISTG-FW-CRYPT-001](./README.md#usage-of-weak-cryptographic-algorithms-istg-fw-crypt-001) をベースとして、ファームウェア更新パッケージのシグネチャが更新プロセス時にデバイスによって適切に検証されているかどうかをチェックしなければなりません。

**対応策**

デバイスはインストールプロセスを開始する前に更新パッケージのデジタルシグネチャを適切に検証しなければなりません。有効なシグネチャがない、またはシグネチャがまったくない更新パッケージは拒否すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



## ビジネスロジック (Business Logic) (ISTG-FW[UPDT]-LOGIC) <a name="business-logic-istg-fw[updt]-logic"></a>

ファームウェア更新の他のすべての側面が安全に実装および構成されていたとしても、更新プロセスの基盤となるロジック自体に問題があると、デバイスが攻撃に対して脆弱になるかもしれません。そのため、プロセスが意図したように動作しているかどうか、例外を検出して適切に処理しているかどうかを検証しなければなりません。

### 不十分なロールバック保護 (Insufficient Rollback Protection) (ISTG-FW[UPDT]-LOGIC-001) <a name="insufficient-rollback-protection-istg-fw[updt]-logic-001"></a>

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

製造業者によってはデバイスにロールバック保護を実装しています。このロールバック保護はデバイスファームウェアが現在インストールされているバージョンよりも古いバージョンに更新されることを防止します。これにより、攻撃者は有効ではあるが古いファームウェアをインストールして、そのバージョンの既知の脆弱性を悪用できなくなります。

**テスト目的**

- 古いバージョンのファームウェアをインストールできるかどうかを評価する必要があります。

**対応策**

インストールされるファームウェアバージョンが現在インストールされているバージョンよりも新しいことを検証する、適切なロールバック保護メカニズムを実装すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



[owasp_fstm]: https://github.com/scriptingxss/owasp-fstm	"OWASP Firmware Security Testing Methodology"
[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
