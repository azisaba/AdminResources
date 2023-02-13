# Rarity

Rarityは任意のレア度の作成や、任意のアイテムにそのレア度を付与することができるプラグインです。

[AzisabaNetwork/Rarity (GitHub)](https://github.com/AzisabaNetwork/Rarity)

!!! note
    データを追加・変更・削除したあとは`/rarity refresh`で変更内容を反映させる必要があります。

## 用語集

### Condition

アイテムを判別するために使う式のこと。

右項はすべて`""`で囲む必要があります。(例: `"test"`)

また、右項には正規表現を使用することも可能です。(例: `"pattern:(IRON|DIAMOND)_HELMET"`)

Conditionの式は`&&`、`||`で連結することが可能で、それぞれANDとORを表しています。
`式 && 式 || 式`は使用できないので、`(式 && 式) || (式)`もしくは`(式) && (式 || 式)`のように明示的に指定してください。(これら二つはそれぞれ意味が異なります)

式は`左項 = 右項`の形式で構成されています。

左項は以下に記述するいずれかが使用可能です。

#### `hash`

!!! note
    `hash`はアイテムのタグが**ひとつでも**変わったら適用されなくなるためおすすめしません。できるだけ`type`、`display_name`、`tag`の組み合わせで済むようにしましょう。

アイテムの種類+アイテムのタグをハッシュした文字列と比較します。

アイテムをメインハンドに持った状態で`/rarity hash`と打つことで文字列が出力されるので、それをクリックすることでチャット欄に補完されます。(Ctrl+A Ctrl+Cをすることでコピーも可能です)

例: `hash = "COOKED_BEEF;68639115739b7a795979353bc0172c274a9fc43a5628dfe4c4c03c8ff4a54e1e65b851163fe6e0e60540a565fa22b87383255462402631af04f3db89b69fba5a"`

#### `type`

!!! warning
    Conditionを`type`のみにすることはConditionの重複が発生する可能性があるのでしないでください。
    バニラアイテムのみを指定したい場合は`type = "STONE" && tag_size == "0"`のようにしてください。

アイテムの種類で比較します。右項はすべて大文字にする必要があります。

アイテムの種類の一覧は[SpigotのJavadocs](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html)にすべて列挙されています。(バージョンによって使用できる文字列が異なります)

例:

- `type = "DIAMOND_SWORD"`
- `type = "pattern:(IRON|DIAMOND)_CHESTPLATE"`

#### `display_name`

アイテムの名前で比較します。

アイテムの名前はアイテムをメインハンドに持って`/rarity dumpItem`で表示できます。

例:

- `display_name = "&r&d炊飯器"`
- `display_name = "pattern:&e[Cc]oin"`

#### `amount`

アイテムの数で比較します。

例: `amount = "64"`

#### `tag`

任意のタグを**文字列として**比較します。

例:

- `tag.CustomModelData = "1"`
- `tag.display.Name = "pattern:\\{\"text\":\"\\u00a7d炊飯器\"}" && tag.CustomModelData = "99" && tag_size = "2" && type = "STONE"`
- `tag.MYTHIC_TYPE = "onikudekai_ga"`

#### `tag_size`

NBTタグのサイズで比較します。

たとえば、`{display:{Name:'{"text":"something"}'},CustomModelData:1}`というアイテムがあった場合、`tag_size`は`2`になります。(`tag_size = "2"`)

## レア度の作成

レア度は`/rarity createRarity <id> <weight> <display name>`で作成ができます。

`<id>`はレア度を識別する名前を入れます。(小文字英数字とアンダーバーのみ使用可能) (例: `legendary`)

`<weight>`はこれを書いている地点(2023/02/13)では未使用なので適当な値(0など)を入れてください。(本来はレア度が低くなるにつれてweightも小さくなる)

`<display name>`はデフォルトの表示名を入れます。(例: `&6&lLEGENDARY`)

## 言語別のレア度の表示名を追加

`/rarity addTranslation <id> <locale> <display name>`で表示名の追加ができます。

`<id>`は作成したレア度のIDを入れます。

`<locale>`はたとえば`ja`や`en`などの言語を入れます。(<https://ja.wikipedia.org/wiki/ISO_639-1%E3%82%B3%E3%83%BC%E3%83%89%E4%B8%80%E8%A6%A7>の`639-1`も参照)

`<display name>`は指定した言語で表示したい表示名を入れます。

## レア度の削除

`/rarity deleteRarity <id>`でレア度の削除ができます。削除をする前にレア度と関連付けられているConditionを空にする必要があります。

`/rarity clearConditionsOf <id>`を実行することで**すべて**のConditionを削除することができます。

`/rarity listConditions <id>`でレア度と関連付けられているConditionをすべて表示できます。さらに`/rarity listConditions <id>`で出てきたテキストをクリックで削除コマンドの補完ができます。

## Conditionのテスト

アイテムをメインハンドに持った状態で`/rarity test <condition>`を実行することでメインハンドに持っているアイテムが指定したConditionに当てはまるかどうかをテストできます。

`true`と出た場合は当てはまっていて、`false`と出た場合は当てはまっていません。

## Conditionの追加

!!! warning
    実際にConditionを追加する前に、[Conditionのテスト](#condition_1)をすることを強くおすすめします。

`/rarity createCondition <id> <condition>`でConditionの追加ができます。

`<id>`にはレア度のID、`<condition>`にはアイテムを判別するための[Condition](#condition)を書く必要があります。

## Conditionの削除

`/rarity deleteCondition <id> <condition>`でConditionの削除ができます。

`/rarity info`や`/rarity listConditions <id>`で出るテキストをクリックすることで削除コマンドを補完させることもできます。

## アイテムと関連付けられたConditionの表示

アイテムをメインハンドに持って`/rarity info`を実行することで、Conditionを表示させることができます。

![](https://messageviewer.azisaba.net/attachments/ub0b00ed8c7ed4edd9fabf3fdd9b44fdc9e/javaw_a2cPcbMzdm.png)

Conditionの数は1以下であるべきで、2以上の場合はConditionが複数当てはまっているのでConditionを変更する必要があります。

なお、Conditionをクリックすることで削除コマンドの補完ができます。
