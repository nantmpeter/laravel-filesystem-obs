<h1 align="center">laravel filesystem obs</h1>

<p align="center">
<a href="https://support.huaweicloud.com/sdk-php-devg-obs/obs_28_0001.html">Obs</a> storage for Laravel based on <a href="https://github.com/minz/huaweicloud-sdk-php-obs">minz/huaweicloud-sdk-php-obs</a>.
</p>

## 说明
原包部分功能无法使用，补充完整后自己需要使用传composer，原作者继续维护之后就合并过去

## 扩展包要求

-   PHP >= 7.0

## 安装命令

```shell
$ composer require "nantmpeter/laravel-filesystem-obs" -vvv
```

## 配置

1 将服务提供者 `Minz\Obs\ObsServiceProvider::class` 注册到 `config/app.php` 文件:

```php
'providers' => [
    // Other service providers...
    Minz\Obs\ObsServiceProvider::class,
],
```

> Laravel 5.5+ 会自动注册服务提供者可过滤

2 在 `config/filesystems.php` 配置文件中添加你的新驱动

```php
<?php

return [
   'disks' => [
        //...
        'obs' => [
                    'driver' => 'obs',
                    'accessKey' => env('OBS_ACCESS_ID'),
                    'secretKey' => env('OBS_ACCESS_KEY'),
                    'bucket' => env('OBS_DEFAULT_BUCKET'),
                    'endpoint' => env('OBS_DEFAULT_ENDPOINT'), //自定义外部域名比如cdn或者bucket默认endpoint
                    'isCName' => env('OBS_DEFAULT_CNAME', false), //isCName为true, getUrl会判断cdnDomain是否设定来决定返回的url，如果cdnDomain未设置，则使用endpoint来生成url，否则使用cdn
                    'sslVerify' => env('OBS_DEFAULT_SSL_VERIFY', true), // use https
                    'buckets' => [
                        'video' => [
                            'bucket' => env('OBS_VIDEO_BUCKET'),
                            'endpoint' => env('OBS_VIDEO_ENDPOINT'),
                            'isCName' => env('OBS_VIDEO_CNAME', false), 
                            'sslVerify' => env('OBS_VIDEO_SSL_VERIFY', true),
                        ]
                    ]
                ],
        //...
    ]
];
```

## 基本使用


```php
    $obs = Storage::disk('obs');
    //上传到默认bucket
    $defaultUploadTokenArray = $obs->uploadToken('objectname.txt');
    //切换到其他bucket
    $otherUploadTokenArray = $obs->bucket('video')->uploadToken('objectname2.txt');
    
    /**
     * 获取签名URL
     *
     * @param string $key
     * @param int $expires
     * @param string $method
     * @return string
     */
    $url = $obs->createSignedUrl($key, $expires = 3600, $method = "GET");
    
    /**
     * 写入文件
     * 
     * @param string $path
     * @param string $contents
     * @param Config $config
     * @return array|bool|false
     */    
    $writeStatus = $obs->write($key, $path);
```


## 依赖的核心包

-   [minz/huaweicloud-sdk-php-obs](https://github.com/minz/huaweicloud-sdk-php-obs)



## License

MIT