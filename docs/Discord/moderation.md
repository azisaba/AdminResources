# モデレーション

ここでは Discord のモデレーション操作について記載します。

## BAN

!!! note BANとは

    Discordサーバーから半永久的に追放することです。

    解除されるまで、メンバーは再参加できなくなります。

### BANする

`/ban add` コマンドを使用します。

- `user` - BANしたいメンバーを指定します。名前での絞り込み、IDの直接指定ができます。(必須)
- `reason` - BANする理由を指定します。
- `duration` - BANの期間を指定します。ここで指定した場合は **TempBan** になります。
- `proof` - 証拠を指定します。
- `hack` - サーバーに参加していないメンバーをBANする **HackBan** を行うかどうか指定します。
- `soft` - BAN時のメッセージ削除機能を使う **SoftBan** を行うかどうか指定します。(SoftBanはBANしたあとすぐにBANを解除します。)
- `purge_days` - BANされるメンバーが送信したメッセージを削除する日数を指定します。`1d` などのFormatが使用できます。
- `dm` - 処罰時に被処罰者にDMするかどうか指定します。

!!! warning

    Discordの仕様上HackBanされたメンバーにDMを送信することはできません。

!!! example

    - `/ban add user:m2en#0092 reason:規約違反`

    サーバーに参加していないm2enを規約違反として通常BANします。

    - `/ban add user:m2en#0092 reason:規約違反 hack:true`

    サーバーに参加していないm2enを規約違反としてHackBanします。

### BANを解除する

`/ban remove` コマンドを使用します。

- `user` - メンバーを指定します。名前での絞り込み、IDの直接指定ができます。(必須)
- `reason` - BANする理由を指定します。
- `proof` - 証拠を指定します。

## タイムアウト

!!! note タイムアウトとは

    メッセージ送信、リアクション送信、VC参加、コマンドの使用など、ほぼ全権限を一時的に剥奪する処罰です。

### タイムアウトする

`/timeout add` コマンドを使用します。

- `user` -　メンバーを指定します。名前での絞り込み、IDの直接指定ができます。(必須)
- `reason` - 理由を指定します。
- `duration` - 期間を指定します。最大日数は28日です。(28d)
- `proof` - 証拠を指定します。
- `dm` - 処罰時に被処罰者にDMするかどうか指定します。

!!! example

    - `/timeout add user:m2en#0902 duration:2d6h`

    m2enを2日6時間タイムアウトします。

### タイムアウトを解除する

`/timeout remove` コマンドを使用します。

- `user` -　メンバーを指定します。名前での絞り込み、IDの直接指定ができます。(必須)
- `reason` - 理由を指定します。
- `proof` - 証拠を指定します。
- `dm` - 処罰時に被処罰者にDMするかどうか指定します。

## キックする

`/kick` コマンドを使用します。

- `user` -　メンバーを指定します。名前での絞り込み、IDの直接指定ができます。(必須)
- `reason` - 理由を指定します。
- `proof` - 証拠を指定します。
- `dm` - 処罰時に被処罰者にDMするかどうか指定します。

## 警告する

`/warn` コマンドを使用します。

警告が一定数貯まると自動処罰が発生する他、 `/info user` の情報に残ります。

- `user` -　メンバーを指定します。名前での絞り込み、IDの直接指定ができます。(必須)
- `reason` - 理由を指定します。
- `proof` - 証拠を指定します。

## 処罰情報を確認する

`/cases view` コマンドを使用します。

- `case` - 処罰IDを指定します。
- `type` - 処罰タイプを指定します。
- `user` - ユーザーを指定します。
- `mod` - モデレーション操作を行ったユーザーを指定します。

!!! note 処罰タイプ

    処罰タイプについてはこちら:

    - `Ban` - BAN
    - `Kick` - Kick
    - `Timeout` - タイムアウト
    - `Quarantine` - 隔離 (モデレーション権限を持ったメンバーから権限を奪う)
    - `Warn` - 警告
    - `Timeout Removal` - タイムアウト解除
    - `Quarantine` - 隔離解除
    - `Unban` - BAN解除

## ユーザー情報を確認する

`/info user` コマンドを使用します。

- `user` - ユーザーを指定します。(指定しなかった場合は実行者の情報を表示)

!!! note

    サーバーに参加していないユーザーはIDで検索できますが表示される情報は制限されます。

### 各情報について

#### General Informations

- `Name:` - ユーザー名、Tag、ID
- `Creation` - アカウント作成日時
- `Join:` - サーバー参加日時
- `Color:` - アカウントカラー
- `Discord Badge:` - アカウントのバッジ

#### Wick Informations

- `Suspicious` - 危険人物に指定されているかどうか
- `Warn Points` - 警告数
- `Active Strikes` - アクティブ状態のストライク数
- `Whitelisted` - 含まれていないWickの監視対象

#### Dangerous User

Discord上で危険な権限として扱われている権限を持っている場合に表示されます。

#### Wick Permissions

Wickの権限に関する情報

#### Account Accessories

- `Roles:` - アカウントに付与されたロール一覧
- `Webhooks:` - Webhookかどうか
