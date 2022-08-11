# 

# 个性化

## 配置项

* [配置项](https://docsify.js.org/#/zh-cn/configuration)

  **自定义封面背景**

  默认的背景是随机生成的渐变色，每次刷新都会显示不同的颜色。docsify封面支持自定义背景色或者背景图，在`_coverpage.md`文档末尾添加

  ```markdown
  <!-- 背景图片 -->
  ![](_media/bg.png)
  
  <!-- 背景色 -->
  ![color](#2f4253)
  ```

  > 自定义背景配置一定要在`_coverpage.md`文档末尾
  >
  > 背景图片和背景色只能有一个生效
  >
  > 背景色一定要是`#2f4253`这种格式的

  **封面为首页**

  配置封面后，封面和首页同事出现。封面在上面，首页在下面。通过设置`onlyCover`参数，分开显示。在`index.html`文件中的`window.$docsify`添加`onlyCover: true`

  ```js
    window.$docsify = {
      coverpage: true,
      onlyCover: true,
    }
  ```

  **设置不显示目录**

  设置了`subMaxLevel `后，默认情况下每个标题都会自动添加到目录中。如果你想忽略特定的标题，可以修改`_sidebar.md`给它添加 <!-- {docsify-ignore} -->，要忽略特定页面上的所有标题，你可以在页面的第一个标题上使用 <!-- {docsify-ignore-all} -->

  ```markdown
  # 忽略全部标题 <!-- {docsify-ignore-all} -->
  ## 忽略部分标题 <!-- {docsify-ignore} -->
  ```

## 主题

* [主题](https://docsify.js.org/#/zh-cn/themes)  

## 插件列表

* [插件列表](https://docsify.js.org/#/zh-cn/plugins)

**字数统计**

在index.html文件中，引入countable.js，增加count配置项，可设置统计显示的语言和样式。

```html
<body>
   <script>
      window.$docsify = {
        count:{
          countable:true,
          fontsize:'0.9em',
          color:'rgb(90,90,90)',
          language:'chinese'
        }
      }
    </script>
  <script src="//unpkg.com/docsify-count/dist/countable.js"></script>
</body>
```

**Gitalk配置**

[Error: Not Found](https://www.cnblogs.com/bigyoung/p/14154060.html)

[Error: Validation Failed](https://blog.csdn.net/Keith_Prime/article/details/111604291)

[整个网站使用一个gitalk评论issues的问题解决方式](https://segmentfault.com/a/1190000018072952)

**谷歌统计**

[谷歌统计 - Google Analytics](https://docsify.js.org/#/zh-cn/plugins?id=谷歌统计-google-analytics)

> [Google Analytics简单教程](https://www.zhihu.com/question/19599402/answer/1904935774)

## 其它

**Live 2D所有模型展示图，看板娘**

> [各种模型可直接复制链接](https://cloud.tencent.com/developer/article/2048383)
>
> [看板娘增加说明](https://www.cnblogs.com/laolieren/p/add_2d_live_for_mediawiki.html)
>
> [看板娘案例，github展示](https://github.com/raoenhui/live2d-example)

**网页特效**

> [十个拿来就能用的网页炫酷特效](https://blog.csdn.net/weixin_45660485/article/details/124185605)

# 表情符号

🌹🍀🍎💰📱🌙🍁🍂🍃🌷💎🔪🔫🏀⚽⚡👄👍🔥♈♉♊♋♌♍♎♏♐♑♒♓💝💞💟❣❤🧡💛💚💙💜🤎🖤🤍

## 常用emoji符号一

😀😁😂😃😄😅😆😉😊😋😎😍😘😗😙😚☺😇😐😑😶😏😣😥😮😯😪😫😴😌😛😜😝😒😓😔😕😲😷😖😞😟😤😢😭😦😧😨😬😰😱😳😵😡😠😈👿👹👺💀👻👽👦👧👨👩👴👵👶👱👮👲👳👷👸💂🎅👰👼💆💇🙍🙎🙅🙆💁🙋🙇🙌🙏👤👥🚶🏃👯💃👫👬👭💏💑👪💪👈👉☝👆👇✌✋👌👍👎✊👊👋👏👐✍👣👀👂👃👅👄💋👓👔👕👖👗👘👙👚👛👜👝🎒💼👞👟👠👡👢👑👒🎩🎓💄💅💍🌂

## 常用emoji符号二

🙈🙉🙊🐵🐒🐶🐕🐩🐺🐱😺😸😹😻😼😽🙀😿😾🐈🐯🐅🐆🐴🐎🐮🐂🐃🐄🐷🐖🐗🐽🐏🐑🐐🐪🐫🐘🐭🐁🐀🐹🐰🐇🐻🐨🐼🐾🐔🐓🐣🐤🐥🐦🐧🐸🐊🐢🐍🐲🐉🐳🐋🐬🐟🐠🐡🐙🐚🐌🐛🐜🐝🐞🦋💐🌸💮🌹🌺🌻🌼🌷🌱🌲🌳🌴🌵🌾🌿🍀🍁🍂🍃🌍🌎🌏🌐🌑🌒🌓🌔🌕🌖🌗🌘🌙🌚🌛🌜☀🌝🌞⭐🌟🌠☁⛅☔⚡❄🔥💧🌊💩🍇🍈🍉🍊🍋🍌🍍🍎🍏🍐🍑🍒🍓🍅🍆🌽🍄🌰🍞🍖🍗🍔🍟🍕🍳🍲🍱🍘🍙🍚🍛🍜🍝🍠🍢🍣🍤🍥🍡🍦🍧🍨🍩🍪🎂🍰🍫🍬🍭🍮🍯🍼☕🍵🍶🍷🍸🍹🍺🍻🍴

## 常用emoji符号三

🎪🎭🎨🎰🚣🛀🎫🏆⚽⚾🏀🏈🏉🎾🎱🎳⛳🎣🎽🎿🏂🏄🏇🏊🚴🚵🎯🎮🎲🎷🎸🎺🎻🎬👾🌋🗻🏠🏡🏢🏣🏤🏥🏦🏨🏩🏪🏫🏬🏭🏯🏰💒🗼🗽⛪⛲🌁🌃🌆🌇🌉🌌🎠🎡🎢🚂🚃🚄🚅🚆🚇🚈🚉🚊🚝🚞🚋🚌🚍🚎🚏🚐🚑🚒🚓🚔🚕🚖🚗🚘🚚🚛🚜🚲⛽🚨🚥🚦🚧⚓⛵🚤🚢✈💺🚁🚟🚠🚡🚀🎑🗿🛂🛃🛄🛅💌💎🔪💈🚪🚽🚿🛁⌛⏳⌚⏰🎈🎉🎊🎎🎏🎐🎀🎁📯📻📱📲☎📞📟📠🔋🔌💻💽💾💿📀🎥📺📷📹📼🔍🔎🔬🔭📡💡🔦🏮📔📕📖📗📘📙📚📓📃📜📄📰📑🔖💰💴💵💶💷💸💳✉📧📨📩📤📥📦📫📪📬📭📮✏✒📝📁📂📅📆📇📈📉📊📋📌📍📎📏📐✂🔒🔓🔏🔐🔑🔨🔫🔧🔩🔗💉💊🚬🔮🚩🎌💦💨💣☠♠♥♦♣🀄🎴🔇🔈🔉🔊📢📣💤💢💬💭♨🌀🔔🔕✡✝🔯📛🔰🔱⭕✅☑✔✖❌❎➕➖➗➰➿❓❔❕❗©®™🎦🔅🔆💯🔠🔡🔢🔣🔤🅰🆎🅱🆑🆒🆓ℹ🆔Ⓜ🆕🆖🅾🆗🅿🆘🆙🆚🈁🈂🈷🈶🈯🉐🈹🈚🈲🉑🈸🈴🈳㊗㊙🈺🈵▪▫◻◼◽◾⬛⬜🔶🔷🔸🔹🔺🔻💠🔲🔳⚪⚫🔴🔵♈♉♊♋♌♍♎♏♐♑♒♓⛎💘❤💓💔💕💖💗💙💚💛💜💝💞💟❣🌿🚧💒☎📟💽⬆↗➡↘⬇↙⬅↖↕↔↩↪⤴⤵🔃🔄🔙🔚🔛🔜🔝🔀🔁🔂▶⏩◀⏪🔼⏫🔽⏬📱📶📳📴♻🏧🚮🚰♿🚹🚺🚻🚼🚾⚠🚸⛔🚫🚳🚭🚯🚱🚷🔞



> [emoji链接一](https://blog.csdn.net/x650007/article/details/120099777)
>
> [emoji链接二](https://www.zfuhao.com/emoji)

# 参考博客

* [rainbomsea](https://rainbomsea.xyz/) 部分插件参数配置
* [wugenqiang/NoteBook](https://notebook.js.org/)插件和自定义配置
* [Docsify-Guide](https://github.com/YSGStudyHards/Docsify-Guide)初始化

* [Awesome docsify](https://docsify.js.org/#/zh-cn/awesome)

* [字节飞扬](https://bytesfly.github.io/blog/) 

> 参考链接
>
> [docsify安装,docsify搭建,docsify使用教程](https://blog.csdn.net/qq_44633541/article/details/122749843)
>
> [入坑 docsify，一款神奇的文档生成利器](https://blog.csdn.net/weixin_39860849/article/details/110797836)
>
> [Docsify个人网站搭建详细教程](https://blog.csdn.net/m0_67393039/article/details/123251706)
>
> [docsify安装使用详解](https://blog.csdn.net/liyou123456789/article/details/124504727)

