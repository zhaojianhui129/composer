# composer
composer使用实践


参考地址：http://docs.phpcomposer.com/

#####局部安装
要真正获取 Composer，我们需要做两件事。首先安装 Composer （同样的，这意味着它将下载到你的项目中）：
```sh
curl -sS https://getcomposer.org/installer | php
```
> 注意： 如果上述方法由于某些原因失败了，你还可以通过 php >下载安装器：

```sh
php -r "readfile('https://getcomposer.org/installer');" | php
```

这将检查一些 PHP 的设置，然后下载 composer.phar 到你的工作目录中。这是 Composer 的二进制文件。这是一个 PHAR 包（PHP 的归档），这是 PHP 的归档格式可以帮助用户在命令行中执行一些操作。

你可以通过 --install-dir 选项指定 Composer 的安装目录（它可以是一个绝对或相对路径）：
```sh
curl -sS https://getcomposer.org/installer | php -- --install-dir=bin
```

#####全局安装
你可以将此文件放在任何地方。如果你把它放在系统的 PATH 目录中，你就能在全局访问它。 在类Unix系统中，你甚至可以在使用时不加 php 前缀。
你可以执行这些命令让 composer 在你的系统中进行全局调用：
```sh
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

>注意： 如果上诉命令因为权限执行失败， 请使用 sudo 再次尝试运行 mv 那行命令。
现在只需要运行 composer 命令就可以使用 Composer 而不需要输入 php composer.phar。


###使用 Composer
现在我们将使用 Composer 来安装项目的依赖。如果在当前目录下没有一个 composer.json 文件，请查看[基本用法](http://docs.phpcomposer.com/01-basic-usage.html)章节。
要解决和下载依赖，请执行 install 命令：
```sh
php composer.phar installer
```
如果你进行了全局安装，并且没有 phar 文件在当前目录，请使用下面的命令代替：
```sh
composer install
```

###自动加载
除了库的下载，Composer 还准备了一个自动加载文件，它可以加载 Composer 下载的库中所有的类文件。使用它，你只需要将下面这行代码添加到你项目的引导文件中：
```sh
require 'vendor/autoload.php';
```

###composer.json：项目安装
####关于 require Key
第一件事情（并且往往只需要做这一件事），你需要在 composer.json 文件中指定 require key 的值。你只需要简单的告诉 Composer 你的项目需要依赖哪些包。
```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

###安装依赖包
取定义的依赖到你的本地项目，只需要调用 composer.phar 运行 install 命令。
```sh
php composer.phar installer
```

###composer.lock - 锁文件
在安装依赖后，Composer 将把安装时确切的版本号列表写入 composer.lock 文件。这将锁定改项目的特定版本。
#####请提交你应用程序的 composer.lock （包括 composer.json）到你的版本库中
这是非常重要的，因为 install 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 composer.json 文件中的定义）。
这意味着，任何人建立项目都将下载与指定版本完全相同的依赖。你的持续集成服务器、生产环境、你团队中的其他开发人员、每件事、每个人都使用相同的依赖，从而减轻潜在的错误对部署的影响。即使你独自开发项目，在六个月内重新安装项目时，你也可以放心的继续工作，即使从那时起你的依赖已经发布了许多新的版本。

如果不存在 composer.lock 文件，Composer 将读取 composer.json 并创建锁文件。

这意味着如果你的依赖更新了新的版本，你将不会获得任何更新。此时要更新你的依赖版本请使用 update 命令。这将获取最新匹配的版本（根据你的 composer.json 文件）并将新版本更新进锁文件。
```sh
php composer.phar update
```
如果只想安装或更新一个依赖，你可以白名单它们：
```sh
php composer.phar update monolog/monolog [...]
```
> 注意： 对于库，并不一定建议提交锁文件 请参考：[库的锁文件](http://docs.phpcomposer.com/02-libraries.html#Lock-file).

###自动加载
对于库的自动加载信息，Composer 生成了一个 vendor/autoload.php 文件。你可以简单的引入这个文件，你会得到一个免费的自动加载支持。
```sh
require 'vendor/autoload.php';
```