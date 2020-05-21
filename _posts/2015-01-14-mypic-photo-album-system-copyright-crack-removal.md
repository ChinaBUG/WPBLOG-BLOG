---
ID: 3858
post_title: >
  mypic-图片相册系统的版权破解去除
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/01/mypic-photo-album-system-copyright-crack-removal.html
published: true
post_date: 2015-01-14 13:06:07
---
mypic这个系统作者还是挺厚道的，就是版权部分不是强制显示的，而是强制输出到页面内隐藏起来。

客户是不喜欢这些乱七八糟的废代码的，那么来去除吧。

先来分析一下思路吧。

一开始我以为那个版权内容是明文写着的，所以我使用搜索大法，全站代码搜索，发现这段代码的地方全部都是模板之类的或者是后台缓存，根本没有涉及到前台的版权逻辑部分。

呵呵，呵呵，作者还是挺厉害的嘛~没有这么容易让我盗用他的劳动成果噢。

既然没有相关的地方，我又想，我的模板文件没有这段内容，但是每次生成缓存都有，那么除了内核的控制外，一般都是在模板引擎里面做的判断增加的。

不管是内核还是模板引擎，都是需要查看core文件夹内的源码的，一开始懒得去看，想想就算了，后面一想，还是挑战一下吧，就打开core/~runtime.php一看哟，还是压缩后的文件，靠，这个样子绝对不利于分析，来吧，美化一下，得到下面的代码，太多了会影响阅读，就放在最下面的附录中，你可以点击<a href="#code001">GO</a>去看查看。<a id="code002"></a>

第一步肯定搜索关键词，结果你会发现不管怎么换关键词都是找不到内容的，所以，只能一个个的看下去。

经过用心的查看与分析，终于发现两个可疑的地方，在两个地方上使用了base64加密的字符串，反正也没有想法，解密看看呗，使用<a href="http://base64.xpcha.com/" target="_blank">Base64编码/解码器</a>解开下面的相应编码后就发现确实是我们需要的东西噢：

[php]IC0gUG93ZXJlZCBieSBNeVBpYw==[/php]

解开的内容是：
<blockquote>- Powered by MyPic</blockquote>

[php]PGRpdiBzdHlsZT0iZGlzcGxheTpub25lIj48YSBocmVmPSJodHRwOi8vd3d3LnBpY2Ntcy5jb20iIHRhcmdldD0iX2JsYW5rIiB0aXRsZT0iUG93ZXJlZCBieSBNeVBpYyI+UG93ZXJlZCBieSBNeVBpYzwvYT48YSBocmVmPSJodHRwOi8vd3d3LmRpcWl5ZS5jb20iIHRhcmdldD0iX2JsYW5rIiB0aXRsZT0iPue+juWlsyznvo7lpbPlm77niYcs576O5aWz5YaZ55yfLOaYjuaYn+WbvueJhyzpnZ7kuLvmtYHlm77niYcs6Z2e5Li75rWB576O5aWzIj7nvo7lpbMs576O5aWz5Zu+54mHLOe+juWls+WGmeecnyzmmI7mmJ/lm77niYcs6Z2e5Li75rWB5Zu+54mHLOmdnuS4u+a1gee+juWlszwvYT48L2Rpdj4=[/php]

解开的内容是：
<blockquote>&lt;div style="display: none;"&gt;&lt;a title="Powered by MyPic" href="http://www.piccms.com" target="_blank"&gt;Powered by MyPic&lt;/a&gt;&lt;a title="&amp;gt;美女,美女图片,美女写真,明星图片,非主流图片,非主流美女" href="http://www.diqiye.com" target="_blank"&gt;美女,美女图片,美女写真,明星图片,非主流图片,非主流美女&lt;/a&gt;&lt;/div&gt;</blockquote>
好了，找到版权内容，接着就是删除或者替换了呗，那么是删除好还是替换好呢？

我选择是替换，而不是删除，因为美化的代码在某些地方会出现错误，所以不能完全相信这些代码，更不能直接使用，特别是代码里面使用高级编程技巧的时候千万不要直接使用美化过的代码来使用，要不可能会出现错误的。

下面说说我的做法吧，我先用base64编码器编码一个空格，获得下面的代码IA==，然后用这4个字符代码上面的那两串代码就可以了。保存，版权消失了。

修改后的文件下载：<a href="http://files1.91zhaota.com/php/201501/cms-mypic-~runtime.zip">~runtime.php</a>

附录：

&lt;?php
!defined('IN_MP') &amp;&amp; die('Forbidden!');
abstract class base
{
protected $_config = array();
private function __set($name, $value)
{
if (property_exists($this, $name)) {
$this-&gt;{$name} = $value;
}
}
private function __get($name)
{
if (isset($this-&gt;{$name})) {
return $this-&gt;{$name};
} else {
return null;
}
}
private function __isset($name)
{
return isset($this-&gt;{$name});
}
private function __unset($name)
{
unset($this-&gt;{$name});
}
public function config($config = array())
{
$this-&gt;_config = array_merge($this-&gt;_config, $config);
return $this;
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class app extends base
{
public function __construct($module = 'action')
{
define(ACTION_DIR, $module);
}
public function run($module = null, $action = null)
{
try {
$this-&gt;init();
$this-&gt;parseUrl();
$this-&gt;exec($module, $action);
} catch (error $err) {
error($err);
}
}
public function init()
{
}
public function exec($module = '', $action = '')
{
getgpc(C('var_module')) &amp;&amp; ($module = getgpc(C('var_module')));
getgpc(C('var_action')) &amp;&amp; ($action = getgpc(C('var_action')));
!$module &amp;&amp; ($module = C('default_module'));
!$action &amp;&amp; ($action = C('default_action'));
include_cache(PAGE . ACTION_DIR . '/action.php');
$app = O($module, 'ACTION');
if (!$app) {
$app = O('empty', 'ACTION');
}
if (!$app) {
throwError(L('error.nomodule') . $module);
}
if (!method_exists($app, $action)) {
if (method_exists($app, '__empty')) {
$action = '__empty';
} else {
throwError(L('error.noaction') . $action);
}
}
$app-&gt;{$action}();
}
public function parseUrl()
{
if (getgpc(C('var_module')) || getgpc(C('var_action'))) {
return;
} elseif (C('url_mode')) {
$query = substr(getquery(), 1);
C('url_suffix') &amp;&amp; ($query = substr($query, 0, -strlen(C('url_suffix'))));
$query = explode(C('url_line'), $query);
$GLOBALS['_MP']['get'] = $query;
$_GET[C('var_module')] = array_shift($query);
$_GET[C('var_action')] = array_shift($query);
for ($i = 0; $i &lt; count($query); $i++) {
$_GET[$query[$i]] = addslashes(urldecode($query[++$i]));
}
$_REQUEST = array_merge($_POST, $_GET);
}
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class cache extends base
{
protected $db;
protected $_config = array('table' =&gt; '#@_cache');
public function __construct()
{
$this-&gt;db = getConn();
}
public function set($name, $value = '', $expire = -1)
{
$name = addslashes($name);
$value = serialize($value);
$data['name'] = $name;
$data['data'] = addslashes($value);
$data['expire'] = $expire == -1 ? -1 : now() + $expire;
$result = $this-&gt;db-&gt;replace($this-&gt;_config['table'], $data);
return $result ? true : false;
}
public function get($name)
{
$name = addslashes($name);
$now = now();
$sql = "SELECT * \r\n\t\t\t\tFROM {$this-&gt;_config['table']} \r\n\t\t\t\tWHERE name = '{$name}' \r\n\t\t\t\t\tAND (expire = -1 OR expire &gt; {$now})";
$data = $this-&gt;db-&gt;getOne($sql);
if ($data) {
return unserialize($data['data']);
} else {
return false;
}
}
public function del($name)
{
$name = addslashes($name);
return $this-&gt;db-&gt;delete($this-&gt;_config['table'], "`name`='{$name}'");
}
public function clear()
{
return $this-&gt;db-&gt;exec("truncate table `{$this-&gt;_config['table']}`");
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class dbmysql extends base
{
protected $_config = array('db_host' =&gt; 'localhost', 'db_user' =&gt; 'root', 'db_pwd' =&gt; 'root', 'db_name' =&gt; 'mp', 'db_lang' =&gt; 'utf8', 'db_prefix' =&gt; 'mp_');
protected $_sql = null;
protected $_conn = null;
protected $_rs = null;
public function __construct($config = array())
{
!extension_loaded('mysql') &amp;&amp; die('mysql模块不支持!');
$this-&gt;config($config);
}
public function config($config = array())
{
$this-&gt;_config = array_merge($this-&gt;_config, $config);
}
public function connect($host = '', $user = '', $pwd = '')
{
!$host &amp;&amp; ($host = $this-&gt;_config['db_host']);
!$user &amp;&amp; ($user = $this-&gt;_config['db_user']);
!$pwd &amp;&amp; ($pwd = $this-&gt;_config['db_pwd']);
$this-&gt;_conn = @mysql_connect($host, $user, $pwd) or die('数据库连接失败!');
!version_compare($this-&gt;version(), '4.1', '&lt;') or die('MySQL数据库版本低于4.1，请升级你的MySQL数据库！');
return $this;
}
public function select_db($db = '')
{
!($db = $this-&gt;_config['db_name']);
@mysql_select_db($db, $this-&gt;_conn) or die("{$db}:数据库尚未建立或者无法连接!");
mysql_query("SET NAMES '{$this-&gt;_config['db_lang']}'", $this-&gt;_conn);
return $this;
}
public function getone($sql)
{
$query = $this-&gt;query($sql);
return $this-&gt;fetch($query);
}
public function getall($sql)
{
$arr = array();
$query = $this-&gt;query($sql);
while ($val = $this-&gt;fetch($query)) {
$arr[] = $val;
}
return $arr;
}
public function insert($table, $data = array())
{
$cols = array();
$vals = array();
$one = reset($data);
if (is_array($one)) {
$cols = implode(',', $this-&gt;deal_field(array_keys($one)));
foreach ($data as $val) {
$vals[] = '(' . implode(',', $this-&gt;deal_value($val)) . ')';
}
$vals = implode(',', $vals);
} else {
$cols = implode(',', $this-&gt;deal_field(array_keys($data)));
$vals = '(' . implode(',', $this-&gt;deal_value($data)) . ')';
}
$sql = 'INSERT INTO ' . $this-&gt;deal_field($table) . " ( {$cols} ) VALUES {$vals}";
$this-&gt;exec($sql);
return $this-&gt;insert_id();
}
public function replace($table, $data = array())
{
$cols = array();
$vals = array();
$one = reset($data);
if (is_array($one)) {
$cols = implode(',', $this-&gt;deal_field(array_keys($one)));
foreach ($data as $val) {
$vals[] = '(' . implode(',', $this-&gt;deal_value($val)) . ')';
}
$vals = implode(',', $vals);
} else {
$cols = implode(',', $this-&gt;deal_field(array_keys($data)));
$vals = '(' . implode(',', $this-&gt;deal_value($data)) . ')';
}
$sql = 'REPLACE INTO ' . $this-&gt;deal_field($table) . " ( {$cols} ) VALUES {$vals}";
$this-&gt;exec($sql);
return $this-&gt;affected_rows();
}
public function update($table, $data, $where)
{
$set = array();
foreach ($data as $col =&gt; $val) {
$set[] = $this-&gt;deal_field($col) . ' = ' . $this-&gt;deal_value($val);
}
$set = implode(',', $set);
$where = $this-&gt;deal_where($where);
!empty($where) &amp;&amp; ($where = " WHERE {$where}");
$sql = 'UPDATE ' . $this-&gt;deal_field($table) . " SET {$set} {$where}";
$this-&gt;exec($sql);
return $this-&gt;affected_rows();
}
public function delete($table, $where)
{
$where = $this-&gt;deal_where($where);
!empty($where) &amp;&amp; ($where = " WHERE {$where}");
$sql = 'DELETE FROM ' . $this-&gt;deal_field($table) . " {$where}";
$this-&gt;exec($sql, $bind);
return $this-&gt;affected_rows();
}
public function query($sql)
{
return $this-&gt;execute($sql, 'mysql_query');
}
public function exec($sql)
{
return $this-&gt;execute($sql, 'mysql_unbuffered_query');
}
private function execute($sql, $func)
{
!$this-&gt;_conn &amp;&amp; $this-&gt;connect();
$this-&gt;_sql = $this-&gt;deal_prefix($sql);
if (C('debug_sql')) {
$t = microtime(true);
}
if (!($this-&gt;_rs = $func($this-&gt;_sql, $this-&gt;_conn))) {
throwError("SQL Query Error:&lt;br/&gt;SQL:{$this-&gt;_sql}&lt;br&gt;" . $this-&gt;error(), $this-&gt;errno());
}
if (C('debug_sql')) {
$sqltime = round(microtime(true) - $t, 3);
$explain = array();
if ($this-&gt;_rs &amp;&amp; preg_match('/^(select )/i', $this-&gt;_sql)) {
$explain = mysql_fetch_assoc(mysql_query('EXPLAIN ' . $this-&gt;_sql, $this-&gt;_conn));
}
$GLOBALS['_MP']['debug_sql'][] = array('sql' =&gt; $sql, 'time' =&gt; $sqltime, 'info' =&gt; $info, 'explain' =&gt; $explain);
}
return $this-&gt;_rs;
}
public function result($sql, $num = 0)
{
$query = $this-&gt;query($sql);
return mysql_result($query, $num);
}
public function fetch($query = null, $type = 1)
{
!$query &amp;&amp; ($query = $this-&gt;_rs);
if ($type == 0) {
return mysql_fetch_object($query);
} else {
return mysql_fetch_array($query, $type);
}
}
public function num_rows($query = null)
{
!$query &amp;&amp; ($query = $this-&gt;_rs);
return mysql_num_rows($query);
}
public function affected_rows($query = null)
{
return $this-&gt;_conn ? mysql_affected_rows($this-&gt;_conn) : mysql_affected_rows();
}
public function insert_id()
{
return ($I1 = mysql_insert_id($this-&gt;_conn)) &gt;= 0 ? $I1 : $this-&gt;result('SELECT last_insert_id();');
}
public function error()
{
return $this-&gt;_conn ? mysql_error($this-&gt;_conn) : mysql_error();
}
public function errno()
{
return intval($this-&gt;_conn ? mysql_errno($this-&gt;_conn) : mysql_errno());
}
public function version()
{
return mysql_get_server_info($this-&gt;_conn);
}
public function free()
{
if (is_resource($this-&gt;_rs)) {
return mysql_free_result($this-&gt;_rs);
}
}
public function close()
{
if (is_resource($this-&gt;_conn)) {
return mysql_close($this-&gt;_conn);
}
}
public function deal_prefix($sql = '')
{
if ($sql &amp;&amp; strpos($sql, '#@_') !== false) {
$sql = str_replace('#@_', $this-&gt;_config['db_prefix'], $sql);
}
return $sql;
}
public function deal_field($str = '')
{
if (is_array($str)) {
$str = array_map(array(__CLASS__, __METHOD__), $str);
return $str;
}
$str &amp;&amp; ($str = "`{$str}`");
return $str;
}
public function deal_value($str = '')
{
if (is_array($str)) {
$str = array_map(array(__CLASS__, __METHOD__), $str);
return $str;
}
$str = "'{$str}'";
return $str;
}
public function deal_where($where)
{
if (is_array($where)) {
if (array_key_exists('_logic', $where)) {
$logic = strtoupper($where['_logic']);
unset($where['_logic']);
} else {
$logic = 'AND';
}
foreach ($where as &amp;$term) {
$term = '(' . $term . ')';
}
$where = implode(' ' . $logic . ' ', $where);
}
return $where;
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class dbplus extends base
{
private $db = null;
private $db_name;
private $db_prefix;
public function __construct($db = null)
{
$this-&gt;db = $db ? $db : getconn();
}
public function dump_tables($dbname, $prefix, $file)
{
$tables = $this-&gt;list_table($dbname);
$fp = fopen($file, 'w');
foreach ($tables as $table) {
$do = strpos($table, $prefix);
if ($do !== false &amp;&amp; $do == 0) {
$sql = $this-&gt;dump_table($table, $fp);
fwrite($fp, $sql);
}
}
fclose($fp);
return true;
}
public function list_table($database)
{
$rs = mysql_list_tables($database);
$tables = array();
while ($row = mysql_fetch_row($rs)) {
$tables[] = $row[0];
}
mysql_free_result($rs);
return $tables;
}
public function dump_table($table, $fp)
{
fwrite($fp, "-- \n-- {$table}\n-- \n");
$rs = mysql_query("SELECT * FROM `{$table}`");
fwrite($fp, "TRUNCATE TABLE `{$table}`;\n");
while ($row = mysql_fetch_row($rs)) {
fwrite($fp, $this-&gt;get_insert_sql($table, $row));
}
mysql_free_result($rs);
fwrite($fp, '

');
}
public function get_insert_sql($table, $row)
{
$sql = "INSERT INTO `{$table}` VALUES (";
$values = array();
foreach ($row as $value) {
$values[] = '\'' . mysql_real_escape_string($value) . '\'';
}
$sql .= implode(', ', $values) . ');
';
return $sql;
}
public function batQuery($sql)
{
$sql = str_replace('
', '
', $sql);
$ret = array();
$num = 0;
foreach (explode(';
', trim($sql)) as $query) {
$queries = explode('
', trim($query));
foreach ($queries as $query) {
$ret[$num] .= $query[0] == '#' || $query[0] . $query[1] == '--' ? '' : $query;
}
$num++;
}
unset($sql);
foreach ($ret as $query) {
$query = trim($query);
if ($query) {
$this-&gt;db-&gt;query($query);
}
}
return true;
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class error extends Exception
{
public function __construct($message, $code = 0)
{
parent::__construct($message, $code);
}
public function __toString()
{
return __CLASS__;
}
public function getError()
{
$trace = $this-&gt;getTrace();
$this-&gt;file = isset($trace[0]['file']) ? $trace[0]['file'] : null;
$this-&gt;line = isset($trace[0]['line']) ? $trace[0]['line'] : null;
$traceInfo = '';
$time = date('y-m-d H:i:m', time());
foreach ($trace as $t) {
$_file = isset($t['file']) ? $t['file'] : null;
$_line = isset($t['line']) ? $t['line'] : null;
$_class = isset($t['class']) ? $t['class'] : null;
$_type = isset($t['type']) ? $t['type'] : null;
$_args = isset($t['args']) ? $t['args'] : null;
$_function = isset($t['function']) ? $t['function'] : null;
$traceInfo .= '[' . $time . '] ' . $_file . ' (' . $_line . ') ';
$traceInfo .= $_class . $_type . $_function . '(';
if (!empty($_args)) {
$traceInfo .= implode(', ', $_args);
}
$traceInfo .= ')
';
}
$error['message'] = $this-&gt;message;
$error['file'] = $this-&gt;file;
$error['line'] = $this-&gt;line;
$error['trace'] = $traceInfo;
return $error;
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class image extends base
{
protected $img;
protected $types = array(1 =&gt; 'gif', 2 =&gt; 'jpg', 3 =&gt; 'png', 6 =&gt; 'bmp');
public $_config = array('do_gif' =&gt; 1);
public function __construct($img = '')
{
!$img &amp;&amp; $this-&gt;setimg($img);
}
public function setimg($img)
{
$this-&gt;img = $img;
return $this;
}
public function getImageInfo($img)
{
$info = @getimagesize($img);
if (isset($this-&gt;types[$info[2]])) {
$info['ext'] = $info['type'] = $this-&gt;types[$info[2]];
} else {
$info['ext'] = $info['type'] = 'jpg';
}
$info['type'] == 'jpg' &amp;&amp; ($info['type'] = 'jpeg');
$info['size'] = @filesize($img);
return $info;
}
public function thumb($filename, $new_w = 160, $new_h = 120, $cut = 0, $big = 0, $pct = 100)
{
$info = $this-&gt;getImageInfo($this-&gt;img);
if (!empty($info[0])) {
$old_w = $info[0];
$old_h = $info[1];
$type = $info['type'];
$ext = $info['ext'];
unset($info);
if ($ext == 'gif' &amp;&amp; !$this-&gt;_config['do_gif']) {
return false;
}
if ($old_w &lt; $new_w &amp;&amp; $old_h &lt; $new_h &amp;&amp; !$big) {
return false;
}
if ($cut == 0) {
$scale = min($new_w / $old_w, $new_h / $old_h);
$width = (int) ($old_w * $scale);
$height = (int) ($old_h * $scale);
$start_w = $start_h = 0;
$end_w = $old_w;
$end_h = $old_h;
} elseif ($cut == 1) {
$scale1 = round($new_w / $new_h, 2);
$scale2 = round($old_w / $old_h, 2);
if ($scale1 &gt; $scale2) {
$end_h = round($old_w / $scale1, 2);
$start_h = ($old_h - $end_h) / 2;
$start_w = 0;
$end_w = $old_w;
} else {
$end_w = round($old_h * $scale1, 2);
$start_w = ($old_w - $end_w) / 2;
$start_h = 0;
$end_h = $old_h;
}
$width = $new_w;
$height = $new_h;
} elseif ($cut == 2) {
$scale1 = round($new_w / $new_h, 2);
$scale2 = round($old_w / $old_h, 2);
if ($scale1 &gt; $scale2) {
$end_h = round($old_w / $scale1, 2);
$end_w = $old_w;
} else {
$end_w = round($old_h * $scale1, 2);
$end_h = $old_h;
}
$start_w = 0;
$start_h = 0;
$width = $new_w;
$height = $new_h;
}
$createFun = 'ImageCreateFrom' . $type;
$oldimg = $createFun($this-&gt;img);
if ($type != 'gif' &amp;&amp; function_exists('imagecreatetruecolor')) {
$newimg = imagecreatetruecolor($width, $height);
} else {
$newimg = imagecreate($width, $height);
}
if (function_exists('ImageCopyResampled')) {
ImageCopyResampled($newimg, $oldimg, 0, 0, $start_w, $start_h, $width, $height, $end_w, $end_h);
} else {
ImageCopyResized($newimg, $oldimg, 0, 0, $start_w, $start_h, $width, $height, $end_w, $end_h);
}
if (!is_dir(dirname($filename))) {
mkDirs(dirname($filename));
}
$type == 'jpeg' &amp;&amp; imageinterlace($newimg, 1);
$imageFun = 'image' . $type;
if ($type == 'jpeg') {
$did = @$imageFun($newimg, $filename, $pct);
} else {
$did = @$imageFun($newimg, $filename);
}
!$did &amp;&amp; die('保存失败!检查目录是否存在并且可写?');
ImageDestroy($newimg);
ImageDestroy($oldimg);
return $filename;
}
return false;
}
public function water($filename, $water, $pos = 0, $pct = 80)
{
$info = $this-&gt;getImageInfo($water);
if (!empty($info[0])) {
$water_w = $info[0];
$water_h = $info[1];
$type = $info['type'];
$fun = 'imagecreatefrom' . $type;
$waterimg = $fun($water);
} else {
return false;
}
$info = $this-&gt;getImageInfo($this-&gt;img);
if (!empty($info[0])) {
$old_w = $info[0];
$old_h = $info[1];
$type = $info['type'];
$ext = $info['ext'];
$fun = 'imagecreatefrom' . $type;
$oldimg = $fun($this-&gt;img);
} else {
return false;
}
if ($ext == 'gif' &amp;&amp; !$this-&gt;_config['do_gif']) {
return false;
}
$water_w &gt; $old_w &amp;&amp; ($water_w = $old_w);
$water_h &gt; $old_h &amp;&amp; ($water_h = $old_h);
switch ($pos) {
case 0:
$posX = rand(0, $old_w - $water_w);
$posY = rand(0, $old_h - $water_h);
break;
case 1:
$posX = 0;
$posY = 0;
break;
case 2:
$posX = ($old_w - $water_w) / 2;
$posY = 0;
break;
case 3:
$posX = $old_w - $water_w;
$posY = 0;
break;
case 4:
$posX = 0;
$posY = ($old_h - $water_h) / 2;
break;
case 5:
$posX = ($old_w - $water_w) / 2;
$posY = ($old_h - $water_h) / 2;
break;
case 6:
$posX = $old_w - $water_w;
$posY = ($old_h - $water_h) / 2;
break;
case 7:
$posX = 0;
$posY = $old_h - $water_h;
break;
case 8:
$posX = ($old_w - $water_w) / 2;
$posY = $old_h - $water_h;
break;
case 9:
$posX = $old_w - $water_w;
$posY = $old_h - $water_h;
break;
default:
$posX = rand(0, $old_w - $water_w);
$posY = rand(0, $old_h - $water_h);
break;
}
imagealphablending($oldimg, true);
imagecopymerge($oldimg, $waterimg, $posX, $posY, 0, 0, $water_w, $water_h, $pct);
if (!is_dir(dirname($filename))) {
mkDirs(dirname($filename));
}
$fun = 'image' . $type;
if ($type == 'jpeg') {
$did = @$fun($oldimg, $filename, $pct);
} else {
$did = @$fun($oldimg, $filename);
}
!$did &amp;&amp; die('保存失败!检查目录是否存在并且可写?');
imagedestroy($oldimg);
imagedestroy($waterimg);
return $filename;
}
public static function vCode($vname = 'vcode', $num = 4, $size = 15, $font = './core/tpl/font.ttf', $width = 0, $height = 0)
{
!$width &amp;&amp; ($width = $num * $size * 4 / 5 + 5);
!$height &amp;&amp; ($height = $size + 10);
$code = getsalt($num);
session::set($vname, $code);
$im = imagecreatetruecolor($width, $height);
$back_color = imagecolorallocate($im, 235, 236, 237);
$boer_color = imagecolorallocate($im, 118, 151, 199);
$text_color = imagecolorallocate($im, mt_rand(0, 200), mt_rand(0, 200), mt_rand(0, 200));
imagefilledrectangle($im, 0, 0, $width, $height, $back_color);
imagerectangle($im, 0, 0, $width - 1, $height - 1, $boer_color);
imagefttext($im, $size, 0, 5, $size + 3, $text_color, $font, $code);
for ($i = 0; $i &lt; 10; $i++) {
$font_color = imagecolorallocate($im, mt_rand(0, 255), mt_rand(0, 255), mt_rand(0, 255));
imagearc($im, mt_rand(-$width, $width), mt_rand(-$height, $height), mt_rand(30, $width * 2), mt_rand(20, $height * 2), mt_rand(0, 360), mt_rand(0, 360), $font_color);
}
for ($i = 0; $i &lt; 50; $i++) {
$font_color = imagecolorallocate($im, mt_rand(0, 255), mt_rand(0, 255), mt_rand(0, 255));
imagesetpixel($im, mt_rand(0, $width), mt_rand(0, $height), $font_color);
}
header('Cache-Control: max-age=1, s-maxage=1, no-cache, must-revalidate');
header('Content-type: image/png');
imagepng($im);
imagedestroy($im);
}
}
class model extends base
{
protected $db;
protected $_options = array();
protected $_table = array();
protected $_data = array();
protected $validate = array();
protected $mp;
public function __construct()
{
}
public function initModel()
{
$this-&gt;db = getConn();
$this-&gt;mp =&amp; $GLOBALS['_MP'];
}
public function getvo()
{
return $this-&gt;_data;
}
public function __set($name, $value)
{
$this-&gt;_data[$name] = $value;
}
public function __get($name)
{
if (isset($this-&gt;_data[$name])) {
return $this-&gt;_data[$name];
} elseif (property_exists($this, $name)) {
return $this-&gt;{$name};
} else {
return null;
}
}
public function find()
{
$sql = $this-&gt;parse_select();
$arr = $this-&gt;db-&gt;getone($sql);
return $arr;
}
public function findAll()
{
$sql = $this-&gt;parse_select();
$arr = $this-&gt;db-&gt;getall($sql);
return $arr;
}
public function result($num = 0)
{
$sql = $this-&gt;parse_select();
$arr = $this-&gt;db-&gt;result($sql, $num);
return $arr;
}
public function save($data = null)
{
$table = $this-&gt;parse_table();
$where = $this-&gt;parse_where();
if (!empty($where)) {
return $this-&gt;db-&gt;update($table, $data, $where);
} else {
return $this-&gt;db-&gt;insert($table, $data);
}
}
public function del()
{
$table = $this-&gt;parse_table();
$where = $this-&gt;parse_where();
if (!$where) {
throwError(L('error.nowhere'));
}
return $this-&gt;db-&gt;delete($table, $where);
}
public function parse_select()
{
if (!empty($this-&gt;_options['sql'])) {
$sql = $this-&gt;_options['sql'];
unset($this-&gt;_options['sql']);
} else {
$field = '*';
if (!empty($this-&gt;_options['field'])) {
if (is_array($this-&gt;_options['field'])) {
$field = implode(',', $this-&gt;db-&gt;deal_field($this-&gt;_options['field']));
} else {
$field = $this-&gt;_options['field'];
}
unset($this-&gt;_options['field']);
}
$table = $this-&gt;parse_table();
$where = $this-&gt;parse_where();
!empty($where) &amp;&amp; ($where = ' WHERE ' . $where);
$order = '';
if (!empty($this-&gt;_options['order'])) {
$order = ' ORDER BY ' . $this-&gt;_options['order'];
unset($this-&gt;_options['order']);
}
$limit = '';
if (!empty($this-&gt;_options['limit'])) {
$limit = ' LIMIT ' . $this-&gt;_options['limit'];
unset($this-&gt;_options['limit']);
}
$sql = "SELECT {$field} FROM {$table}{$where}{$order}{$limit}";
}
return $this-&gt;deal_table($sql);
}
protected function parse_where()
{
$where = '';
if (!empty($this-&gt;_options['where'])) {
if (is_array($this-&gt;_options['where'])) {
$where = $this-&gt;db-&gt;deal_where($this-&gt;_options['where']);
} else {
$where = $this-&gt;_options['where'];
}
unset($this-&gt;_options['where']);
}
return $where;
}
protected function parse_table()
{
$table = $this-&gt;_table[1];
if (!empty($this-&gt;_options['table'])) {
if (is_numeric($this-&gt;_options['table'])) {
$table = $this-&gt;_table[$this-&gt;_options['table']];
} else {
$table = $this-&gt;_options['table'];
}
unset($this-&gt;_options['table']);
}
return $table;
}
protected function deal_table($sql)
{
return preg_replace_callback('/#(\\d+)/', array(__CLASS__, 'deal_table_callback'), $sql);
}
protected function deal_table_callback($tab)
{
return $this-&gt;_table[$tab[1]];
}
public function sql($sql)
{
$this-&gt;_options['sql'] = $sql;
return $this;
}
public function where($where)
{
$this-&gt;_options['where'] = $where;
return $this;
}
public function order($order)
{
$this-&gt;_options['order'] = $order;
return $this;
}
public function table($table)
{
$this-&gt;_options['table'] = $table;
return $this;
}
public function field($field)
{
$this-&gt;_options['field'] = $field;
return $this;
}
public function limit($limit)
{
$this-&gt;_options['limit'] = $limit;
return $this;
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class session extends base
{
static function start()
{
session_start();
}
static function pause()
{
session_write_close();
}
static function clear()
{
unset($_SESSION);
session_destroy();
}
static function name($name = null)
{
$name &amp;&amp; ($name = C('cookie_prefix') . $name);
return isset($name) ? session_name($name) : session_name();
}
static function id($id = null)
{
return isset($id) ? session_id($id) : session_id();
}
static function path($path = null)
{
return !empty($path) ? session_save_path($path) : session_save_path();
}
static function get($name)
{
return isset($_SESSION[C('cookie_prefix')][$name]) ? $_SESSION[C('cookie_prefix')][$name] : null;
}
static function set($name, $value)
{
if (null === $value) {
unset($_SESSION[C('cookie_prefix')][$name]);
} else {
$_SESSION[C('cookie_prefix')][$name] = $value;
}
return;
}
static function is_set($name)
{
return isset($_SESSION[C('cookie_prefix')][$name]);
}
static function del($name)
{
unset($_SESSION[C('cookie_prefix')][$name]);
}
static function setTime($time = 3600, $out = 0)
{
$time = $out ? now() - $time : now() + $time;
setcookie(session_name(), session_id(), $time, '/');
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class cookie extends base
{
static function is_set($name)
{
return isset($_COOKIE[C('cookie_prefix')][$name]);
}
static function get($name)
{
return isset($_COOKIE[C('cookie_prefix')][$name]) ? $_COOKIE[C('cookie_prefix')][$name] : null;
}
static function set($name, $value, $expire = 0, $path = '/', $domain = '')
{
!$domain &amp;&amp; ($domain = C('cookie_domain'));
$expire &amp;&amp; ($expire += now());
setcookie(C('cookie_prefix') . '[' . $name . ']', $value, $expire, $path, $domain);
}
static function del($name, $path = '/', $domain = '')
{
!$domain &amp;&amp; ($domain = C('cookie_domain'));
self::set($name, '', now() - 3600, $path, $domain);
}
}
class template extends base
{
protected $file = '';
protected $html = '';
protected $temp = '';
protected $_config = array('cache' =&gt; true);
public function __construct()
{
}
public function html($html, $temp)
{
$this-&gt;html = $html;
$this-&gt;temp = $temp;
return $this;
}
public function file($file, $temp)
{
if (is_file($file)) {
$this-&gt;file = $file;
$this-&gt;html = read_file($file);
} else {
throwerror("{$file}: 文件不存在,或者无法正常读取");
}
$this-&gt;temp = $temp;
return $this;
}
public function parse()
{
if ($this-&gt;_config['cache'] &amp;&amp; $this-&gt;temp) {
if (is_file($this-&gt;temp) &amp;&amp; filemtime($this-&gt;temp) &gt; filemtime($this-&gt;file)) {
return $this-&gt;temp;
}
}
$this-&gt;html = $this-&gt;parseLable($this-&gt;html);
write_file($this-&gt;temp, $this-&gt;html);
return $this-&gt;temp;
}
public function parseLable($html = '')
{
$a1 = array('/\\.\\.\\/\\.\\.\\/images/is', '/\\{\\s*\\$([\\w\'\\"\\[\\]\\$\\-\\&gt;\\x7f-\\xff]+)\\s*\\}/', '/\\{\\s*#([^-][^}]+?)\\s*#\\}/', '/&lt;!--#\\s*if\\s*(.+?)\\s*#--&gt;/', '/&lt;!--#\\s*else\\s*#--&gt;/', '/&lt;!--#\\s*elseif\\s*(.+?)\\s*#--&gt;/', '/&lt;!--#\\s*loop\\s+(\\S+)\\s+(\\S+)\\s*#--&gt;/', '/&lt;!--#\\s*loop\\s+(\\S+)\\s+(\\S+)\\s+(\\S+)\\s*#--&gt;/', '/&lt;!--#\\s*end\\s*#--&gt;/', '/&lt;!--#(.+?)#--&gt;/is', '/&lt;\\s*\\/body\\s*&gt;/i', '/(&lt;\\s*\\/title\\s*&gt;)/i');
$a2 = array(C('app_dir') . 'images', '&lt;?php echo $\\1?&gt;', '&lt;?php echo \\1?&gt;', '&lt;?php if(\\1){?&gt;', '&lt;?php } else{?&gt;', '&lt;?php } elseif(\\1){?&gt;', '&lt;?php foreach(\\1 as \\2){?&gt;', '&lt;?php foreach(\\1 as \\2 =&gt; \\3){?&gt;', '&lt;?php }?&gt;', '&lt;?php
\\1
?&gt;', self::buzhidao(), self::zhenbuzhidao() . '\\1');
$html = preg_replace($a1, $a2, $html);
$l = '/((\\$[\\w\\x7f-\\xff]*)(\\[[\\w\\-\\.\\"\'\\[\\]\\$\\x7f-\\xff]+\\])+?)/s';
$html = preg_replace_callback($l, array(__CLASS__, 'addquote'), $html);
$html = '&lt;?php !defined(\'IN_MP\') &amp;&amp; die(\'Forbidden!\');?&gt;
' . $html;
return $html;
}
public function addquote($var)
{
$var = $var[0];
return preg_replace('/\\[([\\w\\-\\.-�]+)\\]/s', '[\'\\1\']', $var);
}
static function zhenbuzhidao()
{
$zhenbuzhidao = 'IA==';
return base64_decode($zhenbuzhidao);
}
static function buzhidao()
{
$buzhidao = 'IA==';
return base64_decode($buzhidao);
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class upload extends base
{
protected $error = 0;
public $_config = array('upfile_path' =&gt; './up_files', 'upfile_exts' =&gt; 'png,gif,jpg,jpeg', 'upfile_style' =&gt; 'Y-m', 'max_size' =&gt; 2048000);
public function __construct($config = array())
{
$config &amp;&amp; $this-&gt;config($config);
}
public function save($name, $filename = null)
{
if (!isset($_FILES[$name])) {
return false;
}
$Info = $_FILES[$name];
$Info['ext'] = $this-&gt;getExt($Info['name']);
if ((int) $Info['error'] != 0) {
$this-&gt;Error($Info['error']);
return false;
}
$Info['isimage'] = substr($Info['type'], 0, 5) == 'image' ? 1 : 0;
!$filename &amp;&amp; ($filename = now() . getSalt(4));
$path = date($this-&gt;_config['upfile_style'], now());
$Info['file'] = "{$path}/{$filename}.{$Info['ext']}";
$Info['path'] = $this-&gt;_config['upfile_path'] . '/' . $Info['file'];
if (is_uploaded_file($Info['tmp_name'])) {
if (!in_str($Info['ext'], $this-&gt;_config['upfile_exts'])) {
$this-&gt;error(8);
return false;
}
if ($this-&gt;_config['max_size'] != 0 &amp;&amp; $Info['size'] &gt; $this-&gt;_config['max_size']) {
$this-&gt;error(9);
return false;
}
if (!is_dir(dirname($Info['path']))) {
mkDirs(dirname($Info['path']));
}
if (!move_uploaded_file($Info['tmp_name'], $Info['path'])) {
return false;
}
unset($Info['tmp_name'], $Info['error']);
$Info['size'] = filesize($Info['path']);
return $Info;
} else {
$this-&gt;error();
}
return false;
}
public function saves($l1 = '')
{
if (!empty($l1)) {
$this-&gt;dealFiles($_FILES[$l1]);
}
$l2 = $_FILES;
$I1 = array();
foreach ($l2 as $key =&gt; $file) {
$file = $this-&gt;save($key);
if (!empty($file)) {
$I1[$key] = $file;
} else {
$I1[$key] = $this-&gt;getError();
}
}
return $I1;
}
private function dealFiles(&amp;$file)
{
if (!is_array($file['name'])) {
return;
}
$num = count($file['name']);
for ($i = 0; $i &lt; $num; $i++) {
foreach ($file as $k =&gt; $v) {
$arr[$i][$k] = $file[$k][$i];
}
}
$_FILES = $arr;
}
public function Error($l1 = '')
{
switch ($l1) {
case 1:
$this-&gt;error = '上传的文件超过了 php.ini 中 upload_max_filesize 选项限制的值';
break;
case 2:
$this-&gt;error = '上传文件的大小超过了 HTML 表单中 MAX_FILE_SIZE 选项指定的值';
break;
case 3:
$this-&gt;error = '文件只有部分被上传';
break;
case 4:
$this-&gt;error = '没有文件被上传';
break;
case 6:
$this-&gt;error = '找不到临时文件夹';
break;
case 7:
$this-&gt;error = '文件写入失败';
break;
case 8:
$this-&gt;error = '不允许的文件类型';
break;
case 9:
$this-&gt;error = '文件大小超过限制';
break;
default:
$this-&gt;error = '未知上传错误！';
}
return;
}
private function getExt($l1)
{
$I1 = pathinfo($l1);
return strtolower($I1['extension']);
}
public function getError()
{
return $this-&gt;error;
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class download extends base
{
const ERROR_CONNECT = 600;
const SEND_USER_AGENT = 'Mypic::DownLoad';
public $url, $method, $timeout;
private $host, $port, $path, $query, $referer;
private $header;
private $body;
public function __construct($url = null, $method = 'GET', $timeout = 60)
{
@set_time_limit(0);
if (!empty($url)) {
$this-&gt;connect($url, $method, $timeout);
}
return $this;
}
public function connect($url = null, $method = 'GET', $timeout = 60)
{
$this-&gt;header = null;
$this-&gt;body = null;
$this-&gt;url = $url;
$this-&gt;method = strtoupper(empty($method) ? 'GET' : $method);
$this-&gt;timeout = empty($timeout) ? 30 : $timeout;
if (!empty($url)) {
$this-&gt;parseURL($url);
}
return $this;
}
public function send($params = array())
{
$header = null;
$body = null;
$QueryStr = null;
if (function_exists('fsockopen')) {
$fp = @fsockopen($this-&gt;host, $this-&gt;port, $errno, $errstr, $this-&gt;timeout);
if (!$fp) {
return false;
}
$SendStr = "{$this-&gt;method} {$this-&gt;path}{$this-&gt;query} HTTP/1.0\r\n";
$SendStr .= "Host:{$this-&gt;host}:{$this-&gt;port}\r\n";
$SendStr .= 'Accept: */*
';
$SendStr .= "Referer:{$this-&gt;referer}\r\n";
$SendStr .= 'User-Agent: ' . self::SEND_USER_AGENT . '
';
$SendStr .= 'Pragma: no-cache
';
$SendStr .= 'Cache-Control: no-cache
';
if ($this-&gt;method == 'POST') {
if (is_array($params)) {
$QueryStr = http_build_query($params);
} else {
$QueryStr = $params;
}
$length = strlen($QueryStr);
$SendStr .= 'Content-Type: application/x-www-form-urlencoded
';
$SendStr .= "Content-Length: {$length}\r\n";
}
$SendStr .= 'Connection: Close

';
if (strlen($QueryStr) &gt; 0) {
$SendStr .= $QueryStr . '
';
}
fputs($fp, $SendStr);
do {
$header .= fread($fp, 1);
} while (!preg_match('/

$/', $header));
if ($this-&gt;redirect($header)) {
return true;
}
while (!feof($fp)) {
$body .= fread($fp, 4096);
}
fclose($fp);
} elseif (function_exists('curl_exec')) {
$ch = curl_init($this-&gt;url);
curl_setopt_array($ch, array(CURLOPT_TIMEOUT =&gt; $this-&gt;timeout, CURLOPT_HEADER =&gt; true, CURLOPT_RETURNTRANSFER =&gt; true, CURLOPT_USERAGENT =&gt; self::SEND_USER_AGENT, CURLOPT_REFERER =&gt; $this-&gt;referer));
if ($this-&gt;method == 'GET') {
curl_setopt($ch, CURLOPT_HTTPGET, true);
} else {
if (is_array($params)) {
$QueryStr = http_build_query($params);
} else {
$QueryStr = $params;
}
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $QueryStr);
}
$fp = curl_exec($ch);
curl_close($ch);
if (!$fp) {
return false;
}
$i = 0;
$length = strlen($fp);
do {
$header .= substr($fp, $i, 1);
$i++;
} while (!preg_match('/

$/', $header));
if ($this-&gt;redirect($header)) {
return true;
}
do {
$body .= substr($fp, $i, 4096);
$i = $i + 4096;
} while ($length &gt;= $i);
unset($fp, $length, $i);
}
$this-&gt;header = $header;
$this-&gt;body = $body;
return true;
}
private function redirect($header)
{
if (in_array($this-&gt;status($header), array(301, 302))) {
if (preg_match('/Location\\:(.+)
/i', $header, $regs)) {
$this-&gt;connect(trim($regs[1]), $this-&gt;method, $this-&gt;timeout);
$this-&gt;send();
return true;
}
} else {
return false;
}
}
public function header()
{
return $this-&gt;header;
}
public function body()
{
return $this-&gt;body;
}
public function status($header = null)
{
if (empty($header)) {
$header = $this-&gt;header;
}
if (preg_match('/(.+) (\\d+) (.+)
/i', $header, $status)) {
return $status[2];
} else {
return self::ERROR_CONNECT;
}
}
private function parseURL($url)
{
$aUrl = parse_url($url);
!isset($aUrl['query']) &amp;&amp; ($aUrl['query'] = null);
$this-&gt;host = $aUrl['host'];
$this-&gt;port = empty($aUrl['port']) ? 80 : (int) $aUrl['host'];
$this-&gt;path = empty($aUrl['path']) ? '/' : (string) $aUrl['path'];
$this-&gt;query = strlen($aUrl['query']) &gt; 0 ? '?' . $aUrl['query'] : null;
$this-&gt;referer = 'http://' . $aUrl['host'];
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class valid
{
static $regex = array('require' =&gt; '/.+/', 'email' =&gt; '/^\\w+([-+.]\\w+)*@\\w+([-.]\\w+)*\\.\\w+([-.]\\w+)*$/', 'phone' =&gt; '/^((\\(\\d{2,3}\\))|(\\d{3}\\-))?(\\(0\\d{2,3}\\)|0\\d{2,3}-)?[1-9]\\d{6,7}(\\-\\d{1,4})?$/', 'mobile' =&gt; '/^((\\(\\d{2,3}\\))|(\\d{3}\\-))?(13|15)\\d{9}$/', 'url' =&gt; '/^http:\\/\\/[A-Za-z0-9]+\\.[A-Za-z0-9]+[\\/=\\?%\\-&amp;_~`@[\\]\':+!]*([^&lt;&gt;\\"\\"])*$/', 'img' =&gt; '^(http|https|ftp):(\\/\\/|\\\\)(([\\w\\/\\\\+\\-~`@:%])+\\.)+([\\w\\/\\\\.\\=\\?\\+\\-~`@\':!%#]|(&amp;)|&amp;)+\\.(jpg|bmp|gif|png)$', 'currency' =&gt; '/^\\d+(\\.\\d+)?$/', 'number' =&gt; '/\\d+$/', 'zip' =&gt; '/^[1-9]\\d{5}$/', 'qq' =&gt; '/^[1-9]\\d{4,12}$/', 'int' =&gt; '/^[-\\+]?\\d+$/', 'double' =&gt; '/^[-\\+]?\\d+(\\.\\d+)?$/', 'english' =&gt; '/^[A-Za-z]+$/');
static function check($value, $checkName)
{
$matchRegex = self::getRegex($checkName);
return preg_match($matchRegex, trim($value));
}
static function getRegex($name)
{
if (isset(self::$regex[strtolower($name)])) {
return self::$regex[strtolower($name)];
} else {
return $name;
}
}
}
!defined('IN_MP') &amp;&amp; die('Forbidden!');
class dir extends DirectoryIterator
{
static function delDir($directory, $subdir = true)
{
if (is_dir($directory) == false) {
return false;
}
$handle = opendir($directory);
while (($file = readdir($handle)) !== false) {
if ($file != '.' &amp;&amp; $file != '..') {
is_dir("{$directory}/{$file}") ? Dir::delDir("{$directory}/{$file}") : unlink("{$directory}/{$file}");
}
}
if (readdir($handle) == false) {
closedir($handle);
rmdir($directory);
}
}
static function del($directory, $dir2 = 1)
{
if (is_dir($directory) == false) {
return false;
}
$handle = opendir($directory);
while (($file = readdir($handle)) !== false) {
if ($file != '.' &amp;&amp; $file != '..') {
if (is_file("{$directory}/{$file}")) {
unlink("{$directory}/{$file}");
} elseif (is_dir("{$directory}/{$file}") &amp;&amp; $dir2 == 1) {
dir::deldir("{$directory}/{$file}");
}
}
}
closedir($handle);
}
static function copyDir($source, $destination)
{
if (is_dir($source) == false) {
die('The Source Directory Is Not Exist!');
}
if (is_dir($destination) == false) {
mkdir($destination, 448);
}
$handle = opendir($source);
while (false !== ($file = readdir($handle))) {
if ($file != '.' &amp;&amp; $file != '..') {
is_dir("{$source}/{$file}") ? Dir::copyDir("{$source}/{$file}", "{$destination}/{$file}") : copy("{$source}/{$file}", "{$destination}/{$file}");
}
}
closedir($handle);
}
static function getDirList($source, $type = 'ALL', $no = array('.', '..'), $ext = array())
{
if (is_dir($source) == false) {
return array();
}
$handle = opendir($source);
$dirlist = array();
while (false !== ($file = readdir($handle))) {
if (!in_array($file, $no)) {
if ($type == 'DIR' &amp;&amp; !is_dir($source . '/' . $file)) {
continue;
}
if ($type == 'FILE' &amp;&amp; !is_file($source . '/' . $file)) {
continue;
}
if (!empty($ext)) {
if (is_array($ext)) {
$rs = in_array(end(explode('.', $file)), $ext);
} else {
$rs = in_str(end(explode('.', $file)), $ext);
}
if (!$rs) {
continue;
}
}
$dirlist[] = $file;
}
}
closedir($handle);
return $dirlist;
}
}
class ftp
{
protected $_config = array('ftp_host' =&gt; 'img.mvou.com', 'ftp_port' =&gt; '21', 'ftp_user' =&gt; 'mvou', 'ftp_pwd' =&gt; 'xiaoyu', 'ftp_timeout' =&gt; '30', 'ftp_dir' =&gt; '/', 'ftp_pasv' =&gt; 1);
protected $_conn = null;
protected $_rs = null;
public function __construct($config = array())
{
!function_exists('ftp_connect') &amp;&amp; die('FTP模块不支持!');
$this-&gt;config($config);
}
public function config($config = array())
{
$this-&gt;_config = array_merge($this-&gt;_config, $config);
}
public function connect()
{
$this-&gt;_conn = @ftp_connect($this-&gt;_config['ftp_host'], $this-&gt;_config['ftp_port'], $this-&gt;_config['ftp_timeout']);
if (!$this-&gt;_conn) {
$this-&gt;error('FTP服务器连接失败! 请检查服务器地址和端口!');
return -1;
}
$this-&gt;_rs = @ftp_login($this-&gt;_conn, $this-&gt;_config['ftp_user'], $this-&gt;_config['ftp_pwd']);
if (!$this-&gt;_rs) {
$this-&gt;error('FTP登录错误! 请检查用户名和密码!');
return -2;
}
$this-&gt;_config['ftp_pasv'] &amp;&amp; $this-&gt;pasv(true);
if (!$this-&gt;chdir($this-&gt;_config['ftp_dir'])) {
$this-&gt;error($this-&gt;_config['ftp_dir'] . ': 切换到FTP当前目录失败! 请检查目录是否存在!');
return -3;
}
return $this;
}
public function chdir($dir)
{
return @ftp_chdir($this-&gt;_conn, $dir);
}
public function is_file($file)
{
$buff = @ftp_mdtm($this-&gt;_conn, $file);
if ($buff != -1) {
return true;
} else {
return false;
}
}
public function pasv($mode = true)
{
return @ftp_pasv($this-&gt;_conn, true);
}
public function put($local_file, $remote_file, $mode = 'B')
{
if ($mode == 'B') {
$mode = FTP_BINARY;
} else {
$mode = FTP_ASCII;
}
$this-&gt;mkdirs(dirname($remote_file));
$rs = @ftp_put($this-&gt;_conn, $remote_file, $local_file, $mode);
return $rs;
}
public function mkdirs($dir)
{
$dir = str_replace('\\', '/', $dir);
$dirs = explode('/', $dir);
$total = count($dirs);
foreach ($dirs as $val) {
if ($val == '.') {
continue;
}
if ($this-&gt;chdir($val) == false) {
if (!$this-&gt;mkdir($val)) {
$this-&gt;error($val . ': 创建失败!');
return false;
}
$this-&gt;chdir($val);
}
}
$this-&gt;chdir($this-&gt;_config['ftp_dir']);
return true;
}
public function mkdir($dir)
{
return @ftp_mkdir($this-&gt;_conn, $dir);
}
public function unlink($file)
{
return @ftp_delete($this-&gt;_conn, $file);
}
public function rename($old_name, $new_name)
{
return @ftp_rename($this-&gt;_conn, $old_name, $new_name);
}
public function rmdir($dir)
{
return @ftp_rmdir($this-&gt;_conn, $dir);
}
public function rmdirs($dir, $flag = 1)
{
$res = $this-&gt;rmdir($dir) || $this-&gt;unlink($dir);
if (!$res) {
$files = $this-&gt;nlist($dir);
if (empty($files)) {
return true;
}
foreach ($files as $file) {
$file = basename($file);
$this-&gt;rmdirs($dir . '/' . $file);
}
if ($flag) {
$this-&gt;rmdirs($dir);
}
}
return true;
}
public function nlist($dir)
{
return @ftp_nlist($this-&gt;_conn, $dir);
}
public function error($msg = 'Error!')
{
throw new Error($msg);
}
}
class html
{
static function path($html_name)
{
$md5 = md5($html_name);
$path = DATA . '~html/' . dirname($html_name) . '/' . $md5[0] . '_' . $md5[1] . '/' . basename($html_name) . '.php';
return $path;
}
static function read($html_name, $expire = 0)
{
if (!$expire) {
return false;
}
$html_name = self::path($html_name);
if (is_file($html_name) &amp;&amp; filemtime($html_name) + $expire &gt; now()) {
@readfile($html_name);
return true;
} else {
return $false;
}
}
static function write($html_name, $expire = 0)
{
if (!$expire) {
return false;
}
$html_name = self::path($html_name);
$con = @ob_get_contents();
return write_file($html_name, $con);
}
}?&gt;
<a href="#code002">返回引用文字处</a>