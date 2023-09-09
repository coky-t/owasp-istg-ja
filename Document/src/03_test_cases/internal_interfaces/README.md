# 3.5. 内部インタフェース (Internal Interfaces) (IOT-INT)

## 目次
* [概要](#overview)
* [認可 (Authorization) (IOT-INT-AUTHZ)](#authorization-iot-int-authz)
  * [インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface) (IOT-INT-AUTHZ-001)](#unauthorized-access-to-the-interface-iot-int-authz-001)
  * [権限昇格 (Privilege Escalation) (IOT-INT-AUTHZ-002)](#privilege-escalation-iot-int-authz-002)
* [情報収集 (Information Gathering) (IOT-INT-INFO)](#information-gathering-iot-int-info)
  * [実装内容の開示 (Disclosure of Implementation Details) (IOT-INT-INFO-001)](#disclosure-of-implementation-details-iot-int-info-001)
  * [エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-INT-INFO-002)](#disclosure-of-ecosystem-details-iot-int-info-002)
  * [ユーザーデータの開示 (Disclosure of User Data) (IOT-INT-INFO-003)](#disclosure-of-user-data-iot-int-info-003)
* [構成とパッチ管理 (Configuration and Patch Management) (IOT-INT-CONF)](#configuration-and-patch-management-iot-int-conf)
  * [古いソフトウェアの使用 (Usage of Outdated Software) (IOT-INT-CONF-001)](#usage-of-outdated-software-iot-int-conf-001)
  * [不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-INT-CONF-002)](#presence-of-unnecessary-software-and-functionalities-iot-int-conf-002)
* [シークレット (Secrets) (IOT-INT-SCRT)](#secrets-iot-int-scrt)
  * [機密データへのアクセス (Access to Confidential Data) (IOT-INT-SCRT-001)](#access-to-confidential-data-iot-int-scrt-001)
* [暗号技術 (Cryptography) (IOT-INT-CRYPT)](#cryptography-iot-int-crypt)
  * [脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-INT-CRYPT-001)](#usage-of-weak-cryptographic-algorithms-iot-int-crypt-001)
* [ビジネスロジック (Business Logic) (IOT-INT-LOGIC)](#business-logic-iot-int-logic)
  * [意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic) (IOT-INT-LOGIC-001)](#circumvention-of-the-intended-business-logic-iot-int-logic-001)
* [入力バリデーション (Input Validation) (IOT-INT-INVAL)](#input-validation-iot-int-inval)
  * [不十分な入力バリデーション (Insufficient Input Validation) (IOT-INT-INVAL-001)](#insufficient-input-validation-iot-int-inval-001)
  * [コードインジェクションやコマンドインジェクション (Code or Command Injection) (IOT-INT-INVAL-002)](#code-or-command-injection-iot-int-inval-002)



## 概要

This section includes test cases and categories for the component internal interface. Similar to the processing unit and the memory, the internal interface is a device-internal element that can only be accessed with *PA-4*. Establishing a direct connection to an internal interface might require specific hardware equipment (e.g., a debugging board, an oscilloscope or test probes).

In regards to test case categories that are relevant for an internal interface, the following were identified:

- **Authorization:** Focuses on vulnerabilities that allow to get unauthorized access to the internal interface or to elevate privileges in order to access restricted functionalities.

- **Information Gathering:** Focuses on information that is handled by the internal interface and that might be disclosed to potential attackers if not being properly protected or removed.

- **Configuration and Patch Management:** Focuses on vulnerabilities and issues in the configuration of an internal interface and its software components.

- **Secrets:** Focuses on secrets that are handled by the internal interface in an insecure manner.

- **Cryptography:** Focuses on vulnerabilities in the cryptographic implementation.

- **Business Logic:** Focuses on vulnerabilities in the implementation of the internal interface.

- **Input Validation:** Focuses on vulnerabilities regarding the validation and processing of input from untrustworthy sources.



## 認可 (Authorization) (IOT-INT-AUTHZ)

Depending on the access model for a given device, only certain individuals might be allowed to access an internal interface. Thus, proper authentication and authorization procedures need to be in place, which ensure that only authorized users can get access.

### インタフェースへの認可されていないアクセス (Unauthorized Access to the Interface) (IOT-INT-AUTHZ-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i></tr>
</table>
**要旨**

Depending on the specific implementation of a given device, access to an internal interface might be restricted to individuals with a certain logical access level, e.g., *LA-2*, *LA-3* or *LA-4*. If the device fails to correctly verify access permissions, any attacker (*LA-1*) might be able to get access.

**テスト目的**

- It must be checked if authorization checks for access to the internal interface are implemented.

- In case that authorization checks are in place, it must be determined whether there is a way to bypass them.

**対応策**

Proper authorization checks need to be implemented, which ensure that access to the internal interface is only possible for authorized individuals.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-AUTHZ-001](../data_exchange_services/README.md#unauthorized-access-to-the-data-exchange-service-iot-des-authz-001).

### 権限昇格 (Privilege Escalation) (IOT-INT-AUTHZ-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-2</i> - <i>LA-3</i><br>(depending on the access model for the given device)</tr>
</table>
**要旨**

Depending on the specific implementation of a given device, access to some functionalities via an internal interface might be restricted to individuals with a certain logical access level, e.g., *LA-3* or *LA-4*. If the interface fails to correctly verify access permissions, an attacker with a lower logical access level than intended might be able to get access to the restricted functionalities.

**テスト目的**

- Based on [IOT-INT-AUTHZ-001](#unauthorized-access-to-the-interface-iot-int-authz-001), it must be determined whether there is a way to elevate the given access privileges and thus to access restricted functionalities.

**対応策**

Proper authorization checks need to be implemented, which ensure that access to restricted functionalities is only possible for individuals with the required logical access levels.

**参考情報**

This test case is based on: [IOT-DES-AUTHZ-002](../data_exchange_services/README.md#privilege-escalation-iot-des-authz-002).



## 情報収集 (Information Gathering) (IOT-INT-INFO)

Internal interfaces might disclose various information, which could reveal details regarding the inner workings of the device or the surrounding IoT ecosystem to potential attackers. This could enable and facilitate further, more advanced attacks.

### 実装内容の開示 (Disclosure of Implementation Details) (IOT-INT-INFO-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

If details about the implementation, e.g., algorithms in use or the authentication procedure, are available to potential attackers, flaws and entry points for successful attacks are easier to detect. While the disclosure of such details alone is not considered to be a vulnerability, it facilitates the identification of potential attack vectors, thus allowing an attacker to exploit insecure implementations faster.

For example, relevant information might be included in service banners, console output or error messages.

**テスト目的**

- Accessible details regarding the implementation must be assessed in order to prepare further tests. For example, this includes:

 - Cryptographic algorithms in use

 - Authentication and authorization mechanisms

 - Local paths and environment details

**対応策**

As mentioned above, the disclosure of such information is not considered a vulnerability. However, in order to impede exploitation attempts, only information, necessary for the device operation, should be displayed.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-FW-INFO-002](../firmware/README.md#disclosure-of-implementation-details-iot-fw-info-002).

### エコシステム内容の開示 (Disclosure of Ecosystem Details) (IOT-INT-INFO-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br> depending on the access model for the given device)</tr>
</table>

**要旨**

An internal interface might disclose information about the surrounding IoT ecosystem, e.g., sensitive URLs, IP addresses, software in use etc. An attacker might be able to use this information to prepare and execute attacks against the ecosystem.

For example, relevant information might be included in service banners, console output or error messages.

**テスト目的**

- It must be determined if the internal interface discloses relevant information about the surrounding ecosystem.

**対応策**

The disclosure of information should be reduced to the minimum, which is required for operating the device. The disclosed information it has tobe assessed and all unnecessarily included data should be removed.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-FW-INFO-003](../firmware/README.md#disclosure-of-ecosystem-details-iot-fw-info-003).

### ユーザーデータの開示 (Disclosure of User Data) (IOT-INT-INFO-003)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

During runtime, a device is accumulating and processing data of different kinds, such as personal data of its users. If this data is disclosed, an attacker might be able to get access to it.

**テスト目的**

- It has to be checked whether user data can be accessed by unauthorized individuals.

**対応策**

Access to user data should only be granted to individuals and processes that need to have access to it. No unauthorized or not properly authorized individual should be able to access user data.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-FW[INST]-INFO-001](../firmware/installed_firmware.md#disclosure-of-user-data-iot-fw[inst]-info-001).



## 構成とパッチ管理 (Configuration and Patch Management) (IOT-INT-CONF)

Since IoT devices can have a long lifespan, it is important to make sure that the software, running on the device, is regularly updated in order to apply the latest security patches. The update process of the firmware itself will be covered by [IOT-FW[UPDT]](../firmware/firmware_update_mechanism.md). However, it must also be verified that software packages, which are running on the device and listening on interfaces, are up-to-date as well.

### 古いソフトウェアの使用 (Usage of Outdated Software) (IOT-INT-CONF-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

Every piece of software is potentially vulnerable to attacks. For example, coding errors could lead to undefined program behavior, which then can be exploited by an attacker to gain access to data, processed by the application, or to perform actions in the context of the runtime environment. Furthermore, vulnerabilities in the used frameworks, libraries and other technologies might also affect the security level of a given piece of software.

Usually, developers release an update once a vulnerability was detected in their software. These updates should be installed as soon as possible in order to reduce the probability of successful attacks. Otherwise, attackers could use known vulnerabilities to perform attacks against the device.

**テスト目的**

- The version identifiers of installed software packages as well as libraries and frameworks in use must be determined.

- Based on the detected version identifiers, it must be determined if the software version in use is up-to-date, e.g., by consulting the website of the software developer or public repositories.

- By using vulnerability databases, such as the [National Vulnerability Database](https://nvd.nist.gov) of the NIST, it has to be checked whether any vulnerabilities are known for the detected software versions.

**対応策**

No outdated software packages should be running on the device. A proper patch management process, which ensures that applicable updates are installed once being available, should be implemented.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-FW-CONF-001](../firmware/README.md#usage-of-outdated-software-iot-fw-conf-001).

### 不必要なソフトウェアや機能の存在 (Presence of Unnecessary Software and Functionalities) (IOT-INT-CONF-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

Every piece of software, which is available on the device, broadens the attack surface since it might be used to perform attacks against the device. Even if the installed software is up-to-date, it might still be affected by unpublished vulnerabilities. It is also possible that a software program facilitates an attack without being vulnerable, e.g., by providing access to specific files or processes.

**テスト目的**

- A list of functionalities, available via the interface, should be assembled.

- Based on the device documentation, its behavior and the intended use cases, it must be determined whether any of the available functionalities are not mandatory for the device operation.

**対応策**

The attack surface should be minimized as much as possible by removing or disabling every software that is not required for the device operation.

Especially in case of general-purpose operating systems, such as Windows and Linux systems, it must be ensured that any unnecessary operating system features are disabled.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-FW-CONF-002](../firmware/README.md#presence-of-unnecessary-software-and-functionalities-iot-fw-conf-002).



## シークレット (Secrets) (IOT-INT-SCRT)

IoT devices are often operated outside of the control space of their manufacturer. Still, they need to establish connections to other network nodes within the IoT ecosystem, e.g., to request and receive firmware updates or to send data to a cloud API. Hence, it might be required that the device has to provide some kind of authentication credential or secret. These secrets need to be stored on the device in a secure manner to prevent them from being stolen and used to impersonate the device.

### 機密データへのアクセス (Access to Confidential Data) (IOT-INT-SCRT-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

Malfunctions, unintended behavior or improper implementation of an internal interface might enable an attacker to get access to secrets.

**テスト目的**

- It has to be determined whether secrets can be accessed via the internal interface.

**対応策**

Access to secrets should only be granted to individuals and processes that need to have access to them. No unauthorized or not properly authorized individual should be able to access secrets.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-SCRT-001](../data_exchange_services/README.md#access-to-confidential-data-iot-des-scrt-001).



## 暗号技術 (Cryptography) (IOT-INT-CRYPT)

Many IoT devices need to implement cryptographic algorithms, e.g., to securely store sensitive data, for authentication purposes or to receive and verify encrypted data from other network nodes. Failing to implement secure, state of the art cryptography might lead to the exposure of sensitive data, device malfunctions or loss of control over the device.

### 脆弱な暗号アルゴリズムの使用 (Usage of Weak Cryptographic Algorithms) (IOT-INT-CRYPT-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

Cryptography can be implemented in various ways. However, due to evolving technologies, new algorithms and more computing power becoming available, many old cryptographic algorithms are nowadays considered weak or insecure. Thus, either new and stronger cryptographic algorithms have to be used or existing algorithms must be adapted, e.g., by increasing the key length or using alternative modes of operation.

The usage of weak cryptographic algorithms might allow an attacker to recover the plaintext from a given ciphertext in a timely manner.

**テスト目的**

- The data, processed by the interface, must be checked for the presence of encrypted data segments. In case that encrypted data segments are found, it must be checked whether the cryptographic algorithms in use can be identified.

- Furthermore, based on [IOT-INT-INFO-001](#disclosure-of-implementation-details-iot-int-info-001), it must be checked whether headers, system messages etc. disclose the usage of certain cryptographic algorithms.

- In case that cryptographic algorithms can be identified, it must be determined whether the algorithms in use and their configuration are providing a sufficient level of security at the time of testing, e.g., by consulting cryptography guidelines like the technical guideline [TR-02102-1](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TG02102/BSI-TR-02102-1.pdf __blob=publicationFile&v=10) by the BSI.

**対応策**

Only strong, state of the art cryptographic algorithms should be used. Furthermore, these algorithms must be used in a secure manner by setting proper parameters, such as an appropriate key length or mode of operation.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-FW-CRYPT-001](../firmware/README.md#usage-of-weak-cryptographic-algorithms-iot-fw-crypt-001).



## ビジネスロジック (Business Logic) (IOT-INT-LOGIC)

Even if all other aspects of the internal interface are securely implemented and configured, issues in the underlying logic itself might render the device vulnerable to attacks. Thus, it must be verified if the internal interface and its functionalities are working as intended and if exceptions are detected and properly handled.

### 意図したビジネスロジックの迂回 (Circumvention of the Intended Business Logic) (IOT-INT-LOGIC-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

Flaws in the implementation of the business logic might result in unintended behavior or malfunctions of the device. For example, if an attacker intentionally misses to provide relevant input data or tries to skip or change important steps in the processing workflow the device might end up in an unknown, potentially insecure state.

**テスト目的**

- Based on the specific business logic implementation, it has to be determined whether deviations from the defined workflows are properly detected and handled.

**対応策**

The device should not end up in an unknown state. Anomalies in the workflow must be detected and exceptions have to be handled properly.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-LOGIC-001](../data_exchange_services/README.md#circumvention-of-the-intended-business-logic-iot-des-logic-001).



## 入力バリデーション (Input Validation) (IOT-INT-INVAL)

In order to ensure that only valid and well-formed data enters the processing flows of a device, the input from a all untrustworthy sources, e.g., users or external systems, has to be verified and validated.

### 不十分な入力バリデーション (Insufficient Input Validation) (IOT-INT-INVAL-001)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

If no input validation is performed or only an insufficient input validation mechanism is in place an attacker might be able to submit arbitrary and malformed data. Thus, the process, which handles the user input, or another downstream component might stop working properly due to not being able to process the data. This could result in malfunctions that might enable an attacker to manipulate the device behavior or render it unavailable.

**テスト目的**

- It must be determined whether input to the internal interface is validated.

- In case that an input validation mechanism is implemented, it hast to be checked if there is a way to submit data, which does not comply with the intended data structure and value ranges.

**対応策**

The device has to validate all input from untrustworthy sources. Malformed or otherwise invalid input must either be rejected or converted into a proper data structure, e.g., by encoding the input. However, it must be ensured that the input is not interpreted or executed when converting it.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-INVAL-001](../data_exchange_services/README.md#insufficient-input-validation-iot-des-inval-001).

### コードインジェクションやコマンドインジェクション (Code or Command Injection) (IOT-INT-INVAL-002)
**必要なアクセスレベル**

<table width="100%">
	<tr valign="top">
		<th width="1%" align="left">Physical</th>
 <td><i>PA-4</i></td>
	</tr>
	<tr valign="top">
		<th align="left">Logical</th>
		<td><i>LA-1</i> - <i>LA-4</i><br>(depending on the access model for the given device)</tr>
</table>

**要旨**

If no input validation is performed or only an insufficient input validation mechanism is in place an attacker might be able to submit code or commands, which then might be executed by the system. It strictly depends on the specific implementation of the device and the internal interface which code and commands are potentially executable. For example, a possible injection attack is OS command injection.

**テスト目的**

- Based on [IOT-INT-INVAL-001](#insufficient-input-validation-iot-int-inval-001), it must be checked whether it is possible to submit code or commands, which are then executed by the system.

**対応策**

The device has to validate all input from untrustworthy sources. Malformed or otherwise invalid input must either be rejected or converted into a proper data structure, e.g., by encoding the input. However, it must be ensured that the input is not interpreted or executed when converting it.

**参考情報**

For this test case, data from the following sources was consolidated:

* ["IoT Pentesting Guide"][iot_pentesting_guide] by Aditya Gupta
* ["IoT Penetration Testing Cookbook"][iot_penetration_testing_cookbook] by Aaron Guzman and Aditya Gupta
* ["The IoT Hacker's Handbook"][iot_hackers_handbook] by Aditya Gupta
* ["Practical IoT Hacking"][practical_iot_hacking] by Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, and Beau Woods
* Key aspects of testing of the T-Systems Multimedia Solutions GmbH

This test case is based on: [IOT-DES-INVAL-002](../data_exchange_services/README.md#code-or-command-injection-iot-des-inval-002).



[iot_pentesting_guide]: https://www.iotpentestingguide.com	"IoT Pentesting Guide"
[iot_penetration_testing_cookbook]: https://www.packtpub.com/product/iot-penetration-testing-cookbook/9781787280571	"IoT Penetration Testing Cookbook"
[iot_hackers_handbook]: https://link.springer.com/book/10.1007/978-1-4842-4300-8	"The IoT Hacker's Handbook"
[practical_iot_hacking]: https://nostarch.com/practical-iot-hacking	"Practical IoT Hacking"
