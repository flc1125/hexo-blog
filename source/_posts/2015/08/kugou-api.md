---
title: 酷狗音乐API，关键字搜索歌曲/获取音乐原始路径
date: 2015-08-27
updated: 2015-08-27
permalink: 187
categories:
    - PHP 
    - 分享
---

酷狗API，可根据关键字搜索歌曲及获取音乐原始路径

附根据该API，做的一个简单的音乐网站：http://music.flc.ren/

代码托管地址：https://code.csdn.net/flc1125/kugou-api

```php
<?php
 /**
 * 酷狗API类
 * 
 * @author Flc <2015-08-27 22:51:24>
 */
require_once 'http.class.php';

class Kugou
{
    const LIST_URL  = 'http://lib9.service.kugou.com/websearch/index.php?page={page}&keyword={keyword}&cmd=100&pagesize={pagesize}';

    const MUSIC_URL = 'http://m.kugou.com/app/i/getSongInfo.php?hash={hash}&cmd=playInfo';

    protected static $http = null;

    /**
     * 初始化
     */
    function __construct()
    {
        if (self::$http == null) {
            self::$http = new http();
        }
    }
    /**
     * 获取列表
     * 
     * @param  string  $keyword  关键字
     * @param  integer $page     当前页
     * @param  integer $pagesize 每页获取数量
     * @return array|false            
     */
    public function getList($keyword, $page = 1, $pagesize = 10)
    {
        $url  = str_replace(array('{page}', '{keyword}', '{pagesize}'), array($page, $keyword, $pagesize), self::LIST_URL);
        $json = self::$http->http_gets($url);

        if(!$json) return false;

        $resp = json_decode($json, true);

        return $resp;
    }
    /**
     * 获取音乐地址
     * @param  string $hash 音乐hash码
     * @return string|false       
     */
    public function getMusic($hash)
    {
        $url  = str_replace(array('{hash}'), array($hash), self::MUSIC_URL);
        $json = self::$http->http_gets($url);

        if(!$json) return false;

        $resp = json_decode($json, true);
        
        return $resp;
    }
}
?>
```