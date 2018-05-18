---
title: android--任务和返回栈
tags: 任务栈
grammar_cjkRuby: true
---

## 任务

任务是一个Activity的集合，它使用栈的方式来管理其中的Activity，这个栈又被称为返回栈(back stack)，栈中Activity的顺序就是按照它们被打开的顺序依次存放的。

![enter description here][1]

任务是一个整体，应用A在被用户通过home键回到桌面后，应用A的任务栈就会进入“后台”，任务栈中所有的Activity全部停止，但是该任务栈是不变的。当另一个任务发生时，该任务仅仅只是失去了焦点，当用户再次切换到该应用时，用户能看到该应用还处在离开时的状态。

Activity 和任务的默认行为总结如下：

- 当 Activity A 启动 Activity B 时，Activity A 将会停止，但系统会保留其状态（例如，滚动位置和已输入表单中的文本）。如果用户在处于 Activity B 时按“返回”按钮，则 Activity A 将恢复其状态，继续执行。
- 用户通过按“主页”按钮离开任务时，当前 Activity 将停止且其任务会进入后台。 系统将保留任务中每个 Activity 的状态。如果用户稍后通过选择开始任务的启动器图标来恢复任务，则任务将出现在前台并恢复执行堆栈顶部的 Activity。
- 如果用户按“返回”按钮，则当前 Activity 会从堆栈弹出并被销毁。 堆栈中的前一个 Activity 恢复执行。销毁 Activity 时，系统不会保留该 Activity 的状态。
- 即使来自其他任务，Activity 也可以多次实例化。

### 管理任务
任务是可以管理的，可以通过使用 `<activity> `清单文件元素中的属性和传递给 startActivity() 的 Intent 中的标志，您可以执行所有这些操作以及其他操作。

在这一方面，您可以使用的主要 `<activity>` 属性包括：

- taskAffinity：管理Activity与任务的关联（优先属于哪个任务）
- launchMode：管理Activity的启动模式
- allowTaskReparenting
- clearTaskOnLaunch：是否每当从主屏幕重新启动任务时都从中移除根 Activity 之外的所有 Activity 
- alwaysRetainTaskState
- finishOnTaskLaunch

您可以使用的主要 Intent 标志包括：

- FLAG_ACTIVITY_NEW_TASK
- FLAG_ACTIVITY_CLEAR_TOP
- FLAG_ACTIVITY_SINGLE_TOP





  [1]: ./images/1522390921165.jpg