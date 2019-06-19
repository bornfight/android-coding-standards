# Kotlin standards

## 1. Keep camelCase naming in xml files
Because we make use of kotlinx.synthetic package for using xml ids without having to explicitly bind them, let's keep the code neat and consistent by not having multiple case types.
We stick to camelCase, as follows:


### Do:
```xml
<View
    android:id="@+id/aViewUsedForThisAndThat"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

### Don't:
```xml
<View
    android:id="@+id/a_view_used_for_this_and_that"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

##
## 2. Don't go all out on extension functions/parameters
Before making an extension function, think about whether it really should become one. The power to add code into the SDK API is great, but if we had 200 extension functions, our code would be readable to nobody, since it would mostly be the language you created yourself, from Kotlin.

Therefore, think about how often will you use the function, and whether it's simple and convenient enough that you could even find it in the official SDK. (in another dimension, or of some other language/platform)

### Do:
```kotlin
val Float.dp: Int
    get() = (this * Resources.getSystem().displayMetrics.density).toInt()
```

### Don't:
```kotlin
fun View.makeThisViewShineAndFlickerIfItIsDawn() {
    val scaleX = ObjectAnimator.ofFloat(accentBottomLineView, "scaleX", 0f, 1f)
    scaleX.interpolator = AccelerateDecelerateInterpolator()
    scaleX.duration = 300
    ...
}

```

##
## 3. No getters and setters!
Kotlin introduced properties, which are an upgraded notation of fields from Java. The Kotlin syntax imlpies a lot of things on it's own, so we don't have to define them explicitly, and among other things, we lost the need to write getters and setters. (check the official [docs](https://kotlinlang.org/docs/reference/properties.html) for details).

Remember that you can define properties and backing fields in interfaces/abstract classes as well!

### Do:
```kotlin
interface SomeInterface {
    val fullName: String
}

data class FacebookPageInfo(
    var name: String = "",
    var surname: String = ""
) : SomeInterface {
    override val fullName: String
        get() = "$name $surname"
}
```

### Don't:
```kotlin
interface SomeInterface {
    fun getFullName(): String
}

data class FacebookPageInfo(
    private var name: String = "",
    private var surname: String = ""
) : SomeInterface {
    override fun getFullName(): String {
        return "$name $surname"
    }
    
    fun getName(): String {
        return name
    }
    
    fun getSurname(): String {
        return name
    }
}
```

##
## 4. Default parameters for class properties
Kotlin is strict but righteous about nulls, and we should make use of this as much as possible. Nulls in Kotlin are no longer a headache (compared to Java), but are now used as a feature.

Usually, you shouldn't encounter nulls in Kotlin. However, we often use the null logic when we fear something hasn't initialised yet, and we can do it nicely and properly thanks to the Kotlin syntax.

However, in **models**, we should set default parameters for as much as possible fields, since those are usually primitives for which we **can** assign defaults to, without worrying later whether something had gone wrong. 

### Do:
```kotlin
data class MarkdownText(
        var id: Long = 0,
        var name: String = "",
        var active: Boolean = true,
        var color: String = "#000000",
        var chapterIds: ArrayList<Long> = emptyList()
) : BaseModel()
```

### Don't:
```kotlin
fun View.makeThisViewShineAndFlickerIfItIsDawn() {
data class MarkdownText(
        var id: Long? = null,
        var name: String? = null,
        var active: Boolean? = null,
        var color: String? = null,
        var chapterIds: ArrayList<Long>? = null
) : BaseModel()
}
```

