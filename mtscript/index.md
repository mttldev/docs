# MTScript
トランスパイル型言語として開発されている言語です。  
Ren'Pyでのストーリー作成を支援するために開発されました。

# Reference

> **Note**  
> 現在開発途中の言語であり、Python版コンパイラとJavaScript版コンパイラによって仕様が異なる場合があります。  
> ここでのリファレンスは、現在開発中の最新版コンパイラでの仕様にのっとるものとします。

## ;;<ins>*label*</ins>
ラベルを定義します。  
Ren'Pyにおける`label title:`に該当します。  

### 引数
- `label`  
    定義したいラベル名

### MTScriptでの記載
```mtsc
;;title
```

### コンパイル結果
```renpy
label title:
```


## <ins>*chara*</ins>「<ins>*message*</ins>」
キャラクターにメッセージを喋らせます。  
Ren'PyにおけるSay構文に該当します。

また、コンパイルされたメッセージ内にもカギかっこが残る仕様になっています。

> **Note**  
> コンパイルされた後にもメッセージ内にカギかっこが残ります。  
> ノベルゲームとして違和感ないような文を作り上げることを意識してください。

### 引数
- `chara`  
    喋らせたいキャラクター名またはそのエイリアス
- `message`  
    喋らせたい内容

### MTScriptでの記載
```mtsc
A君「おはよう！」
```

### コンパイル結果
```renpy
"A君" "「おはよう！」"
```

> **Note**  
> 通常、MTScriptの内容はコンパイル時にインデントされます。  
> この構文でも通常はインデントが発生しますが、見やすさのためにインデントは無視します。


## 「<ins>*message*</ins>」
ナレーター（キャラクターなし）として喋らせます。

### 引数
- `message`  
    喋らせたいメッセージ

### MTScriptでの記載
```mtsc
A君「こんにちは！」
「と、彼が元気に挨拶をした。」
```

### コンパイル結果
```renpy
"A君" "「こんにちは！」"
"と、彼が元気に挨拶をした。"
```

> **Waring**  
> ナレーター構文においては、`message`の最後が「。」で終わることが推奨されています。  
> 現段階でエラーなどは出力されませんが、ノベルゲームとして違和感がないようにしてください。


## ;<ins>*alias*</ins>:<ins>*chara*</ins>
キャラクターを指定のエイリアスで定義します。  
これを使用することで、Ren'Py Script内で定義されたキャラクターも使用できます。

### 引数
- `alias`  
    使用したいエイリアス名
- `chara`  
    定義したいキャラクター

### MTScriptでの記載
```mtsc
A君「おはよう！」
;A君:man_a
A君「こんにちは！」
```

### コンパイル結果
```renpy
"A君" "おはよう！"
man_a "こんにちは！"
```


## #<ins>*comment*</ins>
コンパイル時に引き継がれるコメントを作成します。

### 引数
- `comment`  
    書きたいコメント

### MTScriptでの記載
```mtsc
# 下をエイリーン以外に喋らせたほうがいいかもしれない。
エイリーン「こんにちは！」
```

### コンパイル結果
```renpy
# 下をエイリーン以外に喋らせたほうがいいかもしれない。
"エイリーン" "こんにちは！"
```


## :<ins>*text*</ins>
コンパイル時にそのまま引き継がれるテキストを定義します。  
これを使用することで、`MTScript`で公式的にサポートされていないRen'Py Script構文も使用することができます。

### 引数
- `text`  
    書きたいテキスト

### MTScriptでの記載
```mtsc
:jump morning_scene
```

### コンパイル結果
```renpy
jump morning_scene
```


## $inlude <<ins>*path*</ins>>
指定したパスのファイルをインクルードします。

### 引数
- `path`  
    インクルードしたいファイルのパス

### 処理

`include`構文は、指定したファイルを読み込みます。  
この時、読み込むファイルによって処理が異なります。

FileType|処理|インデント
---|---|---
`.mtsc`|コンパイルされる|なし
`.py`|まず`python:`というPythonステートメント宣言が追加される。<br>その後にファイルの中身が読み込まれる|Pythonステートメント宣言部: 1インデント<br>Pythonコード部: 2インデント
`.rpy`|そのまま読み込まれる|なし
その他|エラー|...

# Example

以下はMTScriptのサンプルです。

## main.mtsc
```mtsc
$include <chara.mtsc>

;;morning
# ここから朝のストーリー
A君「おはよう！」
「Aが元気に挨拶をする。」
Bさん「あっA君だ！」
Bちゃん「おっはよー！」
「Bが陽気な返事をする。」
:jump evening
```

## chara.mtsc
```mtsc
;A君:chara_a
;Bさん:chara_b
;Bちゃん:chara_b
```

## コンパイル結果
```renpy
label morning:
    # ここから朝のストーリー
    chara_a "「おはよう！」"
    "Aが元気に挨拶をする。"
    chara_b "「あっA君だ！」"
    chara_b "「おっはよー！」"
    "Bが陽気な返事をする。"
    jump evening
```