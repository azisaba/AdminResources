# Packet Events

## 用途
「ProtocolLib」に代わる、マインクラフトのパケットを扱うためのモダンなライブラリです。  
本来、プラグインでパケットを送受信するためにはマインクラフトのコードを直接実行する必要がありますが、ライブラリを使用することで、これを簡単に行うことができます。

## 特徴
「ProtocolLib」と異なり、Nettyスレッド上で非同期で実行されます。そのため、マインクラフトサーバーの処理に影響を与えることはほとんどありません。  
さらに、モダンな設計により、パケットの送受信がより簡単に行うことができます。  
また、複数のプラットフォームに対応しており、SpigotやPaperはもちろん、VelocityやFabric上でも実行することができます。

## 実装例
後述のExampleより一部改変して紹介します。
### パケットリスナー
```java
@Override
public void onPacketReceive(PacketReceiveEvent event) {
    User user = event.getUser();
    if (event.getPacketType() == PacketType.Play.Client.INTERACT_ENTITY) { //パケットの種類が"INTERACT_ENTITY"かどうか？

        WrapperPlayClientInteractEntity interactEntity = new WrapperPlayClientInteractEntity(event); // ラッパーに変換
        WrapperPlayClientInteractEntity.InteractAction action = interactEntity.getAction(); // パケットの中身（ここではアクション）を取得

        if (action == WrapperPlayClientInteractEntity.InteractAction.ATTACK) { // 攻撃アクションかどうか？
            user.sendMessage(Component.text("You attacked an entity."));
        }
    }
}
```
### パケット送信
```java
public void sendSpawnPacket(User user, Location location) {
    // 新たにパケットを作成する
    WrapperPlayServerSpawnEntity packet = new WrapperPlayServerSpawnEntity(
        entityId,
        uuid,
        EntityTypes.ARMOR_STAND, // エンティティの種類を指定
        location,
        location.getYaw(),
        0,
        null
    );
    user.sendPacket(packet); // パケットを送信する
}
```
## 使い方
使い方には２種類あります。  
1. 複数のプラグインで共通のエンコーダー・デコーダーを使用する  
2. プラグインにライブラリを組み込み(shade)、独自のエンコーダー・デコーダーを使用する 【おすすめ】  

エンコーダー・デコーダーが共通だと、リスナーを通過する順番により他プラグインと競合を起こす可能性があります。そのため、２のようにプラグインに組み込む(shade)ことをおすすめします。  

## 公式ドキュメント
https://docs.packetevents.com/getting-started

## Example Repository
https://github.com/retrooper/packetevents-example