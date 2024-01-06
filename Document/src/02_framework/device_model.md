# 2.1. IoT デバイスモデル (IoT Device Model)

この章は IoT デバイスの一般的な構造を表す IoT デバイスモデルに焦点を当てます。デバイスモデルの作成は、ソリューションアプローチで定義された目標を達成するための最初のステップです ([1. はじめに](../01_introduction/README.md) 参照)。[2.2. 攻撃者モデル](./attacker_model.md), [2.3. 方法論](./methodology.md), [3. テストケースカテゴリ](../03_test_cases/README.md) で説明される以降のすべてのステップはデバイスモデルに基づいています。



## 関連研究

デバイスモデルは IoT プラットフォームのリファレンスアーキテクチャに基づいて構築されました。さらに、デバイスモデルはセキュリティのコンテキストで使用されるため、攻撃対象領域の形で潜在的な攻撃ベクトルも考慮しています。これらは以下の関連研究によって概説されています。

-   **["Comparison of IoT Platform Architectures: A Field Study based on a Reference Architecture"][reference_architecture]:** このペーパーの目的は、IoT エコシステムのリファレンスアーキテクチャを提案することです。このリファレンスアーキテクチャは「リファレンスアーキテクチャの目的は、統一された抽象的な用語集として機能し、さまざまなプラットフォームの比較を容易にすることであるため、意図的に抽象的なままにしています」 ([出典][reference_architecture])。このガイドで開発されたデバイスモデルは、特定の実装や設計に関係なく、さまざまな IoT デバイスの統一モデルとしても機能する必要があるため、Guth らの後のリファレンスモデル ([出典][reference_architecture]) を基礎としました。とはいえ、Guth らの後のモデル ([出典][reference_architecture]) は IoT デバイス自体の観点からみると表面的なものです。ここではデバイスを単一のコンポーネントとして示しており、(ドライバ以外の) 部品をそれ以上に区別していません。そのため、このガイドではテスト範囲 (特定のデバイス部品を含むか含まないか) をきめ細かく定義できないため、これでは十分ではありません。このガイドで紹介されているモデルでは、IoT デバイスの個々の部品をさらに区別するためにいくつかの調整が行われています。

-   **["IoT Attack Surface Areas Project"][owasp_iot_attack_surface_areas]:** OWASP はウェブやモバイルアプリケーションセキュリティなど、いくつかの技術分野におけるペネトレーションテスト方法論と一般的なセキュリティリスクのコレクション ("OWASP Top 10" とよばれる) を定期的に公表しています。その人気により、ペネトレーションテストに関する主要な情報源の一つとなっています。2014 年と 2018 年には、OWASP は IoT 分野に関するセキュリティリスクの Top 10 も公表しました。"IoT Attack Surface Areas Project" で言及されている対象領域は、潜在的な攻撃者に狙われる可能性のある IoT ソリューションの部品を表します。このリストは IoT デバイスおよび IoT エコシステム全般に関して多くの潜在的な攻撃ベクトルをすでにカバーしているという事実により、このガイド内で提案されているデバイスモデルの基礎としても使用しました。ただし、特にハードウェア側の観点から、IoT デバイス実装の詳細をさらに区別するためにいくつかの調整が行われました。さらに、"IoT Attack Surface Areas Project" はデバイス部品の単純なリストで構成しているだけであり、これらの部品が他のそれぞれとどのようにやり取りするのかを規定していません。また、デバイスの各部品 (またはそれぞれの攻撃対象領域) の特性を定義していないため、たとえば「デバイスメモリ」と「ローカルデータストレージ」などを区別することが困難になります。 ([出典][owasp_iot_attack_surface_areas])



## Device Boundaries

In order to distinguish between components belonging to an IoT device and components of the surrounding IoT ecosystem, it is necessary to first define the boundaries of an IoT device. An IoT device is generally encompassed by an enclosure of some kind, which (physically) separates device-internal lements from device-external elements.

Interactions between internal and external elements are only possible via interfaces. Within this guide, these interfaces are not considered to be part of the enclosure. Instead, those interfaces will be categorized individually (see [Interfaces](#interfaces)).

As will be explained in the next section, the term "component" refers to an item that can be the subject of a penetration test. Thus, device-internal elements and interfaces are considered components within this guide.



## Components

As introduced in the previous sections, the proposed device model should provide a generalized selection of parts that IoT devices consist of. These parts will be referred to as components. Every component is a piece of soft- and/or hardware that, in theory, can be tested individually. The penetration test scope for an IoT device can therefore be defined as a list of components.

### Device-Internal Elements

Every device-internal element is a component residing inside the device enclosure. Thus, they are part of the IoT device. IoT devices usually comprise the following internal elements, all of which are mentioned in the list of attack surfaces composed by OWASP ([source][owasp_iot_attack_surface_areas]):

- **Processing unit:** The processing unit, also called processor, is responsible for managing and performing data processing tasks. These tasks are defined as a sequence of instructions that are loaded from the memory. A device has at least a central processing unit handling its core functionalities (defined by the firmware). However, more complex devices might also be equipped with further processing units that are assigned to specific subtasks. A special kind of processor are microprocessors, built on a single circuit. Microcontrollers are microprocessors, which also have analog and digital in- and outputs. They are typically used to control the behavior of a device and are often used in the embedded field. ([source][ekomp_processor])

  *Examples: x86 processor, ARM processor, AVR processor*

- **Memory:** Memory is used to store data, such as programs (instructions for a processing unit) and information, in binary form. Depending on the type of memory, it is used to temporarily store data while being processed by a processing unit (primary memory or cache) or to permanently store data on a device even while the device is turned off (secondary memory). A special kind of secondary memory is flash memory. It is commonly used in many devices because it is energy-saving, develops less heat and is less susceptible to vibration and magnetic fields due to the lack of moving parts. Flash memory is based on semiconductor technology and able to provide fast and permanent access to data (read, write, delete). ([source][ekomp_flash_memory], [source][ekomp_memory])

  *Examples: EEPROM, flash memory*

- **Firmware:** "Firmware is a software program or set of instructions programmed on a hardware device" ([source][tech_terms_firmware]). It is used to control the device and the communication between device-internal and -external elements (data in- and output via data exchange services). Firmware is stored on a memory and executed by a processing unit. In regards of device firmware, the following components might be potential targets for a penetration test:

  -   **Installed firmware:** Installed firmware refers to firmware that is already installed on a device. It might be the target of dynamic analyses and usually handles the storage and processing of sensitive user data.

  -   **Firmware update mechanism:** A firmware update mechanism is part of the firmware and defines how firmware updates, in the form of firmware packages, can be installed on a device. A crucial responsibility of a firmware update process is to ensure that only proper firmware packages can be installed and executed.[^1]

  *Examples: OS, RTOS, bare-metal embedded firmware*

- **Data exchange service:** Data exchange services refer to programs or parts of programs, used to transfer data between two or more components via an interface (e.g., network, bus). These services are part of the firmware and can be used to transmit data, receive data or both.

  *Examples: network service, debug service, bus listener*

[^1]: For performing a test of a firmware update mechanism, a firmware package is required. Due to the fact that a firmware package could also be inspected separately, it could be considered a component as well. However, since this guide focuses on device-internal elements and device interfaces only, firmware packages are not in scope. Contrary to installed firmware, an update package also includes the firmware header, which might include important data.

### Interfaces

Interfaces are required to connect two or more components with each other. Interactions between device-internal elements or between device-internal and device-external elements are only possible via interfaces. Based on which components are connected by an interface, it can be categorized as a machine-to-machine or human-to-machine interface. As long as at least one of the connected components is a device-internal element, the interface itself is also part of the device.

Within this guide, the following kinds of interfaces will be differentiated, all of which are either directly or indirectly mentioned in the list of attack surfaces, composed by OWASP ([source][owasp_iot_attack_surface_areas]):

- **Internal interfaces (machine-to-machine):** These interfaces are used to establish a connection between device-internal elements and are not accessible from outside the device enclosure.

  *Examples: JTAG, UART, SPI*

- **Physical interfaces (machine-to-machine):** Physical interfaces  are used to establish a connection between device-internal and -external elements, based on a physical connection between the components or the respective interfaces of those components. Therefore, physical interfaces require a socket or a port, built into the device enclosure and thus are accessible from outside the device.

  *Examples: USB, Ethernet*

- **Wireless interfaces (machine-to-machine):** Similar to physical interfaces, wireless interfaces are also used to establish a connection between device-internal and -external elements. However, the connection between wireless interfaces is not based on a physical connection, but on radio waves, optical signals or other wireless technologies. Wireless interfaces are accessible from outside the device, usually from a greater distance than physical interfaces.

  *Examples: Wi-Fi, Bluetooth, BLE, ZigBee*

- **User interfaces (human-to-machine):** In contrast to all other above-mentioned interfaces, user interfaces are not utilized to establish a connection between two machines. Instead, their purpose is to allow interactions between device-internal elements and a user. These interactions can either be based on a physical connection, e.g., in case of a touch display, or wireless connections, e.g., in case of a camera or microphone.

  *Examples: touch display, camera, microphone, local web application (hosted on the device)*



## Device Model Scheme

The device model is a combination of all above-mentioned components and can be seen in the figure below. It must be noted that, even though cardinalities were not included for better readability, more than one instance of each component might be built into an IoT device. 



![IoT Device Model](../img/IoT_Device_Model.jpg)



Other models, e.g., the ones mentioned in [Related Work](#related-work), include sensors and actors as components of a device. Within this guide, sensors and actors are considered physical, wireless or user interfaces respectively because they enable interactions between device-internal and -external elements or users via physical (e.g., touch sensor, door control) or wireless connections (e.g., microphone, temperature sensor).

In some cases, it is also possible that devices comprise parts which can be considered devices themselves (i.e., nested devices). It then depends on the perspective of the observer which interfaces are classified as internal and external. The determining factor are the boundaries between the observer and the interface (see [Device Boundaries](#device-boundaries), [Device-Internal Elements](#device-internal-elements) and [Interfaces](#interfaces)).

Overall, the device model, which was specifically developed in the context of this guide, can be used to create and share abstract representations of various different IoT devices. Contrary to other models, this one solely focuses on the IoT device and the components it is built of. Hence, the model allows to describe device implementations in a more detailed manner. In combination with the models and concepts, developed in the following chapters, it is possible to compile a list of applicable test cases for any given device regardless of the specific technologies or standards that are implemented.



[reference_architecture]: https://ieeexplore.ieee.org/document/7872918	"Comparison of IoT platform architectures: A field study based on a reference architecture"
[owasp_iot_attack_surface_areas]: https://wiki.owasp.org/index.php/OWASP_Internet_of_Things_Project#tab=IoT_Attack_Surface_Areas	"OWASP IoT Attack Surface Areas Project"
[tech_terms_firmware]: https://techterms.com/definition/firmware	"TechTerms.com"
[ekomp_processor]: https://www.elektronik-kompendium.de/sites/com/0309161.htm	"CPU - Central Processing Unit / Hauptprozessor"
[ekomp_flash_memory]: https://www.elektronik-kompendium.de/sites/com/0312261.htm	"Flash-Speicher / Flash-Memory"
[ekomp_memory]: https://www.elektronik-kompendium.de/sites/com/1812051.htm	"Speicherarchitektur"
