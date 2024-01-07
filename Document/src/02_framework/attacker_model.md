# 2.2. 攻撃者モデル (Attacker Model)

この章では、特定の IoT デバイスに対する脅威であると想定される潜在的な攻撃者に基づく、テストケースの選択方法について説明します。STRIDE モデルのような、完全な脅威およびリスクのモデリングアプローチとは対照的に、このガイドで使用する攻撃者モデルは IoT デバイスに対する定義および選択のためのより合理的な手順を示しています。

フォーマルな脅威およびリスクのモデリングアプローチを使用しない理由は以下のとおりです。

-   脅威およびリスクのモデリングは通常、一つの特定の実装設計に焦点を当てています。そのため、特定された脅威およびリスクは特定のソリューションやデバイスの特定の条件に基づいており、異なるソリューションを相互に比較することは困難になります。

-   フォーマルな脅威およびリスクの分析を実行するには、かなりの時間が必要になり、対象が複雑になるとさらに時間がかかります。フォーマルな脅威およびリスクの分析をペネトレーションテストの必須要件とすると、テスト期間が長くなり、その結果、テストごとの費用が高くなります。

潜在的な攻撃者の全貌は匿名のグローバルな攻撃者から特権を持つ個人やデバイスのユーザーまで多岐にわたります。以下のセクションで説明するように、テストの観点を表す最小および最大のアクセス要件を定義することで、攻撃者のリストを絞り込むことができます。すべてのデバイスコンポーネントとテストケースには、それぞれのテストを実行するために必要なアクセスレベルでタグ付けされます。そのため、テスト範囲内のデバイスコンポーネントのリストと適用可能なテストケースのリストは、デバイスモデルによって得られた結果に攻撃者モデルを適用した結果になります。

この章では、「IoT デバイス」という用語は単一のデバイスまたはデバイスタイプを指しますが、このガイドの他の章では、IoT デバイス全般を指すことに注意しなければなりません。




## 攻撃者モデルの概念的基盤

この攻撃モデルはアクセス能力に基づいて潜在的な攻撃者のグループを特徴付けます [^1] 。この攻撃者モデルに使用されるメトリックは [CVSS][cvss] のメトリックに基づいています。CVSS は主にウェブアプリケーションとコンピュータネットワーキングの分野で脆弱性の深刻度を評価するためにしようされますが、特定のセキュリティ問題を悪用するための攻撃者の能力と条件を評価するためのわかりやすいアプローチを実装しています。CVSS に類似したモデルを使用するもう一つの利点は、多くのセキュリティ専門家がすでに CVSS を利用していることです。そのため、多くのテスト担当者や製造業者/事業者がこのシステムに精通していることも、この攻撃者モデルの受け入れに寄与しています。

CVSS は以下の悪用可能性のメトリックを定義しています。

-   **攻撃元区分 (Attack Vector):** 「このメトリックは脆弱性悪用が可能となるコンテキストを反映しています」 ([出典][cvss])。このメトリックの値はネットワークアクセス (インターネット経由など) から物理アクセスまでの範囲になります。攻撃者モデルでは、このメトリックは物理アクセスレベルに反映されます。

-   **攻撃条件の複雑さ (Attack Complexity):** 「このメトリックは脆弱性を悪用するために存在しなければならない攻撃者の制御を超える条件を表しています」 ([出典][cvss])。攻撃条件の複雑さは「攻撃者の制御を超える条件」 ([出典][cvss]) を指し、潜在的な攻撃者の分類には関係ないため、攻撃者モデルでは使用されません。

-   **必要な特権レベル (Privileges Required):** 「このメトリックは攻撃者が脆弱性の悪用を成功させるまでに保有しなければならない特権レベルを表しています」 ([出典][cvss])。このメトリックの値は不要 (特権なし) から高までの範囲になります。必要な特権は攻撃者モデルでは認可アクセスレベルで表しています。

-   **ユーザ関与レベル (User interaction):** 「このメトリックは脆弱なコンポーネントの侵害に成功するために攻撃者以外の人間のユーザーが参加する要件を捕捉します」 ([出典][cvss])。正当なユーザーによるやり取りの必要性は脆弱性の悪用可能性に関係しますが、適用可能なテストケースの選択には関係しないため、攻撃者モデルでは考慮されません。

[^1]: IT セキュリティに関して、攻撃者は通常、攻撃性やリソース (処理能力、時間、資金) などのさらなる要因に基づいて特徴付けられます。しかし、これらの要因は適用可能なテストケースの選択に影響を及ぼさないか、わずかな影響しか及ぼしません。



## アクセスレベル

Within this attacker model, access levels are a measure for the relation between a certain group of individuals (access group) and the IoT device. They describe how individuals of the access group are intended to be able to interact with the device. These can either be physical interactions or logical authorization interactions.

The degree of how close individuals can get to the device is measured by the physical access level. The physical access level is an adaption of the CVSS metric "attack vector" and it reflects the physical context that is required to perform attacks against a target device. Therefore, some of the original values from the CVSS were used (network, local, physical). However, the description of local access was adjusted in regards of the focus on the physical context. Additionally, the physical access as defined in the CVSS was split into two levels: non-invasive and invasive physical access. The reason for this is that some IoT devices are protected with special measures that restrict access to device-internal elements, e.g., locked or sealed enclosures. In this case, attackers might not be able to access device-internals in a reasonable amount of time, thus they only have non-invasive physical access. Other devices have enclosures that can be opened in a short time, e.g., by removing screws. Thus, attackers could access device-internals, therefore gaining invasive physical access. Overall, the physical access level can be affected by factors like geographical location, building security or the device enclosure.

The following physical access levels are defined:

1.  **Remote access (*PA-1*):** There is an arbitrary physical distance between an individual and the device. An attacker with remote access can be located anywhere in the world, which usually means that the device is directly accessible via a Global Area Network (GAN).

2.  **Local access (*PA-2*):** There is a limited physical distance[^2] between an individual and the device, but direct physical interactions are not possible. An attacker with local access can use the device from close proximity, which usually means that the device is directly accessible via a Local Area Network (LAN) or Wireless Local Area Network (WLAN).

3.  **Non-invasive access (*PA-3*):** There is no physical distance between an individual and the device, but the individual cannot directly access device-internal elements in a physical manner (i.e., cannot easily open the device enclosure).

4.  **Invasive access (*PA-4*):** There is no physical distance between an individual and the device and the individual can directly access device-internal elements in a physical manner (i.e., open the device enclosure).

The digital privileges of individuals are measured by the authorization access level. The authorization access level is an adaption of the CVSS metric "privileges required". In addition to the values, defined in the CVSS, another level of privileges, called manufacturer-level access, was added on top of the high privileges. Contrary to web applications and computer networks, which are usually operated from within the control zone of the operator (e.g., within a data center), IoT devices are often operated outside that control zone. Established methods for securing maintenance and debugging access (e.g., restricting maintenance access to pre-defined subnets, IP addresses or physical ports in the data center) can not always be applied. Hence, attacks against a device with manufacturer-level access might be possible. Overall, the authorization access level can be affected by factors like policies or role-based access models.

The following authorization access levels are defined:

1.  **Unauthorized access (*AA-1*):** An individual can get anonymous access to the device component. Attackers with anonymous access can be any unregistered user.

2.  **Low-privileged access (*AA-2*):** An individual can only get access to the device component, if it is authenticated and in possession of standard authorization privileges. Attackers with low-privileged access can be any registered user.

3.  **High-privileged access (*AA-3*):** An individual can only get access to the device component, if it is authenticated and in possession of extensive privileges. The term "extensive privileges" means that individuals have access to restricted functionalities that are not available to all registered users of the device component (e.g., configuration settings).

4.  **Manufacturer-level access (*AA-4*):** An individual can only get access to the device component, if it is authenticated and in possession of manufacturer-level authorization privileges. Contrary to high-privileged access, manufacturer-level access is not restricted in any way and includes, e.g., debugging access for developers of the device, access to the source code or root-level access to the firmware.

[^2]: Limited physical distance is not restricted to a specic maximum value per se. Depending on the technologies in use, the maximum distance might range from a few meters (e.g., in case of Bluetooth) to a few kilometers (e.g., in case of LTE).



## デバイスコンポーネントとアクセスレベルのマッピング

The perspective of the testers during the test will be determined by minimal and maximal access levels, chosen as a baseline for the test. Physical and authorization access levels have different impacts on the penetration test and its scope.

**Physical access level:**

-   The physical access level refers to the device as a whole. Thus, some physical access levels directly define that certain device components can not be tested with the given level since an attacker could not interact with these components at all. The relation between physical access levels and device components is shown in the table below.

-   Based on the specific requirements of a manufacturer or operator, the minimal and/or maximal physical access levels might be hard boundaries for the test execution since the contractee might want to specifically exclude certain tests, e.g., those which require invasive physical access.

**Authorization access level:**

-   Since authorization access might be handled differently across multiple device components, the authorization access level rather refers to access to an individual component than to the device as a whole. Thus, the impact of authorization access levels on the test scope always depends on the specific implementation of the business logic and the authorization/permission scheme per component.

-   There is no reason for selecting a minimal authorization access level for the test perspective since evaluating whether it is possible to get access to (parts of) the device with lower privileges than intended should be part of the test.

All in all, the attacker model can be used to create an abstract representation of potential attackers. It can be used to describe which kind of attackers is considered a threat to a given device in its operation environment. Contrary to other methodologies and models, this one can be used in a more streamlined manner, thus being more efficient, e.g., compared to full threat and risk analysis approaches. It is also takes the specifics of the IoT context more into account than the CVSS, which it is based on. In combination with the device model, it is possible to define the test scope and test perspective, thereby determining which test cases can and shall be performed.

| Component                 | PA-4  |   PA-3    |   PA-2    |   PA-1    |
| ------------------------- | :---: | :-------: | :-------: | :-------: |
| Processing Unit           | **✓** |           |           |           |
| Memory                    | **✓** |           |           |           |
| Installed Firmware        | **✓** | **?**[^3] | **?**[^3] | **?**[^3] |
| Firmware Update Mechanism | **✓** | **?**[^3] | **?**[^3] | **?**[^3] |
| Data Exchange Service     | **✓** | **?**[^4] | **?**[^4] | **?**[^4] |
| Internal Interface        | **✓** |           |           |           |
| Physical Interface        | **✓** |   **✓**   | **?**[^5] |           |
| Wireless Interface        | **✓** |   **✓**   |   **✓**   |           |
| User Interface            | **✓** |   **✓**   | **?**[^6] | **?**[^6] |

[^3]: Installed firmware and the firmware update mechanism might be testable with non-invasive (*PA-3*), local (*PA-2*) or remote physical access (*PA-1*), depending on how direct access to the firmware can be accomplished (e.g., via SSH).

[^4]: Data exchange services might be testable with non-invasive (*PA-3*), local (*PA-2*) or remote physical access (*PA-1*), depending on if they were designed for that kind of access, e.g., for remote control or monitoring purposes.

[^5]: Physical interfaces might be testable with local physical access (*PA-2*) under certain circumstances, e.g., if the physical interface is connected to a local network.

[^6]: User interfaces might be testable with local (*PA-2*) or remote physical access (*PA-1*), depending on if they were designed for that kind of access, e.g., for remote control or monitoring purposes.



[cvss]: https://www.first.org/cvss/	"Common Vulnerability Scoring System"
