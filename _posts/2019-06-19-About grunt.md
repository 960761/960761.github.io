
## reference 

https://www.gruntjs.net/

https://webcache.googleusercontent.com/search?q=cache:FHTp7KkTKFUJ:https://yujiangshui.com/grunt-basic-tutorial/+&cd=1&hl=zh-CN&ct=clnk&gl=hk

https://github.com/yujiangshui/gruntxx

## Grunt由来：

构思这个工具的开发方法和需要的功能：  

首先我需要先开发一个工具，可以调用这个工具对我的某个项目目录里面的项目文件做一些操作，比如压缩、查错、合并等。  

如果要做成一个工具，可能不太好，或许别人还需要更多功能，但是我没法开发这么多功能啊。要不我就做个框架把，然后每个功能做成一个插件，比如压缩插件、合并插件。如果有人需要在他的项目里压缩某个文件，他安装一下我这个工具然后再安装压缩插件就好了。这样有更多需求的人，可以自己编写功能插件，然后配合我的工具使用。    

慢着，他安装完了工具和插件之后，要怎么来调用这个插件来处理项目文件？在程序界面上选择文件，然后勾选选项？我的天，我就会写点 JS，哪里可能开发带有界面的程序？慢着，用 JS ？他可以在项目文件夹中编写一个 JS 来设置任务啊！然后我的工具会读取这个 JS，解析之后获得他要执行的任务（比如压缩某某文件并改名成某某），然后调用插件完成任务。  

太棒了。但是插件这么多，放在项目里肯定很大，而且又是不相关代码，要不等他发布的时候自动删除这些插件文件把？不行，如果他要发给别人，别人要继续开发，还得重新安装依次安装这些插件，然后执行任务。那怎么办？要不我再用个文件记录一下当前项目中安装或者需要的插件把！这样只需要把这个文件和 JS 任务文件放在项目目录里面，有需要的人，直接输入一条命令安装一下，然后立刻就可以执行了。  
这样一种自动化任务处理工具，它就是一个工具框架，有很多插件扩展它的功能。

## Grunt理解
Grunt定义：一个可以自动化运行 各种处理前端繁琐操作（压缩文件，删注释，测试等等）的插件 的平台。

所谓的自动化，就是插件帮你处理了各种繁琐的操作。而grunt的作用就是作为主管分配任务给插件，实现自动处理。

Grunt 基于 Node.js ，用 JS 开发，这样就可以借助 Node.js 实现跨系统跨平台的桌面端的操作，例如文件操作等等。此外，Grunt 以及它的插件们，都作为一个包 ，可以用 NPM 安装进行管理。  
NPM 生成的 package.json 项目文件，里面可以记录当前项目中用到的 Grunt 插件，而 Grunt 会调用 Gruntfile.js 这个文件，解析里面的任务（task）并执行相应操作。  
Grunt是一个平台，没有插件Grunt什么事也不能做。它是提供了一个标准，任何插件都要去遵守的一个机制和规则。就和USB接口一样，接在USB插口的，有可能是U盘，摄像头这些完成不同功能的外设。Grunt有一个庞大的生态圈， 里面有各种各样现成的插件，可以满足大部分的日常需要。无需额外开发。  

## Grunt 安装
1.安装node.js,与npm，因为grunt平台基于node.js开发，它依赖于node.js，插件基于npm管理，所以两者是前提。

2.安装grunt命令行到系统，从而可以在任何位置使用grunt命令行工具运grunt 命令  
实际上，这里安装的并不是 Grunt，而是 Grunt-cli，也就是命令行的 Grunt，这样你就可以使用 grunt 命令来执行某个项目中的 Gruntfile.js 中定义的 task 。但是要注意，Grunt-cli 只是一个命令行工具，用来执行，而不是 Grunt 这个工具本身。    
安装 Grunt-cli 需要使用 NPM，使用下面一行即可在全局范围安装 Grunt-cli ，换句话说，就是你可以在任何地方执行 grunt 命令：  
npm install -g grunt-cli  
需要注意，因为使用 －g 命令会安装到全局，可能会涉及到系统敏感目录，如果用 Windows 的话，可能需要你用管理员权限，如果用 OS X ／ Linux 的话，你可能需要加上 sudo 命令。  

3.配置package.json 文件与 gruntfile.js 文件，  

## 核心文件：
每一个Grunt项目都会带上这两个文件：package.json, gruntfile.js  

npm命令运行时会读取当前目录的 package.json 文件和解释这个文件，这个文件基于Packages/1.1 规范。  

当在项目的根目录中运行npm install 时，npm会自动去安装dependencies中定义的依赖包，在项目的根目录中生成一个node_modules的文件夹，这个项目所需要的node库就安装在这个目录下。  

 在这个文件里你可以定义你的应用名称( name )、应用描述( description )、关键字( keywords )、版本号( version )、应用的配置项( config )、主页( homepage )、作者( author )、资源仓库地址( repository )、bug的提交地址( bugs )，授权方式( licenses )、目录( directories )、应用入口文件( main )、命令行文件( bin )、应用依赖模块( dependencies )、开发环境依赖模块( devDependencies )、运行引擎( engines )和脚本( scripts )等。  
Gruntfile.js放到下面结合示例具体讲解。

## Grunt示例：

比如你需要以下这些功能：  
大致功能：检查每个 JS 文件语法、合并两个 JS 文件、将合并后的 JS 文件压缩、将 SCSS 文件编译、新建一个本地服务器监听文件变动自动刷新 HTML 文件。
差不多就是这些，根据这些任务需求，需要用到：  

•合并文件：grunt-contrib-concat
•语法检查：grunt-contrib-jshint
•Scss 编译：grunt-contrib-sass
•压缩文件：grunt-contrib-uglify
•监听文件变动：grunt-contrib-watch
•建立本地服务器：grunt-contrib-connect  

它们的命名和文档都很规范，因为这些是官方提供的比较常用的插件。
安装好相应的插件之后，就可以开始用了。  

**Gruntfile.js的书写**

最关键的就是 gruntfile.js的书写  

插件也装好了，开始写任务吧！既然是要程序来读取执行，必要要有一定的语法规范，下面来简单的说一下：  

首先要明白，这是一个 JS 文件，你可以写任意的 JS 代码，比如声明一个 对象 来存储一会要写任务的参数，或者是一个变量当作开关等等。
然后，所有的代码要包裹在
~~~
module.exports = function(grunt) {
    ...
};
~~~
里面。没有为什么。  

在这里面的代码，除去你自己写的乱七八糟的 JS，与 Grunt 有关的主要有三块代码：任务配置代码、插件加载代码、任务注册代码。  

任务配置代码就是调用插件配置要执行的任务和实现的功能，  
插件加载代码就是把需要用到的插件加载进来，  
任务注册代码就是注册一个 task，里面包含刚在前面编写的任务配置代码。  

分别看一下三部分代码：  

**任务配置代码：**
~~~
grunt.initConfig({
  pkg: grunt.file.readJSON('package.json'),
  uglify: {
    options: {
      banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
    },
    build: {
      src: 'src/<%= pkg.name %>.js',
      dest: 'build/<%= pkg.name %>.min.js'
    }
  }
});
~~~

可以看出，具体的任务配置代码以对象格式放在 grunt.initConfig 函数里面，其中先写了一句 pkg: grunt.file.readJSON('package.json') 功能是读取 package.json 文件，并把里面的信息获取出来，方便在后面任务中应用（例如下面就用了 <%= pkg.name %> 来输出项目名称），这样可以提高灵活性。 

之后就是 uglify 对象，这个名字是固定的，表示下面任务是调用 uglify 插件的，首先先配置了一些全局的 options 然后新建了一个 build 任务。  
也就是说，在 Uglify 插件下面，有一个 build 任务，内容是把 XX.js 压缩输出到 xx.min.js 里面。如果你需要更多压缩任务，也可以参照 build 多写几个任务。

至于怎么写出来 options 里面的参数和 build 里面的参数内容，这才是 grunt 学习的难点，你需要查看每个插件的用法，根据用法来编写任务，可以看下 grunt-contrib-uglify 的官方文档，往下面拉你就可以看到参数和使用方法了。  
这样，我们就新建了一个基于 uglify 的任务 build，功能是把 src/<%= pkg.name %>.js 压缩输出 build/<%= pkg.name %>.min.js。


**插件加载代码**  
这个就超级简单了，由于上面任务需要用到 grunt-contrib-uglify，当 grunt-contrib-uglify 安装到我们的项目之后，写下下面代码即可加载：

grunt.loadNpmTasks('grunt-contrib-uglify');

**任务注册代码**  
插件也加载了，任务也布置了，下面我们得注册一下任务，使用  
grunt.registerTask('default', ['uglify']);  
来注册一个任务。  

上面代码意思是，你在 default 上面注册了一个 Uglify 任务。  
default 就是别名，它是默认的 task，当你在项目目录执行 grunt 的时候，它会执行注册到 default 上面的任务。
也就是说，当我们执行 grunt 命令的时候，uglify 的所有代码将会执行。  

我们也可以注册别的 task，例如：  
grunt.registerTask('compress', ['uglify:build']);  

如果想要执行这个 task，我们就不能只输入 grunt 命令了，我们需要输入 grunt compress 命令来执行这条 task，而这条 task 的任务是 uglify 下面的 build 任务，也就是说，我们只会执行 uglify 里面 build 定义的任务，而不会执行 uglify 里面定义的其他任务。  

这里需要注意的是，task 的命名不能与后面的任务配置同名，也就是说这里的 compress 不能命名成 uglify，这样会报错或者产生意外情况。  

加上这三块代码，我们的示例 Gruntfile.js 就是这样子的：  

~~~
module.exports = function(grunt) {
  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // Load the plugin that provides the "uglify" task.
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Default task(s).
  grunt.registerTask('default', ['uglify']);

};
~~~

这就是官方那个示例，貌似 uglify 的参数好像不对。
