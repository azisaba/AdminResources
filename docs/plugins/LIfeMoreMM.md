# LifeMoreMythicMobs
## 用途
MythicMobsのMechanicやConditionを追加するPluginです。MythicMobsと記述方法は変わりません。

- - -

## Mechanics

---

### takeinv

#### 説明: インベントリから特定のアイテムを消すMechanic

### 属性:

|オプション|省略形|説明|デフォルト
|-----|-----|-----|-----|
|item|material, m, i|指定されたitemを削除する|null
|amount|a|指定された数だけ削除する|1

#### 例:

この例では、mmidが`cl6_shootItNow`のitemを、@targetから一つ消している

```yaml
takeInvSkill:
 Skills:
 - takeinv{i=cl6_shootItNow;a=1} @target
```

- - -

### particleVerticalRing

#### 説明: 縦方向のparticleRingを出すMechanic

### 属性:

|オプション|省略形|説明|デフォルト
|-----|-----|-----|-----|
|radius|r|円の半径|3
|points|p|円を描画するポイントの数|10
|amount|a|pointsに描画するパーティクルの数|1
|speed|s|パーティクルの「速さ」|0
|rotatex|rotx|x方向の回転|1
|rotatey|roty|y方向の回転|1
|rotatez|rotz|z方向の回転|1
|particle|pa|描画するパーティクルの種類|REDDUST
|color|c|パーティクルの色(一部のみ)| #ffffff
|ignoreEntityRotation|ier, i|プレイヤーの視点方向を参照するか|true
|uniform|uni, u|pointsを均等に配置するか|true

#### 例:

この例では、半径5、ポイント30の円形のend_rodを、視点方向の2ブロック先に出している

```yaml
particleVerticalRingSkill:
 Skills:
 - pvr{pa=end_rod;r=5;p=30} @Forward{f=2}
```

##### 省略形: `pvr`
- - -
### bossBar

#### 説明: 指定したbossbarをプレイヤーに見せるMechanic

### 属性:

|オプション|省略形|説明|デフォルト
|-----|-----|-----|-----|
|always|a, al|bossbarを削除しないか|false
|duration|d|bossbarが消えるまでのtick数|100
|progress|pro, p|bossbarの進行度 (0 - 1)|1
|title|t|bossbarのタイトル|1
|style|s|bossbarのスタイル|SOLID
|color|c|bossbarの色|RED
|id|i|bossbarに紐づけるID|def

#### 例:

この例では、自分に`"Mission進捗"`という名前で進捗が`0`のbossbarを400tick(20秒)表示している

```yaml
BossBarSkill:
 Skills:
 - bossbar{title=Mission進捗;color=YELLOW;progress=0;duration=400;id=cl6_mission} @self
```
- - -

### removebossbar

#### 説明: `bossbar`Mechanicで追加されたbossbarを削除する

### 属性:

|オプション|省略形|説明|デフォルト
|-----|-----|-----|-----|
|id|i|bossbarに紐づけられているid|def

#### 例:

この例では、自分に表示しているbossbarの内、紐づけられたidが`cl6_mission`のbossbarを削除している

```yaml
removeBossBarSkill:
 Skills:
 - removebossbar{id=cl6_mission} @self
```

##### 省略形: `bossbarremove`

- - -
### modifybossbar

#### 説明: `bossbar`Mechanicで追加されたbossbarを編集する

### 属性:

|オプション|省略形|説明|デフォルト
|-----|-----|-----|-----|
|id|i|bossbarに紐づけられているid|def
|progress|pro, p|bossbarの進行度 (0 - 1)|1
|title|t|bossbarのタイトル|1
|style|s|bossbarのスタイル|SOLID
|color|c|bossbarの色|RED

#### 例:

この例では、自分に表示されているbossbarの内、紐づけられたidが`cl6_mission`のbossbarのtitleを`Mission完了`に変更している

```yaml
modifyBossBarSkill:
 Skills:
 - modifybossbar{id=cl6_mission;title=Mission完了} @self
```

##### 省略形: `bossbarmodify`git

- - -
## Conditions
- - -

### realtime

#### 説明: 現在時刻を参照するCondition

### 属性:

|オプション|省略形|説明|デフォルト
|-----|-----|-----|-----|
|maxyear|maxy, may, 年次|最大の年|2147483647
|maxmonth|maxm, mam, 月次|最大の月|2147483647
|maxday|maxd, mad, 日次|最大の日|2147483647
|maxhour|maxh, mah, 時次|最大の時|2147483647
|maxminute|maxmi, mami, 分次|最大の分|2147483647
|maxsecond|maxs, mas, 秒次|最大の秒|2147483647
|minyear|miny, miy, 年前|最小の年|-2147483647
|minmonth|minm, mim, 月前|最小の月|-2147483647
|minday|mind, mid, 日前|最小の日|-2147483647
|minhour|minh, mih, 時前|最小の時|-2147483647
|minminute|minmi, mimi, 分前|最小の分|-2147483647
|minsecond|mins, mis, 秒前|最小の秒|-2147483647
|invert|i, 逆転|trueだったらfalse,falseだったらtrue|false

#### 例:

```yaml
 # 現在時刻が2020年から2025年の間だったらtrueを返す
realTimeSkill_1:
 Conditions:
 - realtime{minyear=2020;maxyear=2025;i=false}

 # 現在時刻が10日から20日の間だったらtrueを返す
realTimeSkill_2:
 Conditions:
 - realtime{minday=10;maxday=20;i=false}
```

- - -
## Placeholder
- - -
|Placeholder|関数
|-----|-----
|<caster.mmid>|MainHandに持っているItemのMYTHIC_TYPE(mmid)を返す

#### 例:

```yaml
comparison:
 Skills:
 - setVariable{var=caster.comparisonA;val="cl6_ShootItNow";type=STRING}
 - skill{s=comparisonA}

comparisonA:
 Conditions:
 - varEquals{var=caster.comparisonA;val=<caster.mmid>} true
 Skills:
 - message{m="&f手にShootItNowを持っている！<caster.name>君、めっちゃセンスあるね！"} @self
```