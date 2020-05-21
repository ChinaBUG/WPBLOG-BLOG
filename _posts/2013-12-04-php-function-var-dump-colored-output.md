---
ID: 2946
post_title: >
  PHP-function
  var_dump的语法加亮输出版
author: ChinaBUG
post_excerpt: |
  PHP的var_dump方法显示：关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。唯一不是很好地就是没有看起来很单调哈~对于调试也不是很方便就找出需要的数值，所以找了一下，还真有语法加亮版，摘抄了~保留
  源码来源于《Improved var_dump() with colored output》
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/php-function-var-dump-colored-output.html
published: true
post_date: 2013-12-04 11:22:21
---
PHP的var_dump方法显示：关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。唯一不是很好地就是没有看起来很单调哈~对于调试也不是很方便就找出需要的数值，所以找了一下，还真有语法加亮版，摘抄了~保留

源码来源于《<a href="http://codeaid.net/php/improved-var_dump%28%29-with-colored-output">Improved var_dump() with colored output</a>》
<pre>/*
Debug::dump($system);
*/
class Debug{
    public static function dumpMulti(){
        $args = func_get_args();
        foreach ($args as $arg) {
            self::dump($arg);
        }
    }
    public static function dump($variable, $caption = null){
        //if (APPLICATION_ENV !== 'development') {
        //    return;
        //}
        $html = '';
        ob_start();
        var_dump($variable);
        $output = ob_get_clean();
        $maps = array(
            'string'    =&gt; '/(string\((?P&lt;length&gt;\d+)\)) (?P&lt;value&gt;\"(?&lt;!\\\).*\")/i',
            'array'     =&gt; '/\[\"(?P&lt;key&gt;.+)\"(?:\:\"(?P&lt;class&gt;[a-z0-9_\\\]+)\")?(?:\:(?P&lt;scope&gt;public|protected|private))?\]=&gt;/Ui',
            'countable' =&gt; '/(?P&lt;type&gt;array|int|string)\((?P&lt;count&gt;\d+)\)/',
            'resource'  =&gt; '/resource\((?P&lt;count&gt;\d+)\) of type \((?P&lt;class&gt;[a-z0-9_\\\]+)\)/',
            'bool'      =&gt; '/bool\((?P&lt;value&gt;true|false)\)/',
            'float'     =&gt; '/float\((?P&lt;value&gt;[0-9\.]+)\)/',
            'object'    =&gt; '/object\((?P&lt;class&gt;[a-z_\\\]+)\)\#(?P&lt;id&gt;\d+) \((?P&lt;count&gt;\d+)\)/i',
        );
        foreach ($maps as $function =&gt; $pattern) {
            $output = preg_replace_callback($pattern, array('self', '_process' . ucfirst($function)), $output);
        }
        $header = '';
        if (!empty($caption)) {
            $header = '&lt;h2 style="' . self::_getHeaderCss() . '"&gt;' . $caption . '&lt;/h2&gt;';
        }
        print '&lt;pre style="' . self::_getContainerCss() . '"&gt;' . $header . $output . '&lt;/pre&gt;';
    }
    private static function _processString(array $matches){
        $matches['value'] = htmlspecialchars($matches['value']);
        return '&lt;span style="color: #0000FF;"&gt;string&lt;/span&gt;(&lt;span style="color: #1287DB;"&gt;' . $matches['length'] . ')&lt;/span&gt; &lt;span style="color: #6B6E6E;"&gt;' . $matches['value'] . '&lt;/span&gt;';
    }
    private static function _processArray(array $matches){
        $key = '&lt;span style="color: #008000;"&gt;"' . $matches['key'] . '"&lt;/span&gt;';
        $class = '';
        $scope = '';
        if (isset($matches['class']) &amp;&amp; !empty($matches['class'])) {
            $class = ':&lt;span style="color: #4D5D94;"&gt;"' . $matches['class'] . '"&lt;/span&gt;';
        }
        if (isset($matches['scope']) &amp;&amp; !empty($matches['scope'])) {
            $scope = ':&lt;span style="color: #666666;"&gt;' . $matches['scope'] . '&lt;/span&gt;';
        }
        return '[' . $key . $class . $scope . ']=&gt;';
    }
    private static function _processCountable(array $matches){
        $type = '&lt;span style="color: #0000FF;"&gt;' . $matches['type'] . '&lt;/span&gt;';
        $count = '(&lt;span style="color: #1287DB;"&gt;' . $matches['count'] . '&lt;/span&gt;)';
        return $type . $count;
    }
    private static function _processBool(array $matches){
        return '&lt;span style="color: #0000FF;"&gt;bool&lt;/span&gt;(&lt;span style="color: #0000FF;"&gt;' . $matches['value'] . '&lt;/span&gt;)';
    }
    private static function _processFloat(array $matches){
        return '&lt;span style="color: #0000FF;"&gt;float&lt;/span&gt;(&lt;span style="color: #1287DB;"&gt;' . $matches['value'] . '&lt;/span&gt;)';
    }
    private static function _processResource(array $matches){
        return '&lt;span style="color: #0000FF;"&gt;resource&lt;/span&gt;(&lt;span style="color: #1287DB;"&gt;' . $matches['count'] . '&lt;/span&gt;) of type (&lt;span style="color: #4D5D94;"&gt;' . $matches['class'] . '&lt;/span&gt;)';
    }
    private static function _processObject(array $matches){
        return '&lt;span style="color: #0000FF;"&gt;object&lt;/span&gt;(&lt;span style="color: #4D5D94;"&gt;' . $matches['class'] . '&lt;/span&gt;)#' . $matches['id'] . ' (&lt;span style="color: #1287DB;"&gt;' . $matches['count'] . '&lt;/span&gt;)';
    }

    private static function _getContainerCss(){
        return self::_arrayToCss(array(
            'background-color'      =&gt; '#d6ffef',
            'border'                =&gt; '1px solid #bbb',
            'border-radius'         =&gt; '4px',
            '-moz-border-radius'    =&gt; '4px',
            '-webkit-border-radius' =&gt; '4px',
            'font-size'             =&gt; '12px',
            'line-height'           =&gt; '1.4em',
            'margin'                =&gt; '30px',
            'padding'               =&gt; '7px',
        ));
    }
    private static function _getHeaderCss(){
        return self::_arrayToCss(array(
            'border-bottom' =&gt; '1px solid #bbb',
            'font-size'     =&gt; '18px',
            'font-weight'   =&gt; 'bold',
            'margin'        =&gt; '0 0 10px 0',
            'padding'       =&gt; '3px 0 10px 0',
        ));
    }
    private static function _arrayToCss(array $rules){
        $strings = array();
        foreach ($rules as $key =&gt; $value) {
            $strings[] = $key . ': ' . $value;
        }
        return join('; ', $strings);
    }
}</pre>
使用的时候直接就是Debug::dump($system);即可，当然也有多变量的，自己摸索一下吧，那个代码里面有。