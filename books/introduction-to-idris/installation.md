---
title: "Idrisのインストール"
---

Idris処理系のインストール方法を紹介します。


# 処理系

IdrisはIdris 1とIdris 2があります。

公式で特に言及されてないのですが、バージョン1.0を刻んだ1が推奨で、まだバージョン0.3な2が開発版のプレビューと思ってよさそうです。今回は1のインストール方法を紹介します。

# インストール
## Windows/Mac

[バイナリダウンロード](https://www.idris-lang.org/pages/download.html#binary)のところからダウンロードすれば動くんじゃないですかね。
Linuxユーザなので詳しくは分かりません。

## Ubuntu

公式で推奨されてるのがCabalによるインストールです。[Idrisのwiki](https://github.com/idris-lang/Idris-dev/wiki/Idris-on-Ubuntu)にUbuntuでのインストールが書かれてますが、コマンドがちょっと古いので別の方法を紹介します。

使ってる環境はUbuntu 20.10です。

まずcabalをインストールするところは変わりません。

``` shell-session
$ sudo aptinstall cabal-install make
```

ついでに `libffi` もインストールしておくと便利です。

``` shell-session
$ sudo apt install libffi-dev
```

cabalのバージョンはこうです。

``` shell-session
$ cabal --version
cabal-install version 3.0.0.0
compiled using version 3.0.1.0 of the Cabal library
```

cabalは色々と問題が指摘されてきて、CLIが刷新されています。Idrisのwikiとは違い、3系ならば `new-` 系コマンドを使うとよさそうです。

``` shell-session
$ cabal new-update
$ cabal new-install idris
```

さきほど `libffi` をインストールした方はFFIを有効にしてビルドしましょう。

``` shell-session
$ cabal new-install -f FFI idris
```

インストールにしばらくかかるので気長に待ちます。

完了すれば `~/.cabal/bin/` にidris系コマンドがイントールされます。

``` shell-session
$ ls ~/.cabal/bin
idris  idris-codegen-c  idris-codegen-javascript  idris-codegen-node
```

`~/.caba/bin` にパスを通しておきましょう。Bashユーザなら

``` shell-session
$ echo 'export PATH=~/.cabal/bin:$PATH' >> ~/.bashrc
$ source ~/.bashrc
```

とかすればよさそうです。

## ソースからビルド

以前ブログに書いたのでそちらを参考にしてみて下さい。

[idris環境構築 | κeenのHappy Hacκing Blog](https://keens.github.io/blog/2019/01/06/idriskankyoukouchiku/)


# 処理系の起動

前章ではコンパイルコマンドを紹介しました。今一度Hello Worldをコンパイル、実行してみましょう。

``` shell-session
$ cat <<'EOF' > HelloWorld.idr
main : IO ()
main = putStrLn "Hello World"
EOF
$ idris HelloWorld.idr -o HelloWorld
$ ./HelloWorld
Hello World
```

IdrisのコンパイラはデフォルトではCを生成し、裏でそのままCコンパイラを呼んで実行可能ファイルまで作ります。

Idrisには対話環境もあります。こちらはHaskellで実装されています。引数を何も与えずに起動すると対話環境が起動します。

``` shell-session
$ idris
     ____    __     _
    /  _/___/ /____(_)____
    / // __  / ___/ / ___/     Version 1.3.3
  _/ // /_/ / /  / (__  )      https://www.idris-lang.org/
 /___/\__,_/_/  /_/____/       Type :? for help

Idris is free software with ABSOLUTELY NO WARRANTY.
For details type :warranty.
Idris>
```

式を与えたらそのまま評価して返してくれるので簡単に動作を確かめたいときは重宝します。

```text
Idris> 1 + 1
2 : Integer
```

IO系のものは `:exec` で実行できます。

``` text
Idris> :exec putStrLn "Hello REPL"
Hello REPL
```


他にも色々できることはあるのですが、 細かい使い方は別の章に譲ることにしましょう。

# 本章のまとめ

Idrisの処理系のインストール方法を紹介しました。
