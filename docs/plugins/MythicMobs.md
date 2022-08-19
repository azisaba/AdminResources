# Mythic Mobs

## 用途
モブを作成するのに使用します。
アジ鯖内でMMを使用するときは(恐らく)関係ないですが、アイテムを作成するときは別Plugin(MythicCrucible)が必要です。

## 使い方

### ・Mob
```yml
[MOBの名前]:
  Type: [mobtype] #(Mobの種類 (例: zombie, skeleton) )
  Display: '[display_name]' #(Mobの名前 (例: 最高のゾンビ！) )
  Health: [number] #(MobのHP)
  Armor: [number] #(MobのArmor値 (Mobが受けるダメージを指定された分だけ減らす) )
  Damage: [number] #(Mobの攻撃力)
  BossBar: #(Mobのボスバーを表示する)
    Enabled: [true/false]
    Title: '[name]'
    Range: [number] #(範囲)
    Color: [color] #(ボスバーの色 (例: #ffff00) )
    Style: [Style] #(ボスバーのスタイル (例: SOLID) )
    CreateFog: [true/false]
    DarkenSky: [true/false]
    PlayMusic: [true/false]
  Faction: [name] #(Mobの派閥 (例: SuperZombie) )
  Mount: [mobtype] #(Mobの名前 (例: skeleton / superZombie) )
  Options: #(Mobの細かい設定(ここにあげているやつ以外にもいっぱいある) )
    AlwaysShowName: [true/false] #(常に名前を表示)
    ApplyInvisibility: [true/false] #(常に透明化)
    Collidable: [true/false] #(Mobに衝突判定)
    Despawn: [true/false/chunk/persistent] #(Mobのデスポーン)
    MaxCombatDistance: [number] #(Mobが攻撃を受ける範囲)
    Glowing: [true/false] #(常に発行を付ける)
    Invincible: [true/false] #(Mobが無敵)
    KnockbackResistance: [number] #(ノックバック耐性)
    MovementSpeed: [number] #(移動速度)
    NoAI: [true/false] #(MobにAIをつけるか(trueにするとMobが動かなくなる) )
    NoDamageTicks: [number] #(Mobが攻撃を受けてからダメージを食らわないtick)
    NoGravity: [true/false] #(Mobの重力(trueにするとMobが浮く) )
  Equipment: #(Mobの装備品を指定)
  - <item> <slot> #(<item> MM製アイテム or バニラアイテムの名前を指定 / <slot> HEAD,CHEST,LEGS,FEET,HAND,OFFHAND)
  KillMessages: #(Mobがプレイヤーを倒した時に出すメッセージ)
  Disguise: #(Mobの見た目を変更する(省略) )
  Skills: #(Skillを指定)
  - skill{s=[Skill名]} @[*ターゲッター] ~[*トリガー1] 
```
|  ターゲッター  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  @self  |  @Caster  |  自分をターゲットにする  |
|  @target  |  @T  |  モブの標的をターゲットにする  |
|  @trigger  |    |  スキル発動者をターゲットにする  |
|  @nearestPlayer{r=#}  |    |  半径内で最も近いプレイヤーをターゲットにする。デフォルトでは{r=5}  |
|  @selfLocation{y=0.0}  |    |  自分の位置から `y=0.0` で指定した高さの場所をターゲットにする  |
|  @forward{f=5;y=0.0;sideOffset=0.0}  |    |  自分の向いている方向の前方 `f=5` ブロックの位置を `y=0.0` と `sideOffset=0.0` でターゲットにする  |
|  @targetLocation  |  @targetloc, @TL  |  標的がいる位置をターゲットにする  |
|  @origin{xoffset=0;yoffset=0;zoffset=0}  |    |  Metaskillの原点をターゲットにする  |

※他にもあるので、詳しくは<https://git.mythiccraft.io/mythiccraft/MythicMobs/-/wikis/Skills/Targeters> を参考にしてください

|  トリガー1  |  説明  |
| ---- | ---- |
|  ~onAttack  |  攻撃時に発火  |
|  ~onDamaged  |  ダメージを受けた時に発火  |
|  ~onDeath  |  死亡時に発火  |
|  ~onSpawn  |  MobがSpawnした時に発火  |
|  ~onDespawn  |  MobがDespawnした時に発火  |
|  ~onKill  |  Mobを倒した時に発火  |
|  ~onKillPlayer  |  Playerを倒した時に発火  |
|  ~onInteract  |  右クリック時に発火  |
|  ~onPlayerDeath  |  Playerの死亡時に発火  |
|  ~onSignal  |  Signalを受け取った時に発火  |
|  ~onShoot  |  弓や雪玉を発射した時に発火  |

※他にもあるので、詳しくは<https://git.mythiccraft.io/mythiccraft/MythicMobs/-/wikis/Skills/Triggers> を参考にしてください


### Item

この機能を利用するにはMythicCruicibleまたはMythicArtifactsが必要です。
```yml
[Itemの名前]:
  Id: [itemId] #(ItemのId(例: wooden_sword, dirt, grass) )
  Display: [display_name] #(アイテムの名前 (例: 最高の剣！！) )
  Lore:
  - 'lore1' #(アイテムの説明をここに書ける)
  - 'lore2' #(こんな感じに [- '説明'] を増やせば基本いくらでもLoreを追加できる)
  - 'これは乱数だ！ → <random.1to100>' #(PlaceHolderも入れられる)
  Skills: #(アイテムのスキル)
  - skill{s=[Skill名]} @[*ターゲッター] ~[*トリガー2]
  Attributes: #(アイテムの属性(例: 攻撃速度上昇, 移動速度上昇, 最大体力上昇) )
    Slot: #(All, MainHand, OffHand, Head, Chest, Legs, Feet)
      Attribute: [number] #(↓Attributesの種類↓)

# Attributes # 

AttackSpeed: [number] #(攻撃速度上昇)
Armor: [number] #(防具値上昇)
ArmorToughness: [number] #(防具強度上昇)
Damage: [number] #(攻撃力上昇)
Health: [number] #(最大体力上昇)
Luck: [number] #(幸運上昇)
KnockbackResistance: [number] #(ノックバック耐性)
MovementSpeed: [number] #(移動速度上昇)
```
|  トリガー2  |  説明  |
| ---- | ---- |
|  ~onUse  |  右クリック時に発火  |
|  ~onSwing  |  左クリック時に発火  |
|  ~onPressQ  |  Qキーを押したときに発火  |
|  ~onPressF  |  Fキーを押したときに発火  |
|  ~onConsume  |  食料を食べた時に発火  |
|  ~onShoot  |  弓や雪玉を発射した時に発火  |

※これまた他にもあるので、詳しくは<https://git.mythiccraft.io/mythiccraft/mythiccrucible/-/wikis/Skills/Triggers> を参考にしてください


### Skills

```yml
# 記述方法 #
[Skill名]:
 Conditions: #(スキルが発動する条件を指定)
 - [条件] [true/false]
 Skills: #(スキルをここに書く)
 - メカニック{引数=value} @[ターゲッター]
```

* **particles**

particleを出す

|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  particle  |  p  |  再生するエフェクトの種類  |
|  amount  |  a  |  エフェクトの個数  |
|  hSpread  |  hs  |  エフェクトのばらつき(x,z方向)  |
|  vSpread  |  vs  |  エフェクトのばらつき(y方向)  |
|  speed  |  s  |  エフェクトの速さ  |
|  yOffset  |   y   |  エフェクトを描画する高さ  |
|  color  |  c  |  一部のエフェクトでのみ使用できる 16進数カラーコード(`#○○○○○○`)で記述  |
|  fromorigin  |  fo  |  origin(他の一部のスキルの発生場所など)からエフェクトを発生させるかどうか  |
```yml
# 記述方法 #

Skills:
- particles{particle=reddust;amount=10} @target
```

* **projectile**

目の前に真っ直ぐ飛ぶレーザーを出す
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  onTick  |  ot  |  毎Tick発動するスキル  |
|  onHit  |  oh  |  projectileがHitした時に発動するスキル  |
|  onStart  |  os  |  projectileが発射した時に発動するスキル  |
|  onEnd  |  oe  |  projectileが無くなった時に発動するスキル  |
|  Velocity  |  v  |  速度  |
|  Interval  |   i   |  位置を更新するtick  |
|  Duration  |  d  |  最大持続時間   |
|  HitPlayers  |  hp  |  Playerに当たるか  |
|  HitNonPlayers  |  hnp  |  Player以外のEntityに当たるか  |
|  StopAtEntity  |  sE  |  Entityに当たった時に止まるか  |
|  StopAtBlock  |  sB  |  Blockに当たった時に止まるか  |
|  Gravity  |  g  |  重力を指定する  |
```yml
# 記述方法 #

Skills:
- projectile{ot=skill1;oh=skill2;hp=false;hnp=true;v=10;d=200;g=-1} @target
```

* **heal**

指定した量だけ回復する
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  amount  |  a  |  回復する量  |
|  overheal  |  oh  |  限界突破☆して回復するか  |
```yml
# 記述方法 #

Skills:
- heal{a=100;oh=true} @target
```

* **damage**

指定したダメージを与える
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  amount  |  a  |  ダメージ量  |
|  ignoreArmor  |  ia  |  防具を無視するか  |
|  preventknockback  |  pkb, pk  |  ノックバックするかどうか  |
|  preventimmunity  |  pi  |  無敵時間貫通して攻撃するか  |
|  element  |  type  |  damageのタイプを指定  |
|  damagecause  |  cause  |  ↓いっぱいある↓  |
```
# damageCause # 

  - entity_attack
  - thorns
  - magic
  - fire
  - fire_tick
  - fall
  - freeze
  - void
  - dragon_breath
  - lava
  - hot_floor
```
```yml
# 記述方法 #

Skills:
- damage{a=10;ia=true;pi=true} @target
```

* **message**

メッセージを送信する
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  message  |  msg,m  |  送るメッセージの内容をここに書く  |
```yml
# 記述方法 #

Skills:
- message{m="&c&l最高&e&l！&rうーん、&c&l最高&e&l！"} @target
```

* **potion**

ポーション効果を与える
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  type  |  t  |  与えるポーション効果の[タイプ](https://git.mythiccraft.io/mythiccraft/MythicMobs/-/wikis/Items/Potions)  |
|  duration  |  d  |  ポーション効果を与えるtick  |
|  level  |  l  |  ポーション効果のレベル  |
```yml
# 記述方法 #

Skills:
- potion{type=SLOW;d=200;l=4} @target
```

* **ignite**

ターゲットを燃やす
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  ticks  |  t,d,duration  |  燃焼するtick  |
```yml
# 記述方法 #

Skills:
- ignite{t=100} @target
```

* **swap**

ターゲットと自分の場所を入れ替える
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  none  |  none  |  none  |
```yml
# 記述方法 #

Skills:
- swap @target
```

* **summon**

モブを召喚する
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  type  |  t  |  Mobのタイプを指定する (MM製Mob指定可)  |
|  amount  |  a  |  召喚するMobの量を指定する  |
|  radius  |  r  |  半径内の何処かに指定したMobを召喚する  |

```yml
# 記述方法 #

Skills:
- summon{t=superZombie;a=5;r=4} @self
```

* **raytrace**

即着弾するビームを出す
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  entityskill  |  eskill, es  |  ビームがエンティティにヒットした時に発動するスキル  |
|  locationskill  |  lskill, ls  |  ビームがブロックにヒットした時に発動するスキル  |
|  headshotskill  |  hskill, hs  |  ビームが頭にヒットした時に発動するスキル  |
|  maxdistance  | md  |  ビームが飛ぶ最大距離  |
|  accuracy  |  ac  |  ビームのばらつき  |
|  fluidcollisionmode  |  fcm  |  ビームが流体にヒットした時の挙動 [[詳細]](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/FluidCollisionMode.html)  |
|  verticalnoise  |  vn  |  ビームの垂直方向の広がり  |
|  horizontalnoise  |  hn  |  ビームの水平方向の広がり  |
|  raytraceConditions  |  rc, rcon, rconditions  |  ヒットしたターゲットに適用される条件  |
|  headshotmultiplier  |  hsmultiplier, hsm  |  ビームが頭にヒットした時のダメージ倍率(恐らくentityskillに記述したdamageメカニックのamount参照)  |

```yml
# 記述方法 #

Skills:
- raytrace{
      ees=[
        - damage{a=20}
      ];
      hs=[
        - effect:particles{particle=reddust;color=#ff0000}
        - damage{a=10}
      ];
      ls=[
        - particles{p=flame;a=20;s=0.2;hS=0.1;vS=0.1}
      ];
      hsm=1.5;
      ac=0.995
      } @targetLocation
```

* **remove**

モブを消す
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  none  |  none  |  none  |

```yml
# 記述方法 #

Skills:
- remove @target
```

* **orbital**

ターゲットの周囲を回転する軌道を出す
|  オプション  |  略称系  |  説明  |
| ---- | ---- | ---- |
|  onTick  |  ot  |  毎Tick発動するスキル  |
|  onHit  |  oh  |  projectileがHitした時に発動するスキル  |
|  onStart  |  os  |  projectileが発射した時に発動するスキル  |
|  onEnd  |  oe  |  projectileが無くなった時に発動するスキル  |
|  Velocity  |  v  |  速度  |
|  Interval  |   i   |  位置を更新するtick  |
|  Duration  |  md  |  最大持続時間   |
|  HitPlayers  |  hp  |  Playerに当たるか  |
|  HitNonPlayers  |  hnp  |  Player以外のEntityに当たるか  |
|  Points  |  p  |  軌道を構成する点の数。  |
|  Radius  |  r  |  周囲の軌道の半径  |
|  Charge  |  c  |  指定した回数分、回転した後に軌道が停止する  |

```yml
# 記述方法 #

Skills:
- orbital{oT=[
           - effect:particles{p=snowballpoof;amount=20;speed=0;hS=0.2;vS=0.2} @origin
           ];
           oH=[
           - damage{a=10}
           - potion{type=SLOW;duration=100;lvl=2}
           ];
           p=20;
           i=1;
           d=200;
           c=1
           } @self
```
## その他
- さらに詳しく知りたい人は <https://git.mythiccraft.io/mythiccraft/MythicMobs/-/wikis/home> を参考にすると良いです。
- 自鯖環境で試したい人は <https://mythiccraft.io/index.php?pages/official-mythicmobs-download/> でダウンロードしてください。
