# Script Block Plus

## 用途
ブロックにコマンドを埋め込むのに使うプラグインです。
プレイヤーをテレポートさせたりプレイヤーにアイテムを渡すためなどに使用します。
コマンドブロックの代わりだと思ってください。

## コマンド
### 基礎

!!! warning
    `@bypass`を使うと一部のプラグイン（WorldGuardなど）で保護貫通ができるようになるなどの問題が発生する可能性があります。

- ``/sbp モード create [@bypass ユーザーに実行させるコマンド]``
コマンド実行直後に右クリックしたブロックにコマンドが埋め込まれます。
これが基本的なコマンドの書式です。
@bypassは権限を無視してコマンド発動となるので扱う際は要注意。@playerや@commandなどのホルダーもあります

- ``/sbp モード remove``
コマンド実行直後に右クリックしたブロックのコマンドが削除されます。埋め込まれたコマンドと同じモードにしないとコマンドが消えないので注意。


### 実用例
- テレポートさせたい！
``/sbp interact create [@console /spawn <player>]``
このコマンドを埋め込んだブロックをクリックしたユーザーは全員/spawnコマンドが実行され、スポーンにテレポートされます。
- アイテムを渡したい！
``/sbp walk create [@console /minecraft:give <player> minecraft:diamond 1]``
このコマンドを埋め込んだブロックの上を歩いたユーザーは全員/giveコマンドが実行され、ダイヤモンドを受け取ります。このままだと無限にダイヤモンド受け取れるので実際に使用するときは別の処理と組み合わせます。``<player>`` の部分はブロックの上を歩いたプレイヤーのMCIDで置き換えられます。
- アスレのクリア報酬をSBPで渡したい！  組み合わせの方法
``/sbp interact create [@console /spawn <player>][@console /shot give <player> jetpack]``
このコマンドを埋め込んだブロックをクリックしたユーザーは全員/spawnコマンドが実行され、スポーンにテレポート。また、スポーンにテレポートされた直後に2個目のコマンドが実行され、Jetpackを受け取ります。これによりアスレを再度クリアしないと次の報酬がもらえなくなります。ここではコマンドを二つしか入れていませんが、同じ形式でつなげればもっと多くのコマンドをひとつのブロックで実行可能です。

- パーティクルを発生させたい！
``/sbp walk create @command /particle パーティクル名 X Y Z 1 1 1 1 120 force @s``
120は量です。自由に変えれます
1 1 1 1 の左から3番目は範囲です（X Y Z）最後の1は速度です

- モブを召喚させたい！
``/sbp walk create @bypass /summon 召喚名 座標 ``

- プレイヤーの画面にタイトルを表示させたい！
``/sbp walk create @title:タイトル名``
サブタイトル付きは
``/sbp walk create @title:タイトル名/タイトル名``

## その他
- モードはブロックの上を歩くとコマンドが発動する``walk``、ブロックを右クリックするとコマンドが発動する``interact``を使うことが多いです。
- さらに詳しく知りたい人は <https://github.com/yuttyann/ScriptBlockPlus/wiki/%5BJP%5D-Plugin-Tutorial> を参考にすると良いです。
- 自鯖環境で試したい人は <https://www.spigotmc.org/resources/scriptblockplus-1-9-1-18-all-servers-will-be-enhanced.78413/> でダウンロードしてください。
