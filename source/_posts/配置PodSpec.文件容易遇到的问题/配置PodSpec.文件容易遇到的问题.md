---
title: 配置PodSpec.文件容易遇到的问题
data: 2017-07-05 16:11:32
categories:
tags:
   - 黑魔法
---
<font color = black size = 3> 前言:相信大部分的iOS程序员都想将自己写的工具上传到cocoaPods供大家使用，网上关于这方面的教程已经太多太多了，但是如果你真正亲自去实践一次，你就会发现里面有很多坑等着你，并不像别人说的轻轻松松就把代码托管到了cocoaPods上面，最近闲来无事，我在经过无数次失败以后，终于使自己的PodSpec.文件通过了验证，这里我将自己遇到的坑给大家列出来，希望能够帮到那些有这方面需求的程序猿。</font>


一:第一坑，所有需要验证的PodSpec.文件都必须要打tag值，不然不能通过验证。(注意:每次打的tag值必须比上次打的tag值大，而且你的版本号也不能小于你的tag值，不然也不能验证通过)
<!-- more -->

二:第二坑,所有的配置文件验证都是跟github上面的代码进行对比的，所以在修改了本地PodSpec.文件以后，请先上传代码到github再进行 pod lib lint 命令验证


三:第三坑,公开头文件的目录请用单引号括起来,如下:
![image](http://upload-images.jianshu.io/upload_images/1863813-aadb22a032833b06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

四:如果你在终端中出现了以下显示，证明你的项目已经发布成功了
![image](http://upload-images.jianshu.io/upload_images/1863813-97c0e3583265057a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

五:一般情况下，在你提交成功以后，其实还是不能再终端中通过pod search 命令搜索到你托管的第三方的，你需要到cocoapods去认领你的第三方
[传送门](https://trunk.cocoapods.org/claims/new)

六:之后就一直等待发布一段时间，然后就能在cocoapods上面搜索到你的第三方了(注：有时候还是搜索不出来你发布第三方，我们这个时候就可以只通过名字来搜索你的第三方 )

pod search +三方名  + simple   

(注:一开始我只能通过上面那种+simple的方式搜索出来，我当时是怀疑是自己的图片资源放在项目中，但是并没有在spec文件中设置规则来约束它，因为我的spec文件在进行验证的过程中报警告了，最后操作了一番，不需要加simple也能搜索出来了。)

(1):  如果还是搜索不出来那就执行 pod setup

发现显示的还是 Unable to find a pod with name, author, summary, or descriptionmatching 'LFBPageScrollView'。

(2)：如果搜索不出来那就更新pod  :

命令:  pod repo update   然后就是等待更新结果

(3): 这个时候咱们就将生成的json文件给删除了

pod setup成功后会生成~/Library/Caches/CocoaPods/search_index.json文件。

终端输入rm ~/Library/Caches/CocoaPods/search_index.json

删除成功后再执行pod search

(4):执行pod search 

终端输入：pod search LFBPageScrollView(不区分大小写)

输出：Creating search index for spec repo 'master'.. Done!，稍等片刻就会出现所有带LFBPageScrollView字段的类库出现；如果在这种情况下还是搜索不到你的第三方，那么你就只有再仔细检查一下，你之前哪里做的不对。

关于如何更新版本的问题:

在很多时候，我们需要更新迭代我们发布的第三方，我们这个时候只需要做以下几件事就可以轻松的更新原来的东西。

1.需要将spec文件拷贝过来

2.将spec的version 和tag 都改一下

3.通过git打一个新的大于以前的tag值

4.通过pod spec lint 来验证spec文件是否有效

5.将新的spec文件推送到cocoapods上面去

6.再次到cocoapods官网去认领自己的第三方(注:朋友说需要再次领取，也有人说不需要再次认领，不需要再次认领的情况我还是没尝试过，等我试过以后再给大家分享。)


  最后再送上的第三方名字LFBPageScrollView,大家在用过以后遇到什么问题，都可以向我反馈。我会继续迭代.......
                                    注:(转载请注明出处!!!)
