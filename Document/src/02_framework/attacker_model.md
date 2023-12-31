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

この攻撃モデルにおいて、アクセスレベルは特定の個人グループ (アクセスグループ) と IoT デバイスとの関係を示す尺度です。アクセスレベルはアクセスグループの個人がどのようにデバイスとやり取りできるように意図されているかを説明します。これらは物理的なやり取りか、論理的な認可のやり取りのいずれかになります。

個人がデバイスにどれくらい近づくごとができるかは、物理アクセスレベルによって測定されます。物理アクセスレベルは CVSS メトリックの「攻撃元区分 (Attack Vector)」を適用したものであり、ターゲットデバイスに対して攻撃を実行するために必要な物理コンテキストを反映します。したがって、CVSS の元の値の一部 (ネットワーク、ローカル、物理) を使用しました。しかし、物理コンテキストに焦点をあてて、ローカルアクセスの説明を調整しました。さらに、CVSS で定義されている物理アクセスは、非侵襲的物理アクセスと侵襲的物理アクセスの二つのレベルに分割しました。その理由は、IoT デバイスの一部では、ロックまたは密閉された筐体など、デバイス内部要素へのアクセスを制限する特別な手段で保護されているためです。この場合、攻撃者は妥当な時間内にデバイス内部にアクセスできないかもしれないため、非侵襲的な物理アクセスしかできません。他のデバイスにはネジを外すなどにより短時間で開けることができる筐体があります。そのため、攻撃者はデバイス内部にアクセスして、侵襲的な物理アクセスを獲得できます。全体として、物理アクセスレベルは地理的位置、建物のセキュリティ、デバイスの筐体などの要因によって影響を受ける可能性があります。

以下の物理アクセスレベルを定義しています。

1.  **リモートアクセス (*PA-1*):** 個人とデバイスの間には任意の物理的な距離があります。リモートアクセスを行う攻撃者は世界中のどこにでも可能性があり、これは通常、デバイスがグローバルエリアネットワーク (GAN) 経由で直接アクセスできることを意味します。

2.  **ローカルアクセス (*PA-2*):** 個人とデバイスの間には限定的な物理的な距離 [^2] がありますが、直接的な物理的やり取りはできません。ローカルアクセスを行う攻撃者は近接からデバイスを使用する可能性があり、これは通常、デバイスがローカルエリアネットワーク (LAN) やワイヤレスローカルエリアネットワーク (WLAN) 経由で直接アクセスできることを意味します。

3.  **非侵襲的アクセス (*PA-3*):** 個人とデバイスの間には物理的な距離はありませんが、個人は物理的な方法でデバイス内部要素に直接アクセスできません (つまり、デバイスの筐体を簡単に開けることができません)。

4.  **侵襲的アクセス (*PA-4*):** 個人とデバイスの間には物理的な距離はなく、個人は物理的な方法でデバイス内部要素に直接アクセスできます (つまり、デバイスの筐体を開けます)。

個人のデジタル権限は認可アクセスレベルによって測定されます。認可アクセスレベルは CVSS メトリック「必要な特権レベル (Privileges Required)」を適応したものです。CVSS で定義された値に加えて、製造業者レベルアクセスと呼ばれる別のレベルの権限を高権限のトップに追加しました。ウェブアプリケーションやコンピュータネットワークは通常、事業者のコントロールゾーン内 (データセンタ内など) で運用されるのとは対照的に、IoT デバイスはそのコントロールゾーン外で運用されることがよくあります。メンテナンスの保護とデバッグアクセスのために確立された方法 (メンテナンスアクセスをデータセンタ内のあらかじめ定義されたサブネット、IP アドレス、物理ポートに制限するなど) を常に適用できるとは限りません。そのため、製造業者レベルアクセスでのデバイスに対する攻撃が可能になるかもしれません。全体として、認可アクセスレベルはポリシーやロールベースのアクセスモデルなどの要因によって影響を受ける可能性があります。

以下の認可アクセスレベルを定義しています。

1.  **非認可アクセス (*AA-1*):** 個人はデバイスコンポーネントに匿名でアクセスできます。匿名アクセスでの攻撃者は任意の未登録ユーザーである可能性があります。

2.  **低権限アクセス (*AA-2*):** 個人がデバイスコンポーネントにアクセスできるのは、認証されて標準的な認可権限を持つ場合に限ります。低権限アクセスでの攻撃者は任意の登録ユーザーである可能性があります。

3.  **高権限アクセス (*AA-3*):** 個人がデバイスコンポーネントにアクセスできるのは、認証されて広範な権限を持つ場合に限ります。「広範な権限」という用語は、すべての登録ユーザーが利用できるわけではない制限されたデバイスコンポーネントの機能 (構成設定など) に個人がアクセスできることを意味します。

4.  **製造業者レベルアクセス (*AA-4*):** 個人がデバイスコンポーネントにアクセスできるのは、認証されて製造業者レベルの認可権限を持つ場合に限ります。高権限アクセスとは対照的に、製造業者レベルアクセスはいかなる形でも制限されず、たとえば、デバイス開発者のためのデバッグアクセス、ソースコードへのアクセス、ファームウェアへのルートレベルのアクセスが含まれます。

[^2]: 限定的な物理的な距離は特定の最大値に制限されません。使用するテクノロジによって、最大距離は数メートル (Bluetooth の場合など) から数キロメートル (LTE の場合など) の範囲になるかもしれません。



## デバイスコンポーネントとアクセスレベルのマッピング

テスト時のテスト担当者の観点はテストのベースラインとして選択される最小および最大のアクセスレベルによって決まります。物理アクセスと認可アクセスレベルはペネトレーションテストとその範囲にさまざまな影響を与えます。

**物理アクセスレベル:**

-   物理アクセスレベルはデバイス全体を指します。そのため、一部の物理アクセスレベルでは、攻撃者はこれらのコンポーネントとまったくやり取りできないため、特定のデバイスコンポーネントは指定されたレベルでテストできないことを直接定義します。物理アクセスレベルとデバイスコンポーネントの関係を下表に示します。

-   製造業者や事業者の特定の要件に基づいて、最小や最大の物理アクセスレベルがテスト実行のための厳しい境界となるかもしれません。これは契約者が侵襲的な物理アクセスを必要とするものなどの特定のテストを特に除外したい場合があるためです。

**認可アクセスレベル:**

-   認可アクセスは複数のデバイスコンポーネント間で異なる方法で処理されるかもしれないため、認可アクセスレベルはデバイス全体ではなく個々のコンポーネントへのアクセスを指します。そのため、テスト範囲に対する認可アクセスレベルの影響はビジネスロジックの特定の実装とコンポーネントごとの認可/パーミッションスキームに常に依存します。

-   意図したものよりも低い権限でデバイス (の一部) にアクセスできるかどうかを評価することはテストの一部であるべきなので、テストの観点から最小の認可アクセスレベルを選択する理由はありません。

全体として、攻撃者モデルは潜在的な攻撃者の抽象表現を作成するために使用できます。これはどのような攻撃者が動作環境において特定のデバイスに対する脅威とみなされるかを説明するために使用できます。他の方法論やモデルとは対照的に、これはより合理的な方法で使用できるため、たとえば完全な脅威およびリスクの分析アプローチと比較して、より効率的です。また、ベースとなる CVSS よりも IoT コンテキストの特殊性を考慮しています。デバイスモデルと組み合わせることで、テスト範囲とテスト観点を定義することができ、それによってどのテストケースが実行可能で実行しなければならないかを決定できます。

| コンポーネント                 |  PA-4  |   PA-3    |   PA-2    |   PA-1    |
| ------------------------------ | :----: | :-------: | :-------: | :-------: |
| 処理装置                       | **✓** |           |           |           |
| メモリ                         | **✓** |           |           |           |
| インストール済みファームウェア | **✓** | **?**[^3] | **?**[^3] | **?**[^3] |
| ファームウェア更新メカニズム   | **✓** | **?**[^3] | **?**[^3] | **?**[^3] |
| データ交換サービス             | **✓** | **?**[^4] | **?**[^4] | **?**[^4] |
| 内部インタフェース             | **✓** |           |           |           |
| 物理インタフェース             | **✓** |   **✓**  | **?**[^5] |           |
| 無線インタフェース             | **✓** |   **✓**  |  **✓**   |           |
| ユーザーインタフェース         | **✓** |   **✓**  | **?**[^6] | **?**[^6] |

[^3]: インストール済みファームウェアとファームウェア更新メカニズムは、ファームウェアへの直接アクセスを行う方法 (SSH 経由など) に応じて、非侵襲的 (*PA-3*)、ローカル (*PA-2*)、リモート (*PA-1*) の物理アクセスでテスト可能かもしれません。

[^4]: データ交換サービスは、遠隔操作や監視目的など、その種のアクセスのために設計されているかどうかに応じて、非侵襲的 (*PA-3*)、ローカル (*PA-2*)、リモート (*PA-1*) の物理アクセスでテスト可能かもしれません。

[^5]: 物理インタフェースがローカルネットワークで接続されている場合など、特定の状況下では物理インタフェースはローカル (*PA-2*) の物理アクセスでテスト可能かもしれません。

[^6]: ユーザーインタフェースは、遠隔操作や監視目的など、その種のアクセスのために設計されているかどうかに応じて、ローカル (*PA-2*)、リモート (*PA-1*) の物理アクセスでテスト可能かもしれません。



[cvss]: https://www.first.org/cvss/	"Common Vulnerability Scoring System"
