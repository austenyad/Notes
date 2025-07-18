# Android WorkManger

WorkManger 是 Andorid Jetpack 组件中，专门处理后台任务的。

Android 平台发展过程中，处理后台任务的工具很多，曾经出现过 Service 、IntentServce 、Job、JobService。如果你曾经用过他们处理过后台任务，那么对他们一定也很熟悉，也有可能把所有 任务都在 Activity 、Fragment 中完成，你也觉得我把所有的后台任务都放在 Activity 或者 Fragment 没什么问题。但是有些时候，有些任务，它可能会失败，失败后要一直重试执行，直到成功为止，也有可能会要间断性一直执行。如果把这些和 Activity 或 Fragment 绑定，那么有可能在这些 Activity 或者 Fragment 在页面关闭时，可能会导致它们回收不了，造成内存泄露。主要是 Activity 或者 Fragment 或者 View 它们有自己的生命周期，在这些里面启动任务执行，很大可能会影响它们对象及时的回收。总之，对于后台任务我们要在合理的情况下使用官方给定的特殊组件去执行，这些组件