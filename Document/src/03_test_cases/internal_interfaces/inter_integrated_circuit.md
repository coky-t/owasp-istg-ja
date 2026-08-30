# 3.5.1. I2C (Inter-Integrated Circuit) (ISTG-INT[I2C])

## 目次 <a name="table-of-contents"></a>

- [概要](#overview)
- [認可 (Authorization) (ISTG-INT\[I2C\]-AUTHZ)](#authorization-istg-inti2c-authz)
	- [認可されていないデバイスとのバスインタラクション (Bus Interaction with Unauthorized Devices) (ISTG-INT\[I2C\]-AUTHZ-001)](#bus-interaction-with-unauthorized-devices-istg-inti2c-authz-001)
- [情報収集 (Information Gathering) (ISTG-INT\[I2C\]-INFO)](#information-gathering-istg-inti2c-info)
	- [スレーブ列挙 (Slave Enumeration) (ISTG-INT\[I2C\]-INFO-001)](#slave-enumeration-istg-inti2c-info-001)
	- [通信傍受 (Communication Sniffing) (ISTG-INT\[I2C\]-INFO-002)](#communication-sniffing-istg-inti2c-info-002)
	- [EEPROM/メモリ抽出 (EEPROM/Memory Extraction) (ISTG-INT\[I2C\]-INFO-003)](#eeprommemory-extraction-istg-inti2c-info-003)
- [入力バリデーション (Input Validation) (ISTG-INT\[I2C\]-INPV)](#input-validation-istg-inti2c-inpv)
	- [無効なデータの不十分な処理 (Insufficient Handling of Invalid Data) (ISTG-INT\[I2C\]-INPV-001)](#insufficient-handling-of-invalid-data-istg-inti2c-inpv-001)

## 概要 <a name="overview"></a>

内部インタフェースコンポーネントの一つの特化に Inter-Integrated Circuit (I2C) があります。I2C は、組込みシステムでマイクロコントローラと周辺機器を接続するために広く使用されている、同期式シリアル通信プロトコルです。これはマスタースレーブアーキテクチャを持ち、データ送受信を同時に行うことはできません。デフォルトでは認証、認可、暗号技術などのセキュリティ機能を提供しません。そのため、安全な通信を有効にするには、I2C の上位でより高度なプロトコルを使用する必要があります。

この専門モジュールは、I2C に特化したテストケースを追加することで、IoT デバイスにおける I2C インタフェースのセキュリティをテストするための体系的なアプローチを提供することを目的としています。ビジネスロジック、暗号技術、シークレットなど、親ガイドの一部のカテゴリは、低レベル通信プロトコルであるという I2C の性質上、I2C には適用できません。

以下のカテゴリは特化した [ISTG-INT[I2C]](./inter_integrated_circuit.md) には継承されません。

- **構成とパッチ管理 (Configuration and Patch Management) ([ISTG-INT-CONF](./README.md#configuration-and-patch-management-istg-int-conf))**: このカテゴリは内部インタフェースの構成とパッチ管理の側面に焦点を当てています。この特化は、上位のソフトウェアやアプリケーションではなく、通信プロトコル I2C に焦点を当てているため、それぞれのテストケースは適用できません。
- **シークレット (Secrets) ([ISTG-INT-SCRT](./README.md#secrets-istg-int-scrt))**: このカテゴリは内部インタフェースを介したシークレットのアクセシビリティに焦点を当てています。I2C データバスはシークレットの送信に使用される可能性があり、またシークレットは I2C 接続のメモリに格納される可能性もあるため、これはすでに通信傍受のテストケース [(ISTG-INT\[I2C\]-INFO-002)](#communication-sniffing-istg-inti2c-info-002) とEEPROM/メモリ抽出のテストケース [(ISTG-INT\[I2C\]-INFO-003)](#eeprommemory-extraction-istg-inti2c-info-003) でカバーされています。
- **暗号技術 (Cryptography) ([ISTG-INT-CRYPT](./README.md#cryptography-istg-int-crypt))**: このカテゴリは強力な暗号アルゴリズムの使用に焦点を当てています。I2C はデフォルトでは暗号化を提供しない低レベルプロトコルであるため、これらのテストケースは適用できません。
- **ビジネスロジック (Business Logic) ([ISTG-INT-LOGIC](./README.md#business-logic-istg-int-logic))**: このカテゴリは、意図されたビジネスロジックの回避に焦点を当てており、デバイスの予期しない動作や誤動作につながる可能性があります。I2C は上位のソフトウェアやアプリケーションではなく通信プロトコルであるため、従来のビジネスロジックのテストケースは適用できません。しかしながら、意図しない動作のテストは入力バリデーションのテストケース [ISTG-INT\[I2C\]-INPV-001](#insufficient-handling-of-invalid-data-istg-inti2c-inpv-001) でカバーされています。

## 認可 (Authorization) (ISTG-INT[I2C]-AUTHZ) <a name="authorization-istg-inti2c-authz"></a>

I2C 通信での認可は、認可されたデバイスやユーザーのみが I2C バスを介してやり取りできるようにし、不正アクセスを防止することに重点を置いています。I2C は一般的に認証メカニズムを欠いているため、アクセスがどのように制御されているかを評価することが極めて重要です。

### 認可されていないデバイスとのバスインタラクション (Bus Interaction with Unauthorized Devices) (ISTG-INT[I2C]-AUTHZ-001) <a name="bus-interaction-with-unauthorized-devices-istg-inti2c-authz-001"></a>

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-3</i> - <i>PA-4</i><br>(I2C インタフェースがデバイス上のどこかで非侵襲的にアクセス可能であるかどうかによる)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

I2C 通信バスは複数のマスターと複数のスレーブをサポートしています。IoT ハードウェアでは、回路基板 (PCB) 上のさまざまなコンポーネントが I2C バスを介して互いに通信を行い、データをやり取りしたりタスクを実行します。システムを評価する際には、単に通信を監視するだけでなく、通信で能動的にやり取りすることも重要です。これはバス上の追加のマスターとして振る舞うことで、たとえば、スレーブとやり取りしたり通信を制御することが行われます。

他のマスターとの干渉を最小限に抑えるために、回路トレースを切断することでこれらを分離できます。しかしながら、これは一般的に侵襲的な介入 (アクセスレベル PA-4) を必要とします。

**テスト目的**

- IoT デバイス上のマスターとスレーブコンポーネントを特定する必要があります。
- それらのコンポーネントによってサポートされるメッセージやアクションに関する情報を収集する必要があります (ベンダーのデータシート)。
- I2C コンポーネントとやり取りするには、専用のハードウェアツール (Bus Pirate, HydraBus など) やマイクロコントローラ (smbus2 を用いる Raspberry Pi、Wire.h を用いる Arduino など) を使用する必要があります。
- コンポーネントの反応や応答を解析する必要があります。

**対応策**

I2C コンポーネントでの不正なインタラクションを防ぐために、適切なチェックを実装する必要があります。I2C は設計上、認証/認可メカニズムを備えていないため、認可された個人だけがアクセスできるように、バスやインタフェースを物理的に保護しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

- [Python smbus2 Library](https://pypi.org/project/smbus2/)
- [Arduino Wire.h Library](https://docs.arduino.cc/language-reference/en/functions/communication/wire/)
- [Bus Pirate I2C Guide](http://dangerousprototypes.com/docs/I2C)
- [HydraBus Open-Source Hardware Tool](https://hydrabus.com)

## 情報収集 (Information Gathering) (ISTG-INT[I2C]-INFO) <a name="information-gathering-istg-inti2c-info"></a>

情報収集のセクションは、デバイスアドレスや利用可能なリソースなど、I2C 実装の詳細を特定することを目指します。これは攻撃対象領域を理解する上で極めて重要です。

### スレーブ列挙 (Slave Enumeration) (ISTG-INT[I2C]-INFO-001) <a name="slave-enumeration-istg-inti2c-info-001"></a>

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-3</i> - <i>PA-4</i><br>(I2C インタフェースがデバイス上のどこかで非侵襲的にアクセス可能であるかどうかによる)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

I2C は二線式シリアルインタフェースを使用します。一方の線はシリアルクロック (Serial Clock, SCL)、もう一方はシリアルデータ (Serial Data, SDA) です。マスターはクロック信号を生成し、スレーブとの通信を開始します。スレーブは SCL 線でクロック信号を受信し、自身を宛先としたマスターと通信します。

各 I2C デバイスはローカル接続内に固有の I2C アドレスを持ちます。I2C リファレンス設計は 7 ビットアドレス空間を持ち、10 ビットに拡張されることもあります。7 ビットアドレス空間は 128 通りのアドレスが可能です。しかし、そのうち 16 個 (0x00-0x07 および 0x78-0x7F) は特殊な用途のために予約されており、その列挙には 112 アドレスのみ残ります。

スレーブの検出は通信をスニッフィングすることによって受動的に成し遂げることも可能です ([(ISTG-INT\[I2C\]-INFO-002)](#communication-sniffing-istg-inti2c-info-002) を参照)。

**テスト目的**

- ターゲットデバイスの SCL および SDA ピン/配線を特定しなければなりません。
- スキャン実行には、I2C データバスに別のデバイス (Arduino, Bus Pirate, HydraBus など) または i2c-tools を備えた Linux ホストを接続しなければなりません。
- スキャンツール (i2c-tools パッケージの `i2cdetect -r` など) を使用して、予約済みではない全 112 アドレスを読み取りプローブを使用して調査し、バス上の機密性の高いデバイス (EEPROM, DAC など) を妨害したり破損する恐れのある不慮の書き込みトランザクションを避けます。デフォルトの書き込みプローブモード (`-r` なしでの `i2cdetect`) は、バス上のデバイスの種類がすでに判明している場合にのみ使用しなければなりません。
- ジェネラルコールアドレス (0x00) も調査すべきです。すべてのスレーブにブロードキャストするため、通常のアドレス列挙では応答しないデバイスを示す可能性があります。
- 特定されたデバイスアドレスはデータシートと照らし合わせ、コンポーネントの種類やサポートされているレジスタマップを突き止める必要があります。

**対応策**

スレーブコンポーネントの発見可能性は脆弱性とはみなされませんが、IoT デバイスの設計を理解し、より標的を絞った攻撃を準備するのに役立ちます。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

- [I2C Wiring Basics](https://learn.adafruit.com/scanning-i2c-addresses/i2c-basics)
- [I2C Scanning Tool for Arduino](https://learn.adafruit.com/scanning-i2c-addresses/arduino)
- [i2c-tools Package](https://i2c.wiki.kernel.org/index.php/I2C_Tools)

### 通信傍受 (Communication Sniffing) (ISTG-INT[I2C]-INFO-002) <a name="communication-sniffing-istg-inti2c-info-002"></a>

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-3</i> - <i>PA-4</i><br>(I2C インタフェースがデバイス上のどこかで非侵襲的にアクセス可能であるかどうかによる)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

I2C は二線式シリアルインタフェースを使用します。一方の線はシリアルクロック (Serial Clock, SCL)、もう一方はシリアルデータ (Serial Data, SDA) です。さまざまなコンポーネント間の通信は、データバスの SCL および SDA ラインにデータアナライザを接続することで、スニッフィング可能です。I2C は EEPROM などのオンボードストレージコンポーネントでよく使用されています。データバス上の PCB コンポーネント間で機密情報がやり取りされる可能性があります。

**テスト目的**

- ターゲットデバイスの SCL および SDA ピン/配線を特定しなければなりません。
- ロジックアナライザや専用のハードウェアツール (I2C スニファーモードの Bus Pirate, HydraBus, Saleae など) を I2C データバスに接続しなければならず、キャプチャ設定 (サンプリングレート、閾値電圧など) を適切に構成する必要があります。
- さまざまな状態 (起動時、通常動作時、ファームウェア更新時など) でのターゲットデバイスの I2C 通信をキャプチャし、解析しなければなりません。
- キー、クレデンシャル、設定パラメータなどの機密データについてキャプチャしたトラフィックを調査しなければなりません。

**対応策**

機密データの送信はデバイスの動作に必要な最小限に抑える必要があります。I2C は設計上、保護機能を備えていないため、認可された個人だけがアクセスできるように、バスやインタフェースを物理的に保護しなければなりません。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

- [Saleae Logic Analyzer Documentation](https://support.saleae.com/)
- [sigrok Signal Analysis Software](https://sigrok.org/wiki/Main_Page)
- [Bus Pirate I2C Documentation](http://dangerousprototypes.com/docs/Bus_Pirate_I2C)

### EEPROM/メモリ抽出 (EEPROM/Memory Extraction) (ISTG-INT[I2C]-INFO-003) <a name="eeprommemory-extraction-istg-inti2c-info-003"></a>

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-3</i> - <i>PA-4</i><br>(depending on whether an I2C interface is accessible non-invasively somewhere on the device)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

I2C は EEPROM やその他の不揮発性メモリデバイスをマイクロコントローラに接続する際によく使用されます。これらのメモリチップは、ファームウェア、設定パラメータ、暗号鍵、デバイスクレデンシャルといった機密データを格納していることがよくあります。I2C バスはデフォルトで認証や暗号化を提供していないため、バスに物理アクセスできる攻撃者は、標準的なツールを使用して、アタッチされたメモリデバイスの内容を直接読み取ることが可能です。

**テスト目的**

- [ISTG-INT\[I2C\]-INFO-001](#slave-enumeration-istg-inti2c-info-001) に基づいて、既知の EEPROM アドレス範囲 (例: 24Cxx シリーズ EEPROM では 0x50-0x57) にある I2C デバイスを特定しなければなりません。
- 特定されたメモリデバイスの内容は `i2cdump` (i2c-tools パッケージから) やハードウェアツール (Bus Pirate, HydraBus など) といったツールを使用して抽出しなければなりません。注: `i2cdump` はデフォルトで 8 ビット内部アドレス指定を使用し、256 バイトで折り返します。16 ビット内部アドレス指定の EEPROM (24C256, 24C512 など) では、完全なダンプを取得するには、ワードアドレスモード (`i2cdump -y <bus> <addr> w`) またはスクリプトによるマルチページ読み取りを使用しなければなりません。
- 抽出されたデータは、ファームウェアイメージ、設定ファイル、暗号鍵、クレデンシャルなどの機密情報について解析しなければなりません。
- 抽出されたデータが平文で保存されているか、暗号化によって保護されているかを確認しなければなりません。
- EEPROM への書き込みアクセスが可能な場合、ライトプロテクト (WP) ピンの状態を検証しなければなりません。適切に接地されていない WP ピンは、攻撃者が EEPROM の内容を上書きでき、デバイスの設定やクレデンシャルの永続的な侵害を可能にする恐れがあります。

**対応策**

EEPROM やその他の I2C にアタッチされたメモリに格納される機密データは暗号化されなければなりません。暗号鍵やクレデンシャルは保護されていないバスを介してアクセス可能なデバイス上に格納すべきではありません。可能であれば、汎用 EEPROM の代わりに、ビルトインのアクセス制御を持つ暗号メモリモジュール (例: Microchip ATECC608) を使用すべきです。

**参考情報**

このテストケースでは、以下の情報源からのデータを整理統合しました。

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

- [i2c-tools Package](https://i2c.wiki.kernel.org/index.php/I2C_Tools)
- [Bus Pirate I2C Documentation](http://dangerousprototypes.com/docs/Bus_Pirate_I2C)
- [HydraBus Open-Source Hardware Tool](https://hydrabus.com)



## 入力バリデーション (Input Validation) (ISTG-INT[I2C]-INPV) <a name="input-validation-istg-inti2c-inpv"></a>

妥当で、想定通りで、安全なデータのみが I2C コンポーネントによって受け入れられるようにするには、入力バリデーションが不可欠です。不正な形式の入力や無効なコマンドはデバイスのセキュリティや安定性を損なう恐れがあります。

### 無効なデータの不十分な処理 (Insufficient Handling of Invalid Data) (ISTG-INT[I2C]-INPV-001) <a name="insufficient-handling-of-invalid-data-istg-inti2c-inpv-001"></a>

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">物理 (Physical)</th>
 <td><i>PA-3</i> - <i>PA-4</i><br>(I2C インタフェースがデバイス上のどこかで非侵襲的にアクセス可能であるかどうかによる)</td>
	</tr>
	<tr valign="top">
		<th align="left">認可 (Authorization)</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

I2C は、マスターがクロック信号を生成し、スレーブとの通信を開始する、マスタースレーブアーキテクチャを使用します。いずれかのコンポーネントが受信したデータやコマンドを適切に検証しない場合、攻撃者はデバイスの動作を操作したり、それを利用不能にする恐れがあります。

**テスト目的**

- ターゲットデバイスの SCL および SDA ピン/配線を特定しなければなりません。
- 不正な形式のデータを送信するには、I2C データバスに別のデバイス (Arduino, Bus Pirate, HydraBus など) を接続しなければなりません。
- I2C スレーブコンポーネントに不正なデータや無効なコマンドを作成して送信するには、適切なソフトウェアライブラリを使用する必要があります。テストのための条件の例には以下があります。
  - デバイスのデータシートに定義されていない範囲外のレジスタアドレスやコマンドバイト。
  - 特定のレジスタ書き込みに対して想定されるデータ長を超えるペイロード。
  - NAK フラッディング: トランザクションを完了せずに START コンディションを繰り返し送信します。
  - Repeated START (Sr) コンディションの悪用: バスを解放せずにトランザクションを連鎖して、不適切なバス状態処理を検出します。
- 適用できる場合にはクロックストレッチングの挙動をテストする必要があります。SCL を無期限に Low に保持するスレーブは、マスターを失速し、サービス拒否コンディションを引き起こす可能性があります。クロックストレッチングに対するマスターのタイムアウト処理を検証する必要があります。
- すべてのスレーブにコマンドをブロードキャストするためのジェネラルコールアドレス (0x00) を使用して、スレーブがグローバルリセットやその他のジェネラルコールコマンドに予期しない応答をするかどうかをテストする必要があります。
- 不正な形式の入力を受信した後のターゲットコンポーネントのリアクションおよびリカバリの挙動が文書化され評価される必要があります。

**対応策**

I2C コンポーネントは、デバイスの完全性や可用性を損なう恐れがあるため、無効なデータやコマンドを拒否し、それ以降処理しないようにする必要があります。適切なエラー処理はシステムをクラッシュや予期しない動作から防ぎます。

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

- [Understanding the I2C Bus - Texas Instruments](https://www.ti.com/lit/an/slva704/slva704.pdf)
- [Arduino Wire.h Library](https://docs.arduino.cc/language-reference/en/functions/communication/wire/)



[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
