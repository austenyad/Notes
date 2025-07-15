

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

