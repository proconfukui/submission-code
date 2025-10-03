# ファイル構造

- `client.py`：クライアント用プログラム
- `server.py`：サーバー用プログラム
- `weights.txt`：ビームサーチで使う重みのデータ
- `solver`：クライアントが利用するソルバー一式（実際の試合では異なるプログラムが用いられる可能性がある）
  - `base.cpp`,`base.hpp`：フィールドの回転・表示、ペア数のカウント、ハッシュ計算、ランダム
  - `create_answer_json.cpp`：`main.cpp`の出力を元に、回答フォーマットを作成する
  - `evalution.cpp`,`evalution.hpp`：
  - `input_problem.cpp`：問題フォーマットを読み取り、`main.cpp`が使える形式で出力する
  - `json.hpp`：[nlohmannライブラリ](https://github.com/nlohmann/json)。`create_answer_json.cpp`,`input_problem.cpp`でJSONファイルの読み書きに使う
  - `main.cpp`：ソルバーのメイン部分
  - `steps_table.hpp`：有り得る導きの通り数の、フィールドのサイズごとのリスト
  - `utils.hpp`：導きの情報やビームサーチを実装する構造体

# 事前準備

1台のPCをサーバー（兼クライアント）、2台のPCをクライアントとして用いる。

1. サーバーのPythonに、`requests`ライブラリをダウンロードしておく。
1. `solver`フォルダ内の`input_problem.cpp`,`main.cpp`,`create_answer_json.cpp`をコンパイルし、実行ファイルを作成する。言語規格はC++17を用いること。
1. `client.py`の`INPUT_PROBLEM_FILE_PATH`,`MAIN_FILE_PATH`,`CREATE_ANSWER_JSON_FILE_PATH`の値を、1.で作成した実行ファイルのパスに変更する。
1. `server.py`の`API_URL`を競技サーバーで使われるAPIのエンドポイントに、`TOKEN`を自分のチームの認証トークンに書き換える。
1. `client.py`の`SERVER_HOST`をサーバーPCのIPアドレスに、`SERVER_PORT`をサーバーPCのポート番号に書き換える。

# 競技での運用方法

1. クライアント側で必要に応じて、`weights.txt`の数値や、`client.py`の`WEIGHTS_FILE_LINE`（使用する重みの行番号）を変更する。
1. サーバーが`server.py`を起動する。
1. クライアントが`client.py`を起動する。「接続成功。問題受信を待機...」と出力されていれば、正常にサーバーと通信できている。
1. 試合中の任意のタイミングで`submit`と入力すると、その時点での最良解が試合サーバーに送信される。
   - `status`と入力することで、その時点での最良のペア数・手数を確認できる。
