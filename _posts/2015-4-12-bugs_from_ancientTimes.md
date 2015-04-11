---
layout: post
title: IEブラウザに翻弄された話．
date: 2015-04-12
tags: [IE,tech]
---

といってもIE8の話です.  
誰が興味あるのかという感じの話だけど，ちょっと面白みを感じたので,備忘的にまとめる．  
色々と挙動を確認する機会があり,IE9とも結構違ってると思うことがある．  
  
##起こった事象  
***
直近でぶつかった問題でこういうのがあった．  
JavaScriptからActiveXを使って,ウィンドウの参照を取りたくて以下のようなスクリプトを使った.  
(こんなケースは多分稀だろう.)
  
Var ie_ = CreateObject("Shell.Application").Windows(0)  
  
こんな感じで呼ぶと返ってくるのがShellWindowsオブジェクトの参照キー(ハンドルというらしい).  
  
この値が一見,数値に見えるのだけどJavaScript上で数値比較とかすると,アクセスできないとか言われてエラーになった.  
(IE8でだけ.IE9では普通に動いた)  
(エラーの詳細は忘れた.手元にIE8が動く環境がない)  
  
さらに,  
  
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
  
    
今回の'unknown'てなんなの。'undefined'じゃないの.  
この変数にie_+1とかやると,ブランクが返ってきたり,挙動もなんだか怪しい.  
全く以ってなんだかよくわからなかった.  
  
  
##色々見ていった結果  
***
  
で、色々見ていった結果、どうやらこういうことらしい.  
  
[typeof returning “unknown” in IE:StackOverFlow](http://stackoverflow.com/questions/10982739/typeof-returning-unknown-in-ie)   
  
>Internet Explorer displays “unknown” when the object in question is on the other side of a COM+ bridge.   
>You may not know this or realize this, but MS’s XMLHTTP object is part of a different COM+ object that implements IUnknown;  
>when you call methods on it, you’re doing so over a COM bridge and not calling native JavaScript.  
>Basically that’s MS’s answer if you try to test or access something that’s not a true part of the JScript engine.  
  
要はSHellの世界のオブジェクトが渡されていて、これがIUnnownというインターフェースを継承しているからデバッグするとこんな返り値になっているぽい.  
さらにShellScriptに詳しい人に聞いてみたら、  
JScriptでは64bitのIntegerを正しく認識してくれないかもしれないとのこと.  
  
[この記事](http://bytes.com/topic/javascript/answers/146461-int64-method-com-into-javascript)にも以下のような記載がありました.

>I think in fact *you* do. Since ECMAScript and implementations use
IEEE-754 doubles only, there is no exact representation of a 64 bit
integer value in these languages.  
> Proof: The largest value a signed
64 bit integer can hold is (2^63)-1 (1 MSB + 63 bits) which is
9223372036854775807. However, JavaScript (v1.5 in Mozilla Firefox)
returns 9223372036854776000 for Math.pow(2, 63)-1 because of the 64
bits available for the floating-point number some bits are required
for the exponent (the stored value is 9.2233720368547760E18):
  
  
##ブラウザで動くJavaScriptエンジンの話    
***

IE8だけで起こったのは,どうもIE8でJavaScriptを動かしてるのはJScriptで,こいつのハンドリングがいけてないぽい.  
IE9でJavaScriptエンジンはChakraで，このエンジンの違いが両者の挙動の違いを生んでいる模様．  
  
[JavaScript エンジン Chakra を無理矢理使う。|ういはるかぜの化学](https://subtech.g.hatena.ne.jp/mayuki/20111216/1324015296)   
  

事象としてはほとんど歴史の話といっていいくらい過去の話なんだろうけど,  
[この辺の話](http://ja.wikipedia.org/wiki/%E3%83%96%E3%83%A9%E3%82%A6%E3%82%B6%E6%88%A6%E4%BA%89#.E7.AC.AC.E4.BA.8C.E6.AC.A1.E3.83.96.E3.83.A9.E3.82.A6.E3.82.B6.E6.88.A6.E4.BA.89)を体感した感じというか,ちょっと明らかな改善?というか違いだったので個人的には面白いと感じた．  
  
***  
  
  
じゃー新しいブラウザ**Spartan**ではどうなるの？とかなんとなく気になったんだけど,  
それはまた別のPOSTとして書こうと思う.(まだよく調べてはいない)  
  
まーActiveXはいよいよ滅亡するぽいのだけど. 
