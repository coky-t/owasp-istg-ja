# 3.1. 処理装置 (Processing Units) (ISTG-PROC)

## 目次
* [概要](#overview)
* [認可 (Authorization) (ISTG-PROC-AUTHZ)](#authorization-istg-proc-authz)
  * [処理装置への認可されていないアクセス (Unauthorized Access to the Processing Unit) (ISTG-PROC-AUTHZ-001)](#unauthorized-access-to-the-processing-unit-istg-proc-authz-001)
  * [権限昇格 (Privilege Escalation) (ISTG-PROC-AUTHZ-002)](#privilege-escalation-istg-proc-authz-002)
* [ビジネスロジック (Business Logic) (ISTG-PROC-LOGIC)](#business-logic-istg-proc-logic)
  * [インストラクションの安全でない実装 (Insecure Implementation of Instructions) (ISTG-PROC-LOGIC-001)](#insecure-implementation-of-instructions-istg-proc-logic-001)
* [サイドチャネル攻撃 (Side-Channel Attacks) (ISTG-PROC-SIDEC)](#side-channel-attacks-istg-proc-sidec)
  * [サイドチャネル攻撃に対する不十分な保護 (Insufficient Protection Against Side-Channel Attacks) (ISTG-PROC-SIDEC-001)](#insufficient-protection-against-side-channel-attacks-istg-proc-sidec-001)




## 概要 <a name="overview"></a>

このセクションにはコンポーネントの処理装置に関するテストケースとカテゴリが含まれます。処理装置は *PA-4* でのみアクセスできるデバイス内部要素です。処理装置への直接接続を確立するには特定のハードウェア機器 (デバッグボード、オシロスコープ、テストプローブなど) を必要とすることがあります。

処理装置に関連するテストケースカテゴリには、以下のものが特定されています。

- **認可 (Authorization):** 処理装置への認可されていないアクセスや制限された機能にアクセスするために権限を昇格することを可能にする脆弱性に焦点を当てています。

- **ビジネスロジック (Business Logic):** インストラクションの設計と実装における脆弱性と、文書化されていない潜在的に脆弱なインストラクションの存在に焦点を当てています。

- **サイドチャネル攻撃 (Side-channel Attacks):** タイミング攻撃やグリッチ攻撃などのサイドチャネル攻撃に対する耐性に焦点を当てています。



## 認可 (Authorization) (ISTG-PROC-AUTHZ) <a name="authorization-istg-proc-authz"></a>

特定のデバイスのアクセスモデルによっては、特定の個人のみが処理装置への直接アクセスを許可されるかもしれません。そのため、適切な認証と認可の手順を設け、認可されたエンティティのみがアクセスできるようにする必要があります。

### 処理装置への認可されていないアクセス (Unauthorized Access to the Processing Unit) (ISTG-PROC-AUTHZ-001) <a name="unauthorized-access-to-the-processing-unit-istg-proc-authz-001"></a>

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

このテストケースは [ISTG-DES-AUTHZ-001](../data_exchange_services/README.md#unauthorized-access-to-the-data-exchange-service-istg-des-authz-001) をベースとしています。

### 権限昇格 (Privilege Escalation) (ISTG-PROC-AUTHZ-002) <a name="privilege-escalation-istg-proc-authz-002"></a>

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

- [ISTG-PROC-AUTHZ-001](#unauthorized-access-to-the-processing-unit-istg-proc-authz-001) をベースとして、与えられたアクセス権限を昇格して制限された機能にアクセスする方法があるかどうかを判断しなければなりません。

**対応策**

適切な認可チェックが実装されている必要があり、制限された機能へのアクセスは必要な認可アクセスレベルを持つ個人のみが可能であることを確保します。

**参考情報**

このテストケースは [ISTG-DES-AUTHZ-002](../data_exchange_services/README.md#privilege-escalation-istg-des-authz-002) をベースとしています。



## ビジネスロジック (Business Logic) (ISTG-PROC-LOGIC) <a name="business-logic-istg-proc-logic"></a>

処理装置の基盤となるロジックに問題があると、デバイスが攻撃に対して脆弱になるかもしれません。そのため、処理装置とその機能が意図したように動作しているかどうか、例外を検出して適切に処理しているかどうかを検証しなければなりません。

### インストラクションの安全でない実装 (Insecure Implementation of Instructions) (ISTG-PROC-LOGIC-001) <a name="insecure-implementation-of-instructions-istg-proc-logic-001"></a>

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

このテストケースは [ISTG-DES-LOGIC-001](../data_exchange_services/README.md#circumvention-of-the-intended-business-logic-istg-des-logic-001) をベースとしています。



## サイドチャネル攻撃 (Side-Channel Attacks) (ISTG-PROC-SIDEC) <a name="side-channel-attacks-istg-proc-sidec"></a>

タイミング攻撃やグリッチ攻撃などのサイドチャネル攻撃は、通常、デバイスファームウェアやそのインタフェースではなく、デバイスの物理実装、より具体的には処理装置をターゲットとしています。このような攻撃の目的は処理装置で実行する暗号アルゴリズムや操作に関する情報を収集し、鍵マテリアルの取得、暗号計算の操作、保護情報へのアクセスを行うことです。

### サイドチャネル攻撃に対する不十分な保護 (Insufficient Protection Against Side-Channel Attacks) (ISTG-PROC-SIDEC-001) <a name="insufficient-protection-against-side-channel-attacks-istg-proc-sidec-001"></a>

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

上述したように、攻撃者はサイドチャネル攻撃を使用して、機密データへのアクセスやデバイス動作の操作を行う可能性があります。通常、サイドチャネル攻撃は特定のハードウェア実装に合わせてカスタマイズされた攻撃です。

攻撃の実行方法に応じて、さまざまなレベルの認可アクセスが必要になることがあります。グリッチ攻撃などの一部のサイドチャネル攻撃は、電力供給を操作することによって物理レベルで実行されるため、認可アクセスをまったく必要としません。メルトダウン脆弱性などの他のサイドチャネル攻撃ベクトルは攻撃者によるコードの実行が必要です。そのため、何らかの認可アクセスが必要になります。

**テスト目的**

- 処理装置がメルトダウンやスペクターなどの既知の脆弱性の影響を受けるかどうかを判断する必要があります。

- テスト期間中に、処理装置の動作を解析して、タイミング攻撃やグリッチ攻撃などのサイドチャネル攻撃が成功する確率を評価する必要があります。

**対応策**

解析結果に基づき、ハードウェア設計を調整して、サイドチャネル攻撃に対する耐性を持つようにすべきです。さらに、一般的に既知の脆弱性が存在する場合、最新パッチをインストールすべきです。

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
