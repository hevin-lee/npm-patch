# npm-patch
patch-package 对npm包打补丁，在团队开发的项目中，一般会依赖很多npm包。 有这样一种情况：我们想在某个npm包中自己添加一段代码或者修复某个紧急bug，以实现某个想要的功能。

## 如何使用

- 安装依赖包
```js
 // 官方推荐同时安装 postinstall-postinstall
yarn add patch-package postinstall-postinstall
```

原因简单总结就是：
只有patch-package时，运行yarn / yarn add packageA ，会自动运行补丁，使之生效。但运行 yarn remove，就不会自动运行补丁。 因此postinstall的作用就是弥补在执行yarn remove后，也可以使补丁生效。

-  在package.json中添加命令
```js
  "scripts": {
   "postinstall": "patch-package"
 }
```
- 修改npm包

  从node_modules目录中，找到要修改的包-具体的文件，添加修改内容后，
首先在本地开发查看修改是否生效：
这里要注意两点：

如果这个包同时有src目录和lib或叫dist目录，那可能要修改lib或dist目录下的文件，改动才能生效。因为最终执行的应该是拿src代码去构建得到的lib/dist目录下的文件。
修改完node_modules中的代码后，应该要重启本地开发，在本地才会用新代码运行
在本地可以先确认好要修改的文件，修改成功，达到想要的效果后，就可以打补丁了：


- 打补丁
  
  需要手动执行命令：
yarn patch-package your-package-name
运行这个命令后，在项目的根目录下会生成 patchs 目录，
目录下生成一个 package-name+x.x.x .patch 文件，即包名+版本号，以.patch为后缀的一个文件。
打开该文件，可以看到文件的内容，是记录了修改前后的diff，不同之处。



- 补丁包就和项目或者产品共存了

