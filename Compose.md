

## State

### mutableState











### derivedStateOf

依赖其他状态的状态

Text 组件使用的状态 `capitalizedName` 的值。

```kotlin
var name by remember { mutableStateOf("android") }
val capitalizedName = remember { name.uppercase() }

Text(capitalizedName, modifier.clickable {
    name = "ios"
})
```

上面两个状态，Text 组件使用的 `capitalizedName` 依赖于 `name` 状态，当点击 Text 组件，在事件中修改 `name` 状态为 `ios`，但是 Text 组件使用的 `capitalizedName` 状态值并没有因为它依赖的状态 `name` 改变而重新计算 `capitalizedName` 状态的值，并且没有进行重组 Text 组件，Text 组件显示的还是 `ANDROID`。

derivedStateOf 可以解决上面的问题，我们修改代码如下：

```kotlin
var name by remember { mutableStateOf("android") }
val capitalizedName by remember { derivedStateOf { name.uppercase() } }

Text(capitalizedName, modifier.clickable {
    name = "ios"
})
```

这是点击 Text 组件，按我们预期的效果 Text 组件文字变成了 `IOS`。

derivedStateOf 确实解决了上面的这种情况，但是回顾我们在学习 `remember` 时，是不是 `remember` 可以传递一个参数，当这个参数变化时 `remeber` 函数的 `lambda` 会重新执行，重新计算出状态的值，看下面代码：

```kotlin
var name by remember { mutableStateOf("android") }
val capitalizedName = remember(name) { name.uppercase() }

Text(capitalizedName, modifier.clickable {
     name = "ios"
})
```

当我们再次点击 Text 组件时，Text 所显示的 文字编程了 `IOS`。那这里我们其实发现了一个很奇怪的问题，既然我们使用 `remember` 可以解决：**A 状态依赖 B 状态，当修改 B 状态后，使用 A 的组件重组范围被标记为失效，等下次重组的时候，更新最新值**。那么官方为什么还要给出 derivedStateOf ？ 既然给出了 derivedStateOf ？ 肯定有官方的原因，那么是什么原因呢？

要说明上面两个问题，我们就要再给出一个列子：

```kotlin
val names = remember { mutableStateListOf("android","ios") }
val capitalizedNames = remember(names) { names.map { it.uppercase() } }
Column {
     capitalizedNames.forEach {
          Text(it, modifier.clickable {
               names.add("linux")
           })
        }
}
```

上面列子很简单，我们的意图就是点击 Text 组件（任意一个都行），原来显示的 `ANDROID` 和 `IOS`，再多一个 `LINUX` 在它们两的下边。但是当运行到模拟器上，点击后没有任何反应。这是为什么？

这里直接给出答案，`remember` 函数的参数 `key` ，在重组时会对比 **上一次重组时保存的 key**  和 **本次重组时传入的 key**  是不是相等，从上面代码看，上一次 `remember`  返回的 `mutableStateList` 对象 `names` 和 本次 `remeber` 返回的 `mutableStateList` 对象，由于 `remeber` 的特性可知，这个两次两个对象是一个对象，所以 key 值相等，`remember` 中的 `lambda` 并比会执行，所以上面例子中 `Column`  `lambda` 重组范围并不会重新执行，那么界面上就还是 `ANDROID` 和 `IOS`。

同上面的例子，我们反正出，但靠 `remember` + `key` 并不能解决 **一个状态依赖另一个状态** 时所有的状态使用情况。那么上面这群情况下，就需要用 derivedStateOf。我们把上面例子改一下：

```kotlin
val names = remember { mutableStateListOf("android", "ios") }
val capitalizedNames by remember { derivedStateOf { names.map { it.uppercase() } } }
Column {
     capitalizedNames.forEach {
            Text(it, modifier.clickable {
                names.add("linux")
            })
        }
}
```

我们运行起来再点击一下 Text 组件，发现是我们要的效果，`ANDROID` 、`IOS` 和 `LINUX` 都以 竖排出现在屏幕上。

从上面的讲解中，我们可以总结出：

当 状态 A 依赖状态 B ，伪代码如下：

```
val b = remember{ }
val a = remember{ b + 1}
```

1. `val  a = remember(key){ b+1 }`函数中的 key ，当这个 key 对象内部进行改变时，并不会引起使用了这个函数返回状态 a 的组件们发生必要的重组。
2. 在 1 这种状态下，当明确 key 对象上次和本次重组时，对象内部发生变化而无法判断事，就应用使用 `remember{ derivedStateOf{ b + 1} }`，来解决上面问题。











## CompositionLocal

**具有穿透函数功能的局部变量** 

**不需要显示传递的函数参数**

```kotlin
val LocalName = compositionLocalOf<String> { error("No Provide Value") }

CompositionLocalProvider(LocalName provides "Android"){
	Text(LocalName.current)
}
```

使用场景 ：上下文 / 主题  

定义 CompositionLocal 有没有默认值视情况而定，但大多数情况下抛异常。

1. compositionLocalOf 跟踪使用它的值，当值发生变化时 重组刷新使用到值的组件。（有跟踪上的性能消耗，但当它改变时并不需要全部重组刷新）
2. staticCompositionLocalOf 不跟踪使用值，但当值变化时，会进行全部范围重组。（没有了跟踪时的性能消耗，但却提高了当它改变时的性能消耗）

什么时候用 compositionLocalOf 创建 CompositionLocal ，使用时候使用 staticCompositionLocalOf 创建 CompositionLocal？

<<<<<<< HEAD
# Animation

## AnimaXxxState

# 动画

动画的api 共性

animateXxxAsState 状态是 remember + State。重组时 即会被 remember ，也有 State 记录被调用处的功能，重组时，只刷新被记住的位置。



## animateXxxState 状态转移型动画

animateState 是 State 不是 MutableState

State 不能手动修改 State 中的值

```kotlin
    var big by remember { mutableStateOf(false) }
    val size by animateDpAsState(if (big) 120.dp else 48.dp)
    Box(
        modifier = modifier
            .size(size)
            .background(Color.Green)
            .clickable {
                big = !big
            }, contentAlignment = Alignment.Center) {
        Text(
            text = "Hello $name!",
            modifier = Modifier.padding(16.dp)
        )

    }
```

## Animatable

实现上面一样的动画使用 Animatable

```kotlin
   var big by remember { mutableStateOf(false) }
   val size = remember(big) { if (big) 120.dp else 48.dp }
   val anim = remember { Animatable(size, Dp.VectorConverter) }

    LaunchedEffect(big) {
        // anim.snapTo(size) 不以动画完成，瞬间到达某个动画值
        anim.animateTo(size)
    }
    Box(
        modifier = modifier
            .size(anim.value)
            .background(Color.Green)
            .clickable {
                big = !big
            }, contentAlignment = Alignment.Center
    ) {
        Text(
            text = "Hello $name!",
            modifier = Modifier.padding(16.dp)
        )
    }
```
