Yii2 adapt for sina app engine.这是针对yii2在新浪sae上运行做的适配
==================
This is using saestorage for asset publishing.使用saestorage实现资源发布功能

Installation安装步骤：
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).推荐使用composer

Either run命令行输入并运行

```
php composer.phar require "postor/yii2-for-sae" "*"
```

or add或者在配置文件composer.json中添加

```
"postor/yii2-for-sae": "*"
```

to the require section of your `composer.json` file.

Usage使用方法:
------
1.create a saestorage domain named 'assets' in SAE admin, this can be another name but need to be configed as assetDomain.

在sae后台开启storage，并建立一个domain叫做‘assets’，如果使用别的名字需要在配置中使用assetDomain来配置

2.config 配置

```php

// config/web.php
// change all file operations to db or seastorage or kvdb
//修改所有文件操作到数据库，或者saestorage或者kvdb
...
    'components' => [
        //cache to db缓存到db
        'cache' => [
            'class' => 'yii\caching\DbCache',
        ],
        //sea mysql配置sae的mysql
        'db' => [
            'class' => 'yii\db\Connection',
            'dsn' => 'mysql:host='.SAE_MYSQL_HOST_M.';port='.SAE_MYSQL_PORT.';dbname='.SAE_MYSQL_DB,
            'username' => SAE_MYSQL_USER,
            'password' => SAE_MYSQL_PASS,
            'charset' => 'utf8',
            'tablePrefix' => 'scd_',
        ],
        //use seastorage for assets使用sae发布资源
        'assetManager' =>[
        	'class'=>'postor\sae\SaeAssetManager',
        	'assetDomain'=>'assets',
        	'converter' => [
        		'class' => 'yii\web\AssetConverter',
        	],
        ],
        //log to db日志也修改到db
        'log' => [
            'traceLevel' => YII_DEBUG ? 3 : 0,
            'targets' => [
                [
                    'class' => 'yii\log\DbTarget',
                    'levels' => ['error', 'warning'],
                ],
            ],
        ],
    ]
```
demo: http://yii2postor.sinaapp.com/web/?r=admin
