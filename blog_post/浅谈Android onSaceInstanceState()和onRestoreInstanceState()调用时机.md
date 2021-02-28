---
title: Android
categories:
  - Android
tags:
  - Android
date: 2020-07-12 00:00:00
---
# 浅谈Android onSaceInstanceState()和onRestoreInstanceState()调用时机

> Android系统的回收机制会在**未经用户主动操作的情况下销毁Activity**,而为了避免系统回收activity导致数据丢失,Android为我们提供了onSaveInStaceState(Bundle outState)和onRestoreInstanceState(Bundle savedinstanceState)用户保存和恢复数据

## onSaveInStaceState(Bundle outState)在什么时机被调用

当它离开栈顶时会被调用,此时activity没有被销毁可能会被销毁.

1. 当屏幕息掉,再打开时
2. 用户按下home键时
3. 屏幕方向切换时
4. 从当前Activity开启一个新的Activity

当前activity的声明周期不确定:

**onPause->onSaveInstanceState->onStop或者onPause->onStop->onSaveInstanceState**

## onRestoreInstanceState()在什么时机被调用

调用时机为确定Activity被系统销毁后再打开avtivity时被调用

1. 屏幕旋转时
2. 系统因为资源有限杀死activity时

当前activity的声明周期:

**onPause -> onSaveInstanceState -> onStop -> onDestroy -> onCreate -> onStart -> onRestoreInstanceState -> onResume**



