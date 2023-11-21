---
layout: post
title: 关于wegame给vulkan下毒这件事
tags: 
- vulkan
date: 2023-11-21 09:31 +0800
last_modified_at: 2023-11-21 09:44 +0800
---
由wegame导致的vulkan程序出现的一系列问题及解决方案

## 症状

1. vulkan程序在编译时，会在目的文件夹出现文件
    {% highlight text %}
    项目名.年月日-时分秒-毫秒.log
    {% endhighlight %}
    无法使用文本编辑器正常打开

2. vulkan程序在运行时，若使用1.1以上版本的API，会输出warning信息：
    {% highlight text %}
    Layer VK_LAYER_TENCENT_wegame_cross_overlay uses API version 1.1 which is older than the application specified API version of 1.3. May cause issues.
    {% endhighlight %}

3. vulkan程序在运行时，会加载wegame的layer library,路径为
    {% highlight text %}
    WeGame\apps\Cross\Core\Stable\.\CrossVulkanLayer64.dll
    {% endhighlight %}

4. 在使用renderdoc调试vulkan程序时，会创建设备失败而无法运行

## 原因

因为wegame文件中包含了CrossVulkanLayer64.dll文件，而且所有vulkan程序在编译、运行时都会自动加载此dll文件，导致出现一系列问题

## 解决方案

在系统的环境变量中添加以下变量

{% highlight text %}
VK_LOADER_LAYERS_DISABLE
{% endhighlight %}

将其值设为

{% highlight text %}
VK_LAYER_TENCENT_wegame_cross_overlay
{% endhighlight %}
    
## 副作用

vulkan程序运行时会输出warning信息：

{% highlight text %}
Layer "VK_LAYER_TENCENT_wegame_cross_overlay" forced disabled because name matches filter of env var 'VK_LOADER_LAYERS_DISABLE'.
{% endhighlight %}