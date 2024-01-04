# 3.1. 処理装置 (Processing Units) (IOT-PROC)

## 目次
* [概要](#overview)
* [認可 (Authorization) (IOT-PROC-AUTHZ)](#authorization-iot-proc-authz)
  * [処理装置への認可されていないアクセス (Unauthorized Access to the Processing Unit) (IOT-PROC-AUTHZ-001)](#unauthorized-access-to-the-processing-unit-iot-proc-authz-001)
  * [権限昇格 (Privilege Escalation) (IOT-PROC-AUTHZ-002)](#privilege-escalation-iot-proc-authz-002)
* [ビジネスロジック (Business Logic) (IOT-PROC-LOGIC)](#business-logic-iot-proc-logic)
  * [インストラクションの安全でない実装 (Insecure Implementation of Instructions) (IOT-PROC-LOGIC-001)](#insecure-implementation-of-instructions-iot-proc-logic-001)
* [サイドチャネル攻撃 (Side-Channel Attacks) (IOT-PROC-SIDEC)](#side-channel-attacks-iot-proc-sidec)
  * [サイドチャネル攻撃に対する不十分な保護 (Insufficient Protection Against Side-Channel Attacks) (IOT-PROC-SIDEC-001)](#insufficient-protection-against-side-channel-attacks-iot-proc-sidec-001)




## 概要

このセクションにはコンポーネントの処理装置に関するテストケースとカテゴリが含まれます。処理装置は *PA-4* でのみアクセスできるデバイス内部要素です。処理装置への直接接続を確立するには特定のハードウェア機器 (デバッグボード、オシロスコープ、テストプローブなど) を必要とすることがあります。

処理装置に関連するテストケースカテゴリには、以下のものが特定されています。

- **認可 (Authorization):** 処理装置への認可されていないアクセスや制限された機能にアクセスするために権限を昇格することを可能にする脆弱性に焦点を当てています。

- **ビジネスロジック (Business Logic):** インストラクションの設計と実装における脆弱性と、文書化されていない潜在的に脆弱なインストラクションの存在に焦点を当てています。

- **サイドチャネル攻撃 (Side-channel Attacks):** タイミング攻撃やグリッチ攻撃などのサイドチャネル攻撃に対する耐性に焦点を当てています。



## 認可 (Authorization) (IOT-PROC-AUTHZ)

特定のデバイスのアクセスモデルによっては、特定の個人のみが処理装置への直接アクセスを許可されるかもしれません。そのため、適切な認証と認可の手順を設け、認可されたエンティティのみがアクセスできるようにする必要があります。

### 処理装置への認可されていないアクセス (Unauthorized Access to the Processing Unit) (IOT-PROC-AUTHZ-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、処理装置へのアクセスは特定の認可アクセスレベル (*AA-2*, *AA-3*, *AA-4* など) を持つエンティティに制限されるかもしれません。デバイスがアクセスパーミッションを正しく検証できない場合、攻撃者 (*AA-1*) がアクセスできるかもしれません。

**テスト目的**

- 処理装置へのアクセスに対する認可チェックが実装されているかどうかをチェックしなければなりません。

- 認可チェックが行われている場合、それをバイパスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、処理装置へのアクセスは認可されたエンティティのみが可能であることを確保します。

**参考情報**

このテストケースは [IOT-DES-AUTHZ-001](../data_exchange_services/README.md#unauthorized-access-to-the-data-exchange-service-iot-des-authz-001) をベースとしています。

### 権限昇格 (Privilege Escalation) (IOT-PROC-AUTHZ-002)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
		<td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-2</i> - <i>AA-3</i><br>(特定のデバイスのアクセスモデルによる)</td>
	</tr>
</table>

**要旨**

特定のデバイスの具体的な実装によって、処理装置の一部の機能へのアクセスは特定の認可アクセスレベル (*AA-3*, *AA-4* など) を持つ個人に制限されるかもしれません。処理装置がアクセスパーミッションを正しく検証できない場合、意図したより低い認可アクセスレベルを持つ攻撃者が制限された機能にアクセスできるかもしれません。

**テスト目的**

- [IOT-PROC-AUTHZ-001](#unauthorized-access-to-the-processing-unit-iot-proc-authz-001) をベースとして、与えられたアクセス権限を昇格して制限された機能にアクセスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、制限された機能へのアクセスは必要な認可アクセスレベルを持つ個人のみが可能であることを確保します。

**参考情報**

このテストケースは [IOT-DES-AUTHZ-002](../data_exchange_services/README.md#privilege-escalation-iot-des-authz-002) をベースとしています。



## ビジネスロジック (Business Logic) (IOT-PROC-LOGIC)

処理装置の基盤となるロジックに問題があると、デバイスが攻撃に対して脆弱になるかもしれません。そのため、処理装置とその機能が意図したように動作しているかどうか、例外を検出して適切に処理しているかどうかを検証しなければなりません。

### インストラクションの安全でない実装 (Insecure Implementation of Instructions) (IOT-PROC-LOGIC-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
		<td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(処理装置に正常に命令を出すために必要な権限のレベルによる)</td>
	</tr>
</table>
**要旨**

ビジネスロジックの実装に欠陥があると、デバイスの意図しない動作や誤動作を引き起こすかもしれません。たとえば、攻撃者が意図的に処理ワークフローの重要な命令をスキップしたり変更しようとすると、デバイスは未知の潜在的に安全でない状態になるかもしれません。

**テスト目的**

- 特定の実装に基づいて、命令がデバイスの動作を操作するために悪用される可能性があるかどうかを判断する必要があります。

- 使用する処理装置が文書化されていない潜在的に脆弱な命令をサポートしているかどうかをチェックしなければなりません。たとえば、これは命令をファジングしたり、処理装置モデルに関する調査を実施して行うことができます。

**対応策**

デバイスは未知の状態に陥るべきではありません。ワークフローの異常を検出しなければならず、例外を適切に処理する必要があります。

**参考情報**

このテストケースは [IOT-DES-LOGIC-001](../data_exchange_services/README.md#circumvention-of-the-intended-business-logic-iot-des-logic-001) をベースとしています。



## サイドチャネル攻撃 (Side-Channel Attacks) (IOT-PROC-SIDEC)

Side-channel attacks, such as timing and glitching attacks, are usually targeted against the physical implementation of a device or more specifically a processing unit instead of the device firmware or its interfaces. The goal of such attacks is to gather information about cryptographic algorithms and operations, performed by a processing unit, in order to retrieve key material, manipulate the cryptographic calculations or gain access to protected information.

### サイドチャネル攻撃に対する不十分な保護 (Insufficient Protection Against Side-Channel Attacks) (IOT-PROC-SIDEC-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
		<td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(depending on how the attack is being performed; see summary for more details)</td>
	</tr>
</table>
**要旨**

As mentioned above, side-channel attacks can be used by an attacker to get access to sensitive data or to manipulate the device operation. Usually, side-channel attacks are customized attacks tailored to a specific hardware implementation.

Depending on how the attack is being performed, different levels of authorization access might be required. Some side-channel attacks, such as glitching attacks, do not require authorization access at all since the attack is performed on a physical level by manipulating the power supply. Other side-channel attack vectors, such as the Meltdown vulnerability, require the execution of code by an attacker. Thus, some kind of authorization access is necessary.

**テスト目的**

- It has to be determined whether the processing unit is affected by known vulnerabilities, such as Meltdown and Spectre.

- During the testing period, the behavior of the processing unit has to be analyzed in order to assess the probability of successful side-channel attacks like timing or glitching attacks.

**対応策**

Based on the results of the analysis, the hardware design should be adjusted to be resilient against side-channel attacks. Furthermore, if publicly known vulnerabilities exist, the latest patches should be installed.

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["A practical implementation of the timing attack"][timing_attack] by Jean-François Dhem, François Koeune, Philippe-Alexandre Leroux, Patrick Mestré, Jean-Jacques Quisquater, and Jean-Louis Willems *(In Jean-Jacques Quisquater and Bruce Schneier, editors, Smart Card Research and Applications, pages 167 - 182, Berlin, Heidelberg, 2000. Springer Berlin Heidelberg.)*
* ["Injecting Power Attacks with Voltage Glitching and Generation of Clock Attacks for Testing Fault Injection Attacks"][power_glitching_attack] by Shaminder Kaur, Balwinder Singh, Harsimranjit Kaur, and Lipika Gupta *(In Pradeep Kumar Singh, Arti Noor, Maheshkumar H. Kolekar, Sudeep Tanwar, Raj K. Bhatnagar, and Shaweta Khanna, editors, Evolving Technologies for Computing, Communication and Smart World, pages 23 - 37, Singapore, 2021. Springer Singapore.)*
* ["Spectre attacks: Exploiting speculative execution"][spectre_attack] by Paul Kocher, Jann Horn, Anders Fogh, Daniel Genkin, Daniel Gruss, Werner Haas, Mike Hamburg, Moritz Lipp, Stefan Mangard, Thomas Prescher, Michael Schwarz, and Yuval Yarom *(In 40th IEEE Symposium on Security and Privacy (S&P'19), 2019.)*
* ["Meltdown: Reading kernel memory from user space"][meltdown_attack] Moritz Lipp, Michael Schwarz, Daniel Gruss, Thomas Prescher, Werner Haas, Anders Fogh, Jann Horn, Stefan Mangard, Paul Kocher, Daniel Genkin, Yuval Yarom, and Mike Hamburg *(In 27th USENIX Security Symposium (USENIX Security 18), 2018.)*



[timing_attack]: https://link.springer.com/chapter/10.1007/10721064_15	"A practical implementation of the timing attack"
[power_glitching_attack]: https://link.springer.com/chapter/10.1007/978-981-15-7804-5_3	"Injecting Power Attacks with Voltage Glitching and Generation of Clock Attacks for Testing Fault Injection Attacks"
[spectre_attack]: https://ieeexplore.ieee.org/document/8835233	"Spectre attacks: Exploiting speculative execution"
[meltdown_attack]: https://www.usenix.org/conference/usenixsecurity18/presentation/lipp	"Meltdown: Reading kernel memory from user"
