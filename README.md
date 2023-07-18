# blackjack

## 用語
- ディーラー（胴元、親） ... プレイヤーの対戦相手
- デッキ ... トランプの束、ゲーム前によくシャッフルし、ここからカードを引く
- スート ... トランプの柄（♥♠♣◆）（ブラックジャックでは使用しない）
- ランク ... トランプの番号（1〜13）
- ディール ... ディーラーがカードを一枚配ること
- ヒット ... カードを引いて手札に加える
- スタンド ... カードを引かず、この手で勝負する
- ブラックジャック ... 手札がちょうど21
- バスト（破産） ... 21を超えてしまった（22以上、負けか引き分けになり、勝つことはない）

## メモ
- プレイヤーがヒットしたとき、ディーラーはたとえ17以下であってもカードを追加しない
    もし、追加する仕組みにした時、以下の手でプレイヤーがカードを引いたのにディーラーが引かないと、ディーラーの手が17以上になっていることが明らかになる（この場合、???は10以上で、最高でも18点とわかる）ため
    https://i.gyazo.com/9f7527de219d3897098b074b1706826c.png
    →ディーラーが手札に加えるのは、プレイヤーがスタンド（この手で勝負する）を選んだ時のみ

## GitHub Pages へのデプロイ

URL:
https://AaronMaywood.github.io/blackjack/

- GitHub Pages は doc/ という名前のフォルダをデプロイ可能（フォルダ名は選択不可能）
- npm run build で生成した dist/ の中身を doc/ の中に上書きするとその内容でデプロイされる

