---
title: "Flaskで挨拶を返してくれるだけのアプリをつくる"
description: "Flaskの記事"
pubDate: 2026-11-16
tags: ["Python", "Flask"]
---
### 概要
Flaskで挨拶を返してくれるアプリを作ります。  

### フォルダ、ファイルを準備する。
flask-appというフォルダを作り、その中に以下のようにフォルダ、ファイルを作成します。
![画像](/images/05_Flask/1.png)  
templatesフォルダ内
![画像](/images/05_Flask/2.png)  

### Pythonのコードを書く
app.pyに以下のように書きます。

```
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def home():
    message = ""

    if request.method == "POST":
        name = request.form.get("name", "").strip()

        if name:
            message = f"こんにちは、{name}さん！"
        else:
            message = "名前を入力してください"

    return render_template("index.html", message=message)

if __name__ == "__main__":
    app.run(debug=True)
```

### Pythonのコード解説
```
@app.route("/", methods=["GET", "POST"])
```
@app.route() は、どのURLにアクセスしたら、どのPython関数を実行するかを指定します。  
「@app.route("/")」はトップページを意味します。  

```
def home():
    message = ""
```
トップページにアクセスすると、この home() 関数が実行されます。

```
name = request.form.get("name", "").strip()
```
「request.form」は、フォームからPOSTで送られた入力データを扱います。  
「request.form.get("name", "")」は、送られたデータのうち、name という名前の項目を取得します。  
"name" が存在しなければ、代わりに "" を返します。  

```
return render_template("index.html", message=message)
```
render_template() は、HTMLテンプレートを読み込んで、ブラウザに返すためのHTMLを生成します。  
ここでは、  
・"index.html"：表示するHTMLファイル  
・message=message：Pythonの変数 message の値をHTMLに渡す  
という意味になります。  

### HTMLのコードを書く
templates/index.htmlに以下のように書きます。
```
<!DOCTYPE html>
<html lang="ja">
    <head>
        <meta charset="UTF-8">
        <title>挨拶サイト</title>
    </head>
    <body>
        <h1>挨拶サイト</h1>

        <form method="POST">
            <input type="text" name="name" placeholder="名前を入力">
            <button type="submit">挨拶する</button>
        </form>

        <h2>{{ message }}</h2>
    </body>
</html>
```

### Webサイトを起動する
Flaskがまだ入っていない場合は、ターミナルで以下を実行します。
```
python -m pip install flask
```

flask-appフォルダ（app.pyがあるフォルダ）に移動し、以下を実行します。
```
python app.py
```

ターミナルに表示されるURLをブラウザで開きます。  
http://127.0.0.1:5000

アプリが表示され、名前を入力してボタンを押すと、挨拶が表示される。
