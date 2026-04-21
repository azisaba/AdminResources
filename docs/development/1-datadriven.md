# データ駆動

「データ駆動」はアジ鯖での現代的なプラグイン開発において推奨される重要な設計思想です。

データ駆動では、ハードコーディングされたロジックの代わりに、純粋な「データ」に基づいて動作を決定します。

## 依存関係

[AzisabaNetwork/DataDriven](https://github.com/AzisabaNetwork/DataDriven) を使用して効率的にデータ駆動な開発を行うことができます。

`build.gradle.kts' に依存関係を追加します。

```kts
repositories {
    maven { url = uri("https://repo.azisaba.net/repository/maven-snapshots/") }
}

dependencies {
    implementation("net.azisaba.data:yaml:1.0.0-R0.10-SNAPSHOT")
    implementation("net.azisaba.data:paper:1.0.0-R0.10-SNAPSHOT")
}
```

## Contents API

Minecraftサーバーはコンテンツの集合として理解できます。

そして、コンテンツはコード上では純粋なデータで表現されるべきです。
これによって、ゲームの拡張性と保守性を劇的に向上できます。

```kotlin
@Serializable
data class Sushi(val name: String, val price: Int)
```

純粋なデータには、必要に応じて、状態を持たない純粋なメソッドを定義します。

```kotlin
@Serializable
data class Sushi(val name: String, val price: Int) {
    fun canBuy(budget: Int): Boolean {
        return budget >= price
    }
}
```

Contents APIでは `net.azisaba.data.contents.Contents<T : Any>` を使用してこうしたコンテンツの管理を行います。

```kotlin
val contents: Contents<Sushi> = /* インスタンスを用意 */

// ContentKeyから値を取得
val squidSushi: Sushi? = contents.byKey(ContentsKey.key("azisaba", "squid"))
val squidSushiNotNull: Sushi = contents.byKeyOrThrow(ContentsKey.key("azisaba", "squid"))

// 値からContentKeyを取得
val squidSushiKey: ContentKey<Sushi> = contents.keyOf(squidSushiNotNull)
val squidSushiKeyNotNull: ContentKey<Sushi> = contents.keyOfOrThrow(squidSushiNotNull)

// ContentKeyの一覧を取得
val contentKeys: Collection<ContentKey<Sushi>> = contents.contentKeys()

// 値の一覧を取得
val contents: Collection<Sushi> = contents.contents()

// kotlin.collections.Map<ContentKey<Sushi>, Sushi>として取得
val contentsMap: Map<ContentKey<Sushi>, Sushi> = contents.toMap()
```

`Contents<T : Any>` は `companion object` として実装します。

```kotlin
// 外部に定義されたYAMLファイルからロードする
// Sushi.bootstrap(pluginInstance.dataPath)のようにしてロードする。
@Serializable
data class Sushi(val name: String, val price: Int) {
    companion object : YamlDynamicContents<Sushi>(
        name = "sushi",
        serializer = lazy { Sushi.serializer() },
    )
}

// コード上で直接に定義する
// プラグインのonEnable()などでSushi.bootstrap()を呼び出す必要がある。
@Serializable
data class Sushi(val name: String, val price: Int) {
    companion object : StaticContents<Sushi>() {
        val SQUID: ContentKey<Sushi> = ContentKey.key("azisaba", "squid")
        val SALMON: ContentKey<Sushi> = ContentKey.key("azisaba", "salmon")
        val TUNA: ContentKey<Sushi> = ContentKey.key("azisaba", "tuna")
    
        override fun BindingBuilder<Sushi>.bootstrap() {
            bind(SQUID, Sushi("Squid Sushi", 100))
            bind(SALMON, Sushi("Salmon Sushi", 200))
            bind(TUNA, Sushi("Tuna Sushi", 300))
        }
    }
}

// 'net.kyori.adventure.key.Keyed' を実装したEnumクラスをコンテンツとして扱う
@Serializable
enum class Sushi(val key: Key, val name: String, val price: Int) : Keyed {
    SQUID(Key.key("azisaba", "squid"), "Squid Sushi", 100),
    SALMON(Key.key("azisaba", "salmon"), "Salmon Sushi", 200),
    TUNA(Key.key("azisaba", "tuna"), "Tuna Sushi", 300);

    companion object : EnumContents<Sushi>(Sushi::class)
}
```

`net.azisaba.data:paper` モジュールでは、Brigadierコマンドを生成する拡張メソッドが提供されています。

```kotlin
fun <T : Any> Contents<T>.toGetCommand(literal: String): LiteralCommandNode<CommandSourceStack>

fun <T : Any> DynamicContents<T>.toReloadCommand(literal: String): LiteralCommandNode<CommandSourceStack>
```
