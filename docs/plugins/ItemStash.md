# ItemStash

[https://github.com/AzisabaNetwork/ItemStash](https://github.com/AzisabaNetwork/ItemStash)

## 用途
アイテムが溢れた場合の期限付きバッファとして使用します。
自分でstash機能を実装せず、ItemStashで一元管理することで、複数のプラグインでの溢れた場合の挙動を合わせることができ、プレイヤーが扱いやすくなります。

## 実装例

### 石が10個溢れた場合
- `<playerUuid>`には、`java.util.UUID`型の対象プレイヤーのuuidを入れてください。
```java
ItemStash.getInstance().addItemToStash(<playerUuid>, new ItemStack(Material.STONE, 10));
```

### ダイヤが2個溢れた場合 + 期限を`<expireAt>`に指定したい場合
- `<expireAt>`について
  - この値は、特定の時刻のミリ秒にしてください。
  - `-1`を指定すると、無期限(=アイテムがバッファから消えなくなる)になります。
```java
ItemStash.getInstance().addItemToStash(<playerUuid>, new ItemStack(Material.DIAMOND, 2), <expireAt>);
```
