# AzisabaAchievements

AzisabaAchievements(略称: AA)はアジ鯖の一部サーバーで使用されている、カスタム実績の作成が可能なプラグインです。(バニラの実績、進捗システムは使いません)

[AzisabaNetwork/AzisabaAchievements (GitHub)](https://github.com/AzisabaNetwork/AzisabaAchievements)

## 用語集

### キー (Key)

`名前空間:パス`の形式から成る文字列です。

「名前空間」には小文字英数字と`._-`の文字が使用できます。

「パス」には「名前空間」で使える文字と区切りを表すスラッシュが使用できます。

!!! note
    アジ鯖では「名前空間」には`azisaba`以外の文字列は使用されていません。

### 進行度 (Count)

![](https://messageviewer.azisaba.net/attachments/u51bb8850a13b6c6a58c5c547a0b1fe93ae/javaw_sZFuiAKBUH.png)

[バニラの進捗](https://messageviewer.azisaba.net/attachments/uf2b4e9924c587a360cea4d14fdd84ee1e9/javaw_vzmpWfJgxi.png)に似たようなシステムで、進行度が最大値に達したら解除判定になります。

### 実績ポイント (Point)

![](https://messageviewer.azisaba.net/attachments/u51bb8850a13b6c6a58c5c547a0b1fe93ae/javaw_sZFuiAKBUH.png)

![](https://messageviewer.azisaba.net/attachments/u5d0551c733878b0245306658d482170012/javaw_oW3BcqIuVN.png)

実績プラグイン本体では使い道がありませんが、実績ポイントという概念があります。解除した実績のポイントは合計されて実績メニューの自分の頭やカテゴリにカーソルを当てた際に表示されます。

### 言語キー

一部のコマンドには言語キーという引数が必要になる場合があります。

| 言語 | 言語キー |
| --- | -------- |
| 英語 | en |
| 日本語 | ja |

!!! todo
    英語と日本語以外の言語も追加

### カテゴリ

![](https://messageviewer.azisaba.net/attachments/u781606a90067f7c3db8a272c5267b8e847/javaw_dkAdhuFLOi.png)

実績の種類の一つとしてカテゴリがあり、カテゴリの中に複数の実績をまとめることができます。

カテゴリはコマンド(`/vachievements achievement <キー> flags add category`)で手動で設定する必要があり、カテゴリ自体は実績の達成度には含まれません。

## 実績の作成

### 実績のID(キー)を考える

まず実績を作ったり編集したりするにはそれを識別するための名前(以下[キー](#key))が必要になります。

キーの先頭につける文字はサーバーによって異なります。

| サーバー | 接頭辞 |
| --- | --- |
| 全般 | `azisaba:general/` |
| Life | `azisaba:life/` |
| Leon Gun War | `azisaba:lgw/` |
| DIVERSE | `azisaba:diverse/` |
| Sclat | `azisaba:sclat/` |
| ばにらいふ！ | `azisaba:vanilife/` |
| Despawn | `azisaba:despawn/` |

名前はなんでもいいですが、カテゴリ分けをするのが望ましいです。

たとえばLifeサーバーのPvEのコンテンツの一つであるFFをターゲットにする場合、カテゴリのキーは`azisaba:life/pve/ff`となり、その下にある実績のキーは例えば`azisaba:life/pve/ff/foo`となります。

### 実績を作る

1. `/achievements create <キー> <進行度の最大> <実績ポイント>`を実行して実績データを作成
2. 必要な場合は`/vachievements achievement <キー> flags add seasonal`でシーズン限定実績にする(コマンド先端の`v`に注意)

### カテゴリを作る

1. `/achievements create <キー> 0 0`を実行して実績データを作成(この地点ではまだ「実績」として扱われています)
2. `/vachievements achievement <キー> flags add category`を実行してカテゴリとして設定する(コマンドの先端の`v`に注意)

### 名前と説明を追加

`/achievements addTranslation <キー> <言語キー> "<名前>" "<説明>"`で実績もしくはカテゴリに言語別の名前・説明を追加できます。

名前と説明はダブルクォートで囲むことでスペースを含んだ文字列を使うことができます。

## 進行度を上昇させる

デフォルトでは実績には進行度を上昇させるトリガーがないので、コマンドもしくはAPIで自分自身で実装する必要があります。

コマンドでは`/achievements progress <プレイヤー> <キー> <進行度>`で進行度を指定した分上昇させることができます。

## 実績を削除する

間違って作成したなどの理由で実績を削除したいときは`/vachievements achievement <キー> delete`で実績データの削除が可能です。

## 開発者API

### Artifactの場所

リポジトリURL: `https://repo.azisaba.net/repository/maven-public/`

Group ID: `net.azisaba.azisabaachievements`

Artifact ID: `api`, `common`, `spigot`, `velocity`, `cli`のいずれか(api以外のすべてのものはapiを含み、apiとcommon以外のすべてのものはcommonを含みます)

### API: インスタンスの取得

```java
AzisabaAchievements api = AzisabaAchievementsProvider.get();
```

### API: 実績の作成

```java
// Obtain the instance of AzisabaAchievements
AzisabaAchievements api = AzisabaAchievementsProvider.get();
// Obtain the instance of the achievement manager
AchievementManager achievementManager = api.getAchievementManager();
// Create achievement named azisaba:general/happy_new_year, with the count of 1 and points of 5, and block the thread until the opration is completed
achievementManager.createAchievement(Key.key("azisaba", "general/happy_new_year"), 1, 5).join();
```

### API: 実績の進行度を上昇させる

```java
// Obtain player uuid
UUID playerUuid = ExamplePlatform.getPlayer("Dinnerbone").getUniqueId();
// Obtain the instance of AzisabaAchievements
AzisabaAchievements api = AzisabaAchievementsProvider.get();
// Obtain the instance of the achievement manager
AchievementManager achievementManager = api.getAchievementManager();
// Increment the count of azisaba:general/happy_new_year by 1 for Dinnerbone
achievementManager.progressAchievement(playerUuid, Key.key("azisaba", "general/happy_new_year"), 1).join();
```
