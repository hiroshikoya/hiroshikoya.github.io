---
layout: post
title: ブラウザの．
date: 2015-04-12
tags: [IE,tech]
---

IE8の話.
誰が興味あるのかという感じの話だけど，ちょっと広がりを感じたので,備忘的にまとめる．
色々と叩く機会があり,挙動としてIE9と大きく異なると思うことがある．
  
直近でぶつかった問題でこういうのがあった．  

Set ie_ = CreateObject("Shell.Application").Windows(0)

で返ってくるのがShellWindowsオブジェクトの参照キー(ハンドルというらしい)を返してくる.

これがぱっと見,数値に見えるのだけど数値として比較すると,アクセスできないとか言われてエラーになる.
(IE8でだけ.IE9では普通に動いた)

typeof ie_ 

とかやると'unknown'とか出力される。
そもそもtypeof演算子で'unknown'とかあるの？？という.

https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/typeof

>以下は typeof が返す事が出来る値（文字列）の一覧表です。 JavaScript data structureにデータ型(types)とプリミティブについての詳しい情報があります。
>型 	戻り値
>未定義 	"undefined"
>Null 	"object"
>真偽値 	"boolean"
>数値 	"number"
>文字列 	"string"
>シンボル (ECMAScript6 で新しく導入) 	"symbol"
>ホストオブジェクト (provided by the JS environment) 	実装に委ねる
>関数オブジェクト (implements [[Call]] in ECMA-262 terms) 	"function"
>E4X XML オブジェクト 	"xml"
>E4X XMLList オブジェクト 	"xml"
>他のオブジェクト 	"object"


で、色々見ていった結果、どうやらこういうことらしい.
  
http://stackoverflow.com/questions/10982739/typeof-returning-unknown-in-ie

>Internet Explorer displays “unknown” when the object in question is on the other side of a COM+ bridge. You may not know this or realize this, but MS’s XMLHTTP object is part of a different COM+ object that implements IUnknown; when you call methods on it, you’re doing so over a COM bridge and not calling native JavaScript.
>Basically that’s MS’s answer if you try to test or access something that’s not a true part of the JScript engine.
  
どうもIE8で動いているのはJScriptで、厳密にはJavaScriptではない。
そしてIE9で動いているのはChackraというエンジンでこれまた

こういうのはほとんど歴史の話であるが,個人的には非常に面白いと感じる．  

