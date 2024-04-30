# 動画ストリーミング再生
## ファイル説明
- hls.shがffmpegの実行をするコマンド
- endpoint.htmlに動画再生用のhls.jsや分割されたファイルのURLが記載されている
- (redundantの中の不可視ファイルはサーバーの設定で必要かどうか、、)
## 全体の流れ
1. ffmpegをインストール(されていれば不要)
1. 対象の動画をffmpegで分割
1. 分割したファイル群をWebサーバーにアップ
1. アップしたURLをendpoint.htmlに記述
1. endpoint.htmlをアップしたURLをエンドポイントとする
1. エンドポイントにアクセスすると動画がストリーミング再生される

## ffmpegをインストール(されていれば不要)
```
brew install ffmpeg
```
## 対象の動画をffmpegで分割
hls.shに記述してあるコマンドをzshから実行する  
（動画の元ファイルは同じディレクトリに置いておくとパスの記述が楽）
```
ffmpeg -i 元ファイル名.mp4 -c:v copy -c:a copy -f hls -hls_time 1 -hls_playlist_type vod -hls_segment_filename "分割ファイル名_%3d.ts" エンドポイント.m3u8
```
分割ファイル名とエンドポイント名は毎回変える必要は無いので、元ファイルだけ適宜変えれば良い。

## 分割したファイル群をWebサーバーにアップ
サーバーによっては実行権限など調整が必要かもしれない
## アップしたURLをendpoint.htmlに記述
エンドポイント名を固定しておけば、毎回同じendpoint.htmlで済むかもしれない
## endpoint.htmlをアップしたURLをエンドポイントとする
endpoint.htmlを任意の場所にアップし、そこURLがエンドポイントとなる。
## エンドポイントにアクセスすると動画がストリーミング再生される
エンドポイントURLを開くと全画面で動画が再生される。hls非対応のブラウザの為に、hls.jsで再生できるよう`if `で分岐させている。  
`Hls.isSupported()`を実行するとhls対応ブラウザは`true`を返す。