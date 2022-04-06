---
title: "スクロール速度に合わせて消えたり現れたりするヘッダーを作る"
emoji: "🙄"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["CSS", "design", "JavaScript"]
published: true
---

上にスクロールすると生えてくるヘッダーを作るときは JavaScript でクラスを付け替えてアニメーションは CSS で実現するというやり方が多いです。しかし、それだとアニメーションの速度が一定になってしまって気持ち悪い（と僕は感じる）ので、ヘッダーもスクロールと同じ速度で動くようにしたいと思います。

言葉で説明するのは難しいのですが、要するにこういう挙動ですね。

![Google 画像検索結果画面](/images/awesome-header/google.gif)

## サンプル

マウスだと（設定によりますが）速くて分かりにくいのでタッチパッドやスマホで試してみてください。

@[codepen](https://codepen.io/akimon658/pen/zYpROeV)

やることはシンプルで、`position: sticky;` で固定したヘッダーの `top` の値を JavaScript でいじるだけです。（もっと良い方法があったら教えてください）

```javascript
const header = document.querySelector("header"),
  headerStyle = window.getComputedStyle(header);

let lastPosition = 0;

document.addEventListener("scroll", () => {
  const currentPosition = window.scrollY,
    diff = currentPosition - lastPosition,
    headerHeight = parseFloat(headerStyle.height);

  let newTop = parseFloat(headerStyle.top) - diff;
  if (diff < 0) {
    newTop = Math.min(newTop, 0);
  } else {
    newTop = Math.max(newTop, 0 - headerHeight);
  }

  header.style.top = `${newTop}px`;
  lastPosition = currentPosition;
});
```

現在の `top` から移動距離を引いたものか、はみ出す場合には範囲の上限・下限を新しく `top` とします。

影をつける場合にはその高さも考慮した方が良いかもしれません。

```javascript
newTop = Math.max(newTop, 0 - headerHeight - shadowHeight)
```

## あとがき

ゆっくりスクロールしなきゃ分からないので拘りが無ければ CSS で十分だと思います。
