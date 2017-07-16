# yii2-webuploader
==========================

此扩展集成webuploader图片上传插件，旨在更好的处理 Yii2 framework 图片上传的前端问题，目前支持多图多字段多modal的混合上传。

## Demo 演示
![image](https://github.com/bailangzhan/demo/blob/master/images/yii2-webuploader.gif)

## 安装


推荐使用composer进行安装

```
$ php composer.phar require bailangzhan/yii2-webuploader dev-master
```

## 使用
params.php或者params-local.php内增加webuploader和domain配置项
```php
// 图片服务器的域名设置，拼接保存在数据库中的相对地址，可通过web进行展示
'domain' => 'http://blog.m/',
'webuploader' => [
	// 后端处理图片的地址，value 是相对的地址
	'uploadUrl' => 'blog/upload',
	// 多文件分隔符
	'delimiter' => ',',
	// 基本配置
	'baseConfig' => [
		'defaultImage' => 'http://img1.imgtn.bdimg.com/it/u=2056478505,162569476&fm=26&gp=0.jpg',
		'disableGlobalDnd' => true,
		'accept' => [
			'title' => 'Images',
			'extensions' => 'gif,jpg,jpeg,bmp,png',
			'mimeTypes' => 'image/*',
		],
		'pick' => [
			'multiple' => false,
		],
	],
],
```

视图文件

单图
```php
<?php 
// ActiveForm
echo $form->field($model, 'file')->widget('manks\FileInput', [
]); 

// 非 ActiveForm
echo '<label class="control-label">图片</label>';
echo \manks\FileInput::widget([
    'model' => $model,
    'attribute' => 'file',
]);
?>
```

多图
```php
<?php 
// ActiveForm
echo $form->field($model, 'file2')->widget('manks\FileInput', [
	'clientOptions' => [
		'pick' => [
			'multiple' => true,
		],
		// 'server' => Url::to('upload/u2'),
		// 'accept' => [
		// 	'extensions' => 'png',
		// ],
	],
]); ?>

// 非ActiveForm
echo '<label class="control-label">图片</label>';
echo \manks\FileInput::widget([
	'model' => $model,
	'attribute' => 'file',
	'clientOptions' => [
		'pick' => [
			'multiple' => true,
		],
	]
]); 
```

控制器
controller的地址可以在params.php或者params-local.php中配置 `Yii::$app->params['webuploader']['uploadUrl']`, 也可以在 clientOptions中配置 `server` 项。控制器需要返回的数据格式如下
```php
// 错误时
{"code": 1, "msg": "error"}

// 正确时， 其中 attachment 指的是保存在数据库中的路径，url 是该图片在web可访问的地址
{"code": 0, "url": "http://domain/图片地址", "attachment": "图片地址"}
```

## 注意
如果是修改的多图片操作，务必保证 $model->file = 'src1,src2,src3,...'; 或者 $model->file = ['src1', 'src2'. 'src3', ...];

## 许可

**yii2-webuploader** is released under the MIT License. See the bundled `LICENSE.md` for details.
