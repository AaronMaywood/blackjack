# JavaScript によるブラックジャック実装

ルール
https://ja.wikibooks.org/wiki/%E3%83%88%E3%83%A9%E3%83%B3%E3%83%97/%E3%83%96%E3%83%A9%E3%83%83%E3%82%AF%E3%82%B8%E3%83%A3%E3%83%83%E3%82%AF
- 細かな動作はこちらを参考にしました https://p-game.jp/game/341/detail
    - 手札がAceと８のとき、19ではなく9としかカウントされなかった。バグかな？


- デッキの作成
    デッキは各スートの1〜13までのカード（13*4=52枚）とし、ジョーカーは含まれない
- デッキをシャッフル
- このプログラムでの仕様
    - プレイヤーは必ず３枚のカードを引く
    - 親は手持ちの数が17を超えるまで、カードを引く
    - （実際のゲーム）以下の手順をUIを付けて遊ばせる
        - プレイヤーはコインを掛ける
        - 親とプレイヤーに２枚づつカードが配られる
            - 親は二枚目のカードを伏せておく
        - プレイヤーはカードを追加する（ヒット）か、引くのを止める（スタンド）かを選ぶ
        - プレイヤーがスタンド（スティとも。カードを引くのを止める）すると、親は手札が17以上でなければ追加で引き続ける。
- この状態でプレイヤーと親との勝負判定をする
- スコアの付け方
    - 1(Ace) ... 1もしくは11と数える
    - 2〜10 ... この値をそのまま点数とする
    - 11〜13（絵札） ... 10と数える
- 判定
    - 22以上の場合にはバスト（ドボン、点数なし）
    - 両者がバストの場合は引き分け
    - 片方がバストの場合はバストした方が負け
    - それ以外の場合は、点数が高い方が勝ち
- このプログラムでの仕様
    - 次のゲームはなく、ページを再読込して最初からプレイ
    - （実際のゲーム）
        - 残りのデッキを使って次の試合へ。
            - ５ゲームで１セットとする
        - また、賭けたコインのやりとりの処理を行う。

## メモ
### C言語での実装例
http://www.shoeisha.com/book/hp/surasura/data/c/makeBJ_Q.pdf
http://tanichu.sakura.ne.jp/data/prog3_resume.pdf

### 関数型プログラミングで行う（高度すぎる話題）
https://qiita.com/Guvalif/items/a4fe01a4c069836db4d8
https://guvalif.github.io/functional-blackjack-v2/1
