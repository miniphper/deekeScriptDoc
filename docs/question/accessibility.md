# 无障碍问题

在无障碍程序设计过程中，我们和大家一样，遇到最多的问题，就是无障碍的不稳定，接下来主要介绍几个不稳定因素的起因和解决方案。

##### 场景1：APP运行一段时间就挂掉了？

> 原因：目前Android对后台应用限制比较严格，尤其内存不足或者电量不足的时候，会对相关应用进行误杀。
> 解决方案：目前可以根据各个品牌的机型，关闭对应的省电模式功能，或者插电运行，又或者给应用加锁（具体的可以查看对应品牌机型的加锁方法），当然也可以三者都使用，这样稳定性会更好。


##### 场景2：在页面A点击用户头像进入用户详情页，获取用户数据，最终失败

> 原因1：某些情况下，当前A页面可能存在多个id相同的头像控件，你的代码拿到的可能不是在当前界面上的控件，导致点击了其他控件
> 
> 解决方案：增加判断，确保你要获取的id在当前界面，可以通过位置信息确保控件在大概位置（或者判断是否可见等）

> 原因2：某些情况下，点击头像控件失效了，但是系统确实点击了（这种情况常见于某些APP框架，DeekeScript中暂未发现）
> 
> 解决方案：在点击的时候，尽可能使用控件的特性，假设控件的clickable为true，则直接使用obj.click()即可；或者发现它的父控件的clickable为true，则直接使用obj.parent().click()；只有在clickable为false的时候，才去使用系统提供的位置点击方法。

> 原因3：某些情况下被弹窗遮挡了，位置点击的时候跑到其他界面去了
> 
> 解决方案：对APP的弹窗进行全局的监控，发现之后进行关闭。


##### 场景3：可滑动的用户列表，对其每个用户进行操作时，经常因为多返回或者少返回，最终找不到用户而出错

> 原因1：操作用户的时候，国内某些APP框架可能手机卡顿导致返回没有执行（DeekeScript不会存在这种问题）
> 
> 解决方案：每次返回之后，做一个界面的判断，如果没有返回，则循环再返回

> 原因2：操作用户的时候，通过点击界面上的“返回”按钮返回，但是因为其他原因导致点击失败
> 
> 解决方案：尽量使用系统全局的返回，不要使用APP提供的“返回”按钮返回

> 原因3：因为操作用户的过程中出错了，导致返回异常
> 
> 解决方案：编写强壮的代码，每一步失败，都做到“心中有数”，然后执行对应的返回操作（或者在执行返回的时候，检测是否到了用户列表页）


##### 场景4：因为某个功能的操作周期比较长，应用程序经常挂掉了

> 原因1：有时候，我们在开发一款新的APP的时候（又或者APP功能比较多，更新比较频繁），经常会有一些超出我们预期的东西，比如：未知的弹窗，未知的界面等等（当然也包括手机来电等）
> 
> 解决方案：遇到异常，可以退回到主界面（每退一次检查一次是否进入了主界面），然后再重新运行；当然如果没办法进入主界面，应该也可以进入其他默认的界面，或者自行对当前页面进行判断，然后执行对应操作
> 
> 注意：尽量不要采用关闭APP，然后重启的方式，因为这种方式很难让客户接受（除非客户接受，并且必须这么做）


##### 场景5：在评论列表或者用户列表出现死循环了

> 原因1：没有列表已经执行完做判断，或者因为某些原因导致一直在滑动列表
>
> 解决方案：对列表执行完成进行判断（DeekeScript中，可以获取对象的_addr属性，比如一次获取到了3个控件，把_addr进行保存，然后滑动之后，再进行读取，与前面的比对，如果相同则认为滑动完成）
> 对滑动进行判断，超出时间或者次数，进行异常处理
