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

Proper checks need to be implemented to prevent unauthorized interaction with I2C components. As I2C does not feature authentication/authorization mechanisms by design, the bus and interfaces must be protected physically to be only accessible for authorized individuals.

**参考情報**

For this test case, data from the following sources was consolidated:

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

The information-gathering section aims to identify the details of the I2C implementation, including device addresses and available resources. This is crucial in understanding the attack surface.

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

I2C uses a two wire serial interface. One wire is the Serial Clock (SCL), the other one is the Serial Data (SDA). The master generates the clock signal and starts the communication with the slave. The slave receives the clock signal on the SCL wire and communicates with the master it was addressed by.

Each I2C device has a unique I2C address within the local connection. The I2C reference design has a 7 bit address space, which is sometimes extended up to 10 bits. A 7 bit address space allows a range of 128 distinct addresses. However, 16 of those are reserved for special tasks (0x00-0x07 and 0x78-0x7F), leaving only 112 addresses for the enumeration.

The detection of slaves can also be achieved passively by sniffing the communication (see [(ISTG-INT\[I2C\]-INFO-002)](#communication-sniffing-istg-inti2c-info-002)).

**テスト目的**

- The SCL and SDA pins/wires on the target device must be identified.
- A separate device (e.g., an Arduino, Bus Pirate, or HydraBus) or a Linux host with i2c-tools must be connected to the I2C data bus for scanning.
- A scanning tool (e.g., `i2cdetect -r` from the i2c-tools package) should be used to probe all 112 non-reserved addresses using read probes, which avoids accidental write transactions that could disturb or corrupt sensitive devices (e.g., EEPROMs, DACs) on the bus. The default write-probe mode (`i2cdetect` without `-r`) must only be used when the device types present on the bus are already known.
- The general call address (0x00) should also be probed, as it broadcasts to all slaves and may expose devices that do not respond to normal address enumeration.
- Identified device addresses should be cross-referenced against datasheets to determine the component type and supported register map.

**対応策**

While the discoverability of slave components is not considered a vulnerability, it helps understanding the design of the IoT device and preparing more targeted attacks.

**参考情報**

For this test case, data from the following sources was consolidated:

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

I2C uses a two wire serial interface. One wire is the Serial Clock (SCL), the other one is the Serial Data (SDA). The communication between different components can be sniffed by connecting a data analyzer to the SCL and SDA lines of the data bus. I2C is often used by onboard storage components, such as EEPROMs. It is possible that sensitive information will be exchanged between PCB components on the data bus.

**テスト目的**

- The SCL and SDA pins/wires on the target device must be identified.
- A logic analyzer or dedicated hardware tool (e.g., Bus Pirate in I2C sniffer mode, HydraBus, Saleae) must be connected to the I2C data bus and the capture settings (e.g., sampling rate, threshold voltage) have to be configured accordingly.
- The I2C communication of the target device must be captured and analyzed in different states (e.g., startup, normal operation, firmware update).
- Captured traffic must be inspected for sensitive data, including keys, credentials, or configuration parameters.

**対応策**

The transmission of sensitive data should be reduced to the minimum which is required for operating the device. As I2C does not feature protective measures by design, the bus and interfaces must be secured physically to be only accessible for authorized individuals.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

- [Saleae Logic Analyzer](https://www.saleae.com)
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

I2C is frequently used to connect EEPROMs and other non-volatile memory devices to a microcontroller. These memory chips often store sensitive data such as firmware, configuration parameters, cryptographic keys, or device credentials. Since the I2C bus provides no authentication or encryption by default, an attacker with physical access to the bus can directly read the contents of attached memory devices using standard tooling.

**テスト目的**

- Based on [ISTG-INT\[I2C\]-INFO-001](#slave-enumeration-istg-inti2c-info-001), I2C devices at known EEPROM address ranges (e.g., 0x50-0x57 for 24Cxx series EEPROMs) must be identified.
- The contents of identified memory devices must be extracted using tools such as `i2cdump` (from the i2c-tools package) or a hardware tool (e.g., Bus Pirate, HydraBus). Note: `i2cdump` uses 8-bit internal addressing by default and will wrap at 256 bytes; for EEPROMs with 16-bit internal addressing (e.g., 24C256, 24C512), word-addressed mode (`i2cdump -y <bus> <addr> w`) or a scripted multi-page read must be used to obtain a complete dump.
- Extracted data must be analyzed for sensitive information, including firmware images, configuration files, cryptographic keys, and credentials.
- It must be determined whether the extracted data is stored in plaintext or is protected by encryption.
- If write access to the EEPROM is possible, the state of the write-protect (WP) pin must be verified. An improperly grounded WP pin allows an attacker to overwrite EEPROM contents, which may enable persistent compromise of device configuration or credentials.

**対応策**

Sensitive data stored in EEPROM or other I2C-attached memory must be encrypted. Cryptographic keys and credentials should not be stored on devices accessible via an unprotected bus. Where possible, cryptographic memory modules with built-in access control (e.g., Microchip ATECC608) should be used instead of general-purpose EEPROMs.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

- [i2c-tools Package](https://i2c.wiki.kernel.org/index.php/I2C_Tools)
- [Bus Pirate I2C Documentation](http://dangerousprototypes.com/docs/Bus_Pirate_I2C)
- [HydraBus Open-Source Hardware Tool](https://hydrabus.com)



## 入力バリデーション (Input Validation) (ISTG-INT[I2C]-INPV) <a name="input-validation-istg-inti2c-inpv"></a>

Input validation is critical to ensure that only valid, expected, and safe data is accepted by the I2C components. Malformed input or invalid commands could potentially compromise the device's security or stability.

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

I2C uses a master-slave architecture where the master generates a clock signal and initializes the communication with slaves. If any of the components does not properly validate the received data or commands, an attacker might be able to manipulate the device's behavior or render it unavailable.

**テスト目的**

- The SCL and SDA pins/wires on the target device must be identified.
- A separate device (e.g., an Arduino, Bus Pirate, or HydraBus) must be connected to the I2C data bus for sending malformed data.
- An appropriate software library should be used to craft and send malformed data or invalid commands to I2C slave components. Examples of conditions to test include:
  - Out-of-range register addresses or command bytes not defined in the device datasheet.
  - Payloads that exceed the expected data length for a given register write.
  - NAK flooding: repeatedly sending START conditions without completing transactions.
  - Repeated START (Sr) condition abuse: chaining transactions without releasing the bus to detect improper bus state handling.
- Clock stretching behavior must be tested where applicable: a slave that holds SCL low indefinitely can stall the master and cause a denial-of-service condition. The master's timeout handling for clock stretching must be verified.
- The general call address (0x00) must be used to broadcast commands to all slaves simultaneously to test whether slaves respond unexpectedly to global resets or other general call commands.
- The reaction and recovery behavior of target components after receiving malformed input must be documented and assessed.

**対応策**

The I2C components should reject and not further process invalid data or commands, as they could harm the device's integrity and availability. Proper error handling should prevent the system from crashing or behaving unexpectedly.

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
