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

Usually, only certain individuals, e.g., administrators, should be allowed to access the device firmware during runtime. Thus, proper authentication and authorization procedures need to be in place, which ensure that only authorized users can get access to the firmware.

### ファームウェアへの認可されていないアクセス (Unauthorized Access to the Firmware) (IOT-FW[INST]-AUTHZ-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(depending on how the firmware can be accessed, e.g., via an internal/physical debugging interface or remotely via SSH)</td>
	</tr>
	<tr valign="top">
		<th align="left">Authorization</th>
		<td><i>AA-1</i></td>
	</tr>
</table>

**要旨**

Depending on the specific implementation of a given device, access to the firmware or its functions might be restricted to individuals with a certain authorization access level, e.g., *AA-2*, *AA-3* or *AA-4*. If the device firmware fails to correctly verify access permissions, any attacker (*AA-1*) might be able to get access to the firmware.

**テスト目的**

- It must be checked if authorization checks for access to the firmware are implemented.

- In case that authorization checks are in place, it must be determined whether there is a way to bypass them.

**対応策**

Proper authorization checks need to be implemented, which ensure that access to the firmware is only possible for authorized individuals.

**参考情報**

For this test case, data from the following sources was consolidated:

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-AUTHZ-001](../data_exchange_services/README.md#unauthorized-access-to-the-data-exchange-service-iot-des-authz-001).

### 権限昇格 (Privilege Escalation) (IOT-FW[INST]-AUTHZ-002)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(depending on how the firmware can be accessed, e.g., via an internal/physical debugging interface or remotely via SSH)</td>
	</tr>
	<tr valign="top">
		<th align="left">Authorization</th>
		<td><i>AA-1</i> - <i>AA-3</i><br>(depending on the access model for the given device) </td>
	</tr>
</table>

**要旨**

Depending on the specific implementation of a given device, access to parts of the firmware or its functions might be restricted to individuals with a certain authorization access level, e.g., *AA-3* or *AA-4*. If the device firmware fails to correctly verify access permissions, an attacker with a lower authorization access level than intended might be able to get access to the restricted firmware parts.

**テスト目的**

- Based on *IOT-FW-AUTHZ-001*, it must be determined whether there is a way to elevate the given access privileges and thus to access restricted functions or parts of the firmware.

**対応策**

Proper authorization checks need to be implemented, which ensure that access to restricted parts of the firmware is only possible for individuals with the required authorization access levels.

**参考情報**

This test case is based on: [IOT-DES-AUTHZ-002](../data_exchange_services/README.md#privilege-escalation-iot-des-authz-002).



## 情報収集 (Information Gathering) (IOT-FW[INST]-INFO)

As mentioned above, during the dynamic analysis, it is also possible to test whether user data is securely stored on the device during runtime.

### ユーザーデータの開示 (Disclosure of User Data) (IOT-FW[INST]-INFO-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(depending on how the firmware can be accessed, e.g., via an internal/physical debugging interface or remotely via SSH)</td>
	</tr>
	<tr valign="top">
		<th align="left">Authorization</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(depending on the access model for the given device) </td>
	</tr>
</table>

**要旨**

During runtime, a device is accumulating and processing data of different kinds, such as personal data of its users. If this data is not stored securely, an attacker might be able to recover it from the device.

**テスト目的**

- It has to be checked whether user data can be accessed by unauthorized individuals.

**対応策**

Access to user data should only be granted to individuals and processes that need to have access to it. No unauthorized or not properly authorized individual should be able to recover user data.

**参考情報**

For this test case, data from the following sources was consolidated:

* OWASP ["Firmware Security Testing Methodology"][owasp_fstm]
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH



## 暗号技術 (Cryptography) (IOT-FW[INST]-CRYPT)

Many IoT devices need to implement cryptographic algorithms, e.g., to securely store sensitive data, for authentication purposes or to receive and verify encrypted data from other network nodes. Failing to implement secure, state of the art cryptography might lead to the exposure of sensitive data, device malfunctions or loss of control over the device.

### ブートローダーシグネチャの不十分な検証 (Insufficient Verification of the Bootloader Signature) (IOT-FW[INST]-CRYPT-001)

**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-1</i> - <i>PA-4</i><br>(depending on how the firmware can be accessed, e.g., via an internal/physical debugging interface or remotely via SSH)</td>
	</tr>
	<tr valign="top">
		<th align="left">Authorization</th>
		<td><i>AA-1</i> - <i>AA-4</i><br>(depending on the access model for the given device) </td>
	</tr>
</table>

**要旨**

Verifying the digital signature of the bootloader is an important measure to detect manipulations of the bootloader, thus preventing the execution of manipulated firmware on a device.

**テスト目的**

- It must be checked if the signature of the bootloader is properly verified by the device during the boot process.

**対応策**

The device must properly verify the digital signature of a bootloader before it is executed. A bootloader without a valid signature should not be executed.



[owasp_fstm]: https://github.com/scriptingxss/owasp-fstm	"OWASP Firmware Security Testing Methodology"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
