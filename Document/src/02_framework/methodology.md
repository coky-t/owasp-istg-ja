# 2.3. テスト方法 (Testing Methodology)

この章では、IoT デバイスのペネトレーションテストを実行するための方法論について説明します。これは [2.1. IoT デバイスモデル](./device_model.md) および [2.2. 攻撃者モデル](./attacker_model.md) で提示されたコンセプトに基づいており、既存のペネトレーションテストワークフローおよびフレームワークとともに使用できる補助として機能します。この方法論は IoT デバイスのペネトレーションテスト時に実行する必要があるテストの重要な側面で構成しています。したがって、各個別のデバイスコンポーネントのためのテストケースのカタログを含みます。前の章で説明したように、適用可能なテストケースの具体的な選択は、この方法論のコンテキストで設計されたデバイスモデルと攻撃者モデルを適用した結果に依存します。

最初に、この方法論を他のワークフローにどのように統合できるか、また、どのステップでこの方法論のモデルとコンセプトを使用できるかについて説明します。次に、テスト時に適用できて、特定のテストケースに限定されない、選択されたテスト技法について説明します。最後に、テストケースカタログの構造コンセプトについて説明します。

他の IoT ペネトレーションテストフレームワークと比較して、この方法論はより一般的でありながら包括的なアプローチに従っています。特定のテクノロジや標準の詳細に制限されることなく、IoT コンテキスト (テストの重要な側面) に関連する特定のセキュリティ問題を定義しています。そのため、この方法論は他のフレームワークよりも柔軟であり、IoT 分野の変動性を考慮すると重要な利点となります。とはいえ、この方法論はさまざまなテクノロジに適用でき、さらなる詳細化の可能性を提供します。

複数のコンポーネントに適用されるテストケースはこの章には含まれないことに注意しなければいけません。テストケースの完全なリストは [3. テストケースカタログ](../03_test_cases/README.md) にあります。



## 他のワークフローやフレームワークとの統合

効率性を達成するため、この方法論を組み込むために既存のワークフローを大幅に調整する必要はありません。以下では BSI ペネトレーションテストモデル ([出典][bsi_pentest]) の例に基づいて、この方法論を他のフレームワークにどのように統合できるかを示します。この場合、テストワークフロー全体を変更する必要はありません。

このガイドで提案されている方法論は以下のステップを容易にするために使用できます。

-   **テスト範囲とテスト観点の明確化:** この方法論は、デバイスと攻撃者のモデルの形で共通の用語を確立することで、BSI モデル ([出典][bsi_pentest]) のフェーズ 1 および 3 で契約者とのテストの目的と条件の明確化をサポートし、コミュニケーションを円滑にします。さらに、デバイスモデルは、潜在的な攻撃ベクトルを特定するために、特定の IoT デバイスのアーキテクチャと比較できる汎用的なスキームを提供することで、フェーズ 2 でテストチームをサポートします。

-   **テスト実行と文書化:** テストケースカタログはアクティブテスト (BSI モデルのフェーズ 4 ([出典][bsi_pentest])) の際にテスト担当者のガイドラインとして機能します。テスト範囲とテスト観点に応じて、適用可能なテストケースが定義され、テストで実行される必要があります。そのため、テストカタログはチェックリストとして使用して、必須テストがすべて実行されたことを確認します。また、実行したテストケースをレポートで参照できるため、フェーズ 5 において再現可能な方法でテスト手順を透過的に文書化できます。



## 階層構造の説明

In the following, the overall structure of the test case catalog as well as the general layout of a test case will be defined.

### テストケースのカタログの構造

The catalog of test cases will follow a hierarchic (tree) structure. Starting from a single root node (IOT), each component of the device model will be represented as a child node, thereby forming its own subtree. Subsequently, further nodes will be added as children to the component nodes, eventually resulting in each test case being a leaf node. A unique identifier, incorporating this structure, will be assigned to each node, allowing to reference it in the test report or other documents.

The following hierarchic levels and types of nodes are defined:

- **Component:** The first main hierarchy level is the component (see [2.1. IoT Device Model](./device_model.md)). The type of component (device-internal element/interface) was not included in the hierarchy for the sake of simplicity and due to the lack of added value.

  *Short representation: 2 - 5 uppercase alphabetic characters*

  *Examples: IOT-PROC, IOT-MEM, IOT-FW, IOT-DES, IOT-INT, IOT-PHY, IOT-WRLS, IOT-UI*

- **Component Specialization (Optional):** Optional component specializations can be used to define test cases that are only relevant for certain parts or exemplars of a component (e.g., installed firmware - IOT-FW[INST] - as specialization for the component firmware - IOT-FW - or SPI - IOT-INT[SPI] - as specialization for the component internal interface - IOT-INT).

  By default, component specializations inherit all categories and test cases, defined for their parent node (e.g., all test cases defined for the component firmware - IOT-FW - are inherited by the specialization installed firmware - IOT-FW[INST]).

  If required, it is allowed to chain specializations, for example over-the-air firmware updates - IOT-FW\[UPDT][OTA] - as specialization of firmware update - IOT-FW[UPDT]. In this case, the second specialization inherits all categories and test cases, defined for the first specialization, thus also inheriting all test cases, defined for the component in general.

  Furthermore, if required, it is also allowed to define a list of categories or test cases, which should be excluded from being inherited by a component specialization.

  *Short representation: 2 - 5 uppercase alphabetic characters in square brackets*

  *Examples: IOT-FW[INST], IOT-FW[UPDT]*

- **Category:** The second main hierarchy level is the category, which can be used to group test cases, e.g., all test cases related to authorization can be grouped in the category AUTHZ.

  *Short representation: 2 - 5 uppercase alphabetic characters*

  *Examples: IOT-\*-AUTHZ, IOT-\*-INFO, IOT-\*-CONF*

- **Test Case:** The third main hierarchy level is the test case. See [3. Test Case Catalog](../03_test_cases/README.md) for more details.

  *Short representation: three-digit incremental number of the test case.*

  *Examples: IOT-FW-INFO-001, IOT-FW-INFO-002, IOT-FW-INFO-003*

This kind of structure allows to efficiently determine applicable subtrees by deselecting nodes (e.g., components, component specializations and categories) that are not relevant for a given device or test scenario. The table below shows an exemplary list of nodes for each hierarchy level. An overview of all components and categories that are included in this guide can be seen in the figure below the table.

The usage of component and category specializations allows to expand the catalog of general test cases to include test cases for specific standards and technologies. By inheriting test cases from their parent nodes, it is ensured that these test cases are also applied to the child nodes by default. However, at the time of writing this guide, the possibility that test cases of a parent node might not be applicable to a child node in particular cases could not be precluded. Thus, it is allowed to specify a list of test cases, which are excluded from being inherited by a certain child node.

Another way to expand the catalog is to add custom components, categories and test cases. This way, the methodology could also be expanded to include further components, e.g., device-external elements of the IoT ecosystem.

<table>
    <thead>
        <tr>
            <th>階層レベル</th>
            <th>ID</th>
            <th>説明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>IOT</td>
            <td>ルートノード</td>
        </tr>
        <tr>
            <td rowspan="14" valign="top">1</td>
            <td colspan="2"><b>コンポーネント (Component)</b></td>
        </tr>
        <tr>
            <td>IOT-PROC</td>
            <td>処理装置 (Processing Unit)</td>
        </tr>
        <tr>
            <td>IOT-MEM</td>
            <td>メモリ (Memory)</td>
        </tr>
        <tr>
            <td>IOT-FW</td>
            <td>ファームウェア (Firmware)</td>
        </tr>
        <tr>
            <td>IOT-DES</td>
            <td>データ交換サービス (Data Exchange Service)</td>
        </tr>
        <tr>
            <td>IOT-INT</td>
            <td>内部インタフェース (Internal Interface)</td>
        </tr>
        <tr>
            <td>IOT-PHY</td>
            <td>物理インタフェース (Physical Interface)</td>
        </tr>
        <tr>
            <td>IOT-WRLS</td>
            <td>無線インタフェース (Wireless Interface)</td>
        </tr>
        <tr>
            <td>IOT-UI</td>
            <td>ユーザーインタフェース (User Interface)</td>
        </tr>
        <tr>
            <td>IOT-*</td>
            <td>カスタムコンポーネント (Custom Component)<i>(将来の拡張用のプレースホルダ)</i></td>
        </tr>
        <tr>
            <td colspan="2"><b>コンポーネントの特殊化 (Component Specialization) (オプション)</b></td>
        </tr>
        <tr>
            <td>IOT-FW[INST]</td>
            <td>インストール済みファームウェア (Installed Firmware)</td>
        </tr>
        <tr>
            <td>IOT-FW[UPDT]</td>
            <td>ファームウェア更新メカニズム (Firmware Update Mechanism)</td>
        </tr>
        <tr>
            <td>IOT-*[*]</td>
            <td>カスタムコンポーネントの特殊化 (Custom Component Specialization)<i>(将来の拡張用のプレースホルダ)</i></td>
        </tr>
        <tr>
            <td rowspan="10" valign="top">2</td>
            <td colspan="2"><b>カテゴリ (Category)</b></td>
        </tr>
        <tr>
            <td>IOT-*-AUTHZ</td>
            <td>認可 (Authorization)</td>
        </tr>
        <tr>
            <td>IOT-*-INFO</td>
            <td>情報収集 (Information Gathering)</td>
        </tr>
        <tr>
            <td>IOT-*-CRYPT</td>
            <td>暗号技術 (Cryptography)</td>
        </tr>
        <tr>
            <td>IOT-*-SCRT</td>
            <td>シークレット (Secrets)</td>
        </tr>
        <tr>
            <td>IOT-*-CONF</td>
            <td>構成とパッチ管理 (Configuration and Patch Management)</td>
        </tr>
        <tr>
            <td>IOT-*-LOGIC</td>
            <td>ビジネスロジック (Business Logic)</td>
        </tr>
        <tr>
            <td>IOT-*-INPV</td>
            <td>入力バリデーション (Input Validation)</td>
        </tr>
        <tr>
            <td>IOT-*-SIDEC</td>
            <td>サイドチャネル攻撃 (Side-Channel Attacks)</td>
        </tr>
        <tr>
            <td>IOT-*-*</td>
            <td>カスタムカテゴリ (Custom Category) <i>(将来の拡張用のプレースホルダ)</i></td>
        </tr>
        <tr>
            <td rowspan="5" valign="top">3</td>
            <td colspan="2"><b>テストケース (Test Case)</b></td>
        </tr>
        <tr>
            <td>IOT-*-INFO-001</td>
            <td>ソースコードの開示 (Disclosure of Source Code)</td>
        </tr>
        <tr>
            <td>IOT-*-INFO-002</td>
            <td>実装内容の開示 (Disclosure of Implementation Details)</td>
        </tr>
        <tr>
            <td>IOT-*-INFO-003</td>
            <td>エコシステム内容の開示 (Disclosure of Ecosystem Details)</td>
        </tr>
        <tr>
            <td>IOT-*-*-*</td>
            <td>カスタムテストケース (Custom Test Case) <i>(将来の拡張用のプレースホルダ)</i></td>
        </tr>
    </tbody>
</table>


![image](../img/Component_Overview.png)

### テストケースの構造

Each individual test case, which is represented by a leaf node, is divided into the following sections:

-   **必要条件 (Requirements):** The requirements section will define which physical and authorization access levels are required to carry out the test case. Since these requirements also depend on the given test conditions, e.g., the specific implementation of the target device and its operational environment, a range of access levels might be defined which apply to the test case in general.

-   **要旨 (Summary):** The summary section includes an overall description of the security issue, which the test case is based on.

-   **テスト目的 (Test Objectives):** In the test objectives section, a list of checks that the tester has to perform is given. By performing these checks, the tester can determine whether the device is affected by the security issue described in the summary.

-   **対応策 (Remediation):** The remediation section comprises recommendations regarding potential measures that can be applied to solve the security issue. However, these recommendations are only rough suggestions. It is the responsibility of the manufacturer/operator to derive detailed measures in regards of the device implementation.




[bsi_pentest]: https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/Studies/Penetration/penetration_pdf.pdf?__blob=publicationFile&v=1	"Study: A Penetration Testing Model"
[nvd]: https://nvd.nist.gov	"National Vulnerability Database"
[owasp_fuzzing]: https://owasp.org/www-community/Fuzzing	"Fuzzing"
