---
layout: post
title: ブラウザの．
date: 2015-04-12
tags: [IE,tech]
---

IE8の話.  
誰が興味あるのかという感じの話だけど，ちょっと広がりを感じたので,備忘的にまとめる．  
色々と叩く機会があり,挙動としてIE9と大きく異なると思うことがある．  
  
##ぶつかった問題  
***
直近でぶつかった問題でこういうのがあった．  
  
Set ie_ = CreateObject("Shell.Application").Windows(0)  
  
で返ってくるのがShellWindowsオブジェクトの参照キー(ハンドルというらしい)を返してくる.  
  
これがぱっと見,数値に見えるのだけど数値として比較すると,アクセスできないとか言われてエラーになる.  
(IE8でだけ.IE9では普通に動いた)  
  
typeof ie_   
  
とかやると'unknown'とか出力される。  
そもそもtypeof演算子で'unknown'とかあるの？？という.  

[typeof演算子:MSDN](https://msdn.microsoft.com/ja-jp/library/ie/259s7zc1%28v=vs.94%29.aspx)   

>typeof 演算子は、型情報を文字列で返します。  
>typeof 演算子が返す文字列は、"number"、"string"、"boolean"、"object"、"function"、"undefined" の 6 つです。   

[typeof演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/typeof)   
  
>以下は typeof が返す事が出来る値（文字列）の一覧表です。 JavaScript data   structureにデータ型(types)とプリミティブについての詳しい情報があります。  
型 | 戻り値 
:----- | :------:  
未定義   | "undefined"  
Null   | "object"  
真偽値   | "boolean"  
数値 	|"number"  
文字列 	|"string"  
シンボル (ECMAScript6 で新しく導入) |	"symbol"  
ホストオブジェクト (provided by the JS environment) |実装に委ねる    
関数オブジェクト (implements [[Call]] in ECMA-262 terms) 	|"function"    
E4X XML オブジェクト 	|"xml"  
E4X XMLList オブジェクト 	|"xml"  
他のオブジェクト 	|"object"  
  
  
今回の'unknown'てなんなの。  
この変数にie_+1とかやると,ブランクが返ってきたり,全く以ってなんだかよくわからなかった.  
  
  
##色々見ていった結果  
***
  
で、色々見ていった結果、どうやらこういうことらしい.  
  
[typeof returning “unknown” in IE:StackOverFlow](http://stackoverflow.com/questions/10982739/typeof-returning-unknown-in-ie)   
  
>Internet Explorer displays “unknown” when the object in question is on the other side of a COM+ bridge.   
>You may not know this or realize this, but MS’s XMLHTTP object is part of a different COM+ object that implements IUnknown;  
>when you call methods on it, you’re doing so over a COM bridge and not calling native JavaScript.  
>Basically that’s MS’s answer if you try to test or access something that’s not a true part of the JScript engine.  
  

どうもIE8で動いているのはJScriptで、IE9で動いているのはChackraというエンジンでこの違いが両者の挙動の違いを生んでいる模様．  
  
ShellScriptに詳しい人に聞いたら、JScriptでは32bitのIntegerを正しく認識してくれないらしい.
  
こういうのはほとんど歴史の話であるが,個人的には非常に面白いと感じる．  

