---
ID: 4409
post_title: 我的代码方法
author: ChinaBUG
post_excerpt: ""
layout: page
permalink: http://blog.ipodmp.com/chianbug-code
published: true
post_date: 2016-06-14 14:49:48
---
<pre lang="php">/**
 * 检测是否base64加密
 * @params $code        待识别的字符串
 * @params return       结果
 */
function b64code( $code ){
    if( !$code ) return $code;
    if ( $code == base64_encode( base64_decode( $code ) ) ) {
        return base64_decode( $code );
    }else{
        return $code;
    }
}

/**
 * 过滤非法字符，保留中文
 * @params $code        待识别的字符串
 * @params $charIn      待识别的编码，统一转为utf-8判断
 * @params $charOut     输出的编码
 * @params return       结果
 */
function mbshowcn( $code, $charIn=null, $charOut=null ){
    if( !$code ) return $code;
    $mb_detect_order = array('UTF-8','GB2312','BIG5','GBK','ASCII','JIS', 'eucjp-win','sjis-win','EUC-JP');
    if( $charIn != null ){
        if( $charIn == 'auto' )
            $charIn = strtoupper(mb_detect_encoding( $code, "auto" ));
        if( $charIn != 'UTF-8' ){
            if( !in_array($charIn, $mb_detect_order) ) $mb_detect_order[] = strtoupper($charIn);
            $code = mb_convert_encoding( $code, 'UTF-8', $charIn );           
        }
    }
    preg_match_all('/[\x{4e00}-\x{9fff}A-Za-z0-9]+/u', $code, $matches);
    $code = join('', $matches[0]);
    if( ( $charOut != null ) && ( $charOut != 'utf-8' ) ){
        if( !in_array($charOut, $mb_detect_order) ) $mb_detect_order[] = strtoupper($charOut);
        $code = mb_convert_encoding( $code, $charOut, 'UTF-8' );
    }
    return $code;
}
</pre>