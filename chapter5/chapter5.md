## 业务逻辑
我大概画了个图，画的不是很好看，大概知道意思就好
<div align=center><img src="https://raw.githubusercontent.com/DengBoCong/guide-log/master/res/font.png" width="600" height= "350"></div>

## 本地安装启动
+ `git clone git@github.com:DengBoCong/fe-ubiquity.git`
+ `cd` 到项目根目录
+ 执行命令安装依赖 ` npm i `
+ 本地启动

+ 启动命令 `npm run no_parse`，此时 node 服务会占用本地  3000 端口启动。
+ 编译本地 js `npm run es6`
+ 编译本地 css `npm run scss`
+ 启动 js es6 的编译。一般用来在本地检查语法错误。
+ 编译本地 scss 样式文件 `npm run scss`
+ 需要注意的是：关于 js： gulp 打包时入口是 `/js/**/*_main.js`, gulp 会检测到这样格式的文件，使用 `rollup` 进行以来分析，并最终打包成一个 js 文件。所以新建页面引用的 js 文件必须是 `*_main.js` 的命名格式。

## 项目常用文件目录
+ controller：`controller/mobile/parent.js`


+ *.marko 文件(页面)：`views/mobile/parent/parent_ai/`

+ *_main.js 文件（每个页面编译前的 js）：`development/js/mobile/parent/js/parent_ai/`

+ 编译后输出的 js 文件：`static/mobile/parent/js/parent_ai/`

+ *.scss 文件（编译成每个页面 css 文件的 [sass](https://www.sass.hk/) 文件）：`development/scss/mobile/parent/css/parent_ai/css/`

+ 编译后输出的 css 文件：`static/mobile/parent/css/parent_ai/css/`

+ 图片资源位置：`static/mobile/parent/css/parent_ai/images`

+ 新建 *_main.js
在 `development/js/mobile/parent/js/parent_ai/` 下新建 js。

+ 新建 scss
在 `development/scss/mobile/parent/css/parent_ai/css/` 下新建 *scss 文件。然后执行命令 `npm run scss`，这时 `static/mobile/parent/css/parent_ai/css/` 下就会出现编译后的 *.css 文件。修改样式都在这个 *.css 文件里，__修改完之后复制回 *.scss 文件。

+ 页面图片资源
如果需要的话在 `static/mobile/parent/css/parent_ai/images` 下添加图片资源。一般需要检查下图片资源是否被压缩过。
