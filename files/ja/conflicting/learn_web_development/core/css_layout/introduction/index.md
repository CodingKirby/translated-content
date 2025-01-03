---
title: 通常フロー
slug: conflicting/Learn_web_development/Core/CSS_layout/Introduction
original_slug: Learn/CSS/CSS_layout/Normal_Flow
l10n:
  sourceCommit: 289d6314f3368aa3e28524e7d090f6e9c704e3b1
---

{{LearnSidebar}}

{{PreviousMenuNext("Learn/CSS/CSS_layout/Introduction", "Learn/CSS/CSS_layout/Flexbox", "Learn/CSS/CSS_layout")}}

この記事では、通常フロー、つまりレイアウトを変更していない場合のウェブページの要素のレイアウト方法について説明します。

<table>
  <tbody>
    <tr>
      <th scope="row">前提知識:</th>
      <td>
        HTML の基礎（<a href="/ja/docs/Learn/HTML/Introduction_to_HTML"
          >HTML 入門</a
        >で学習）、および CSS の動作の考え方（
        <a href="/ja/docs/Learn/CSS/First_steps">CSS 入門</a>で学習）
      </td>
    </tr>
    <tr>
      <th scope="row">目的:</th>
      <td>
        変更を加える前に、ブラウザーが既定ででウェブページをどのようにレイアウトするかを説明します。
      </td>
    </tr>
  </tbody>
</table>

レイアウトを紹介した前回のレッスンで詳しく説明したように、CSS を適用してふるまいを変更していない場合、ウェブページ上の要素は通常フローでレイアウトされます。そして、理解を深め始めるにつれ、その通常フローの中で要素の位置を調整したり、要素を完全に取り除くたりして、要素のふるまいを変更できます。通常フローで読み取り可能な、しっかりとよく構造化された文書から始めることは、どんなウェブページでも始めるための最良の方法です。それは、ユーザーが非常に制限されたブラウザーを使用している場合や、ページのコンテンツを読み上げるスクリーンリーダーなどのデバイスを使用している場合でも、コンテンツを読みやすくすることができます。また、通常フローは読みやすい文書を作るために設計されているため、この方法で始める場合、レイアウトを変更しながら文書と格闘するのではなく、文書と一緒に作業することになります。

さまざまなレイアウト方法を深く掘り下げる前に、以前のモジュールで通常の文書フローに関して検討したことのいくつかを再検討する価値があります。

## 要素は既定でどのようにレイアウトされるか

まず始めに、個々の要素ボックスは要素のコンテンツを取り、それからそれらの周りにパディング (padding、詰め物)、境界 (border)、マージン (margin、余白) を追加することによってレイアウトされます。これはまた以前に見たことがある**ボックスモデル**のことです。

既定では、[ブロックレベル要素](/ja/docs/Glossary/Block-level_content)のコンテンツは、それを包含する親要素の利用可能なインライン空間を満たし、そのコンテンツを収めるためにブロック方向へ成長します。[インラインレベル要素](/ja/docs/Glossary/Inline-level_content)のサイズは、そのコンテンツのサイズだけです。 {{cssxref("width")}} や {{cssxref("height")}} を、 {{cssxref("display")}} プロパティが既定で `inline` である要素、例えば {{HTMLElement("img")}} に設定しても、 `display` 値は `inline` のままです。

この方法でインラインレベル要素の `display` プロパティを制御したい場合は、 CSS を使用してブロックレベル要素のように動作するように設定します（例えば、 `display: block;` または `display: inline-block;` を使用し、両方の特性を混合します）。

それは個々の要素を説明していますが、要素がどのように相互作用するかについてはどうでしょうか？ 通常のレイアウトフロー (レイアウト入門の記事に記載) は、ブラウザーのビューポート内に要素を配置するシステムです デフォルトでは、ブロックレベル要素は、親の[書字方向](/ja/docs/Web/CSS/writing-mode) (writing mode、_初期値_ は horizontal-tb) に基づいて*ブロックのフロー方向* にレイアウトされます。各要素は最後の行の下に新しい行として現れ、各要素は指定したマージンで区切られます。例えば英語では（あるいは他の横書き、上から下への書字方向でも）ブロックレベルの要素は縦に並べられます。

インライン要素は異なるふるまいをします。新しい行に現れません。代わりに、親ブロックレベル要素の幅の内側にマージンがある限り、それらは互いに同じ行に配置され、隣接する (または折り返された) テキストコンテンツに配置されます。空間がなければ、あふれているテキストや要素は新しい行に移動します。

隣接する 2 つの要素の両方にマージンが設定されていて、2 つのマージンが接触している場合、2 つのうち大きい方が残り、小さい方が消えます。これは[**マージンの相殺**](/ja/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing) (margin collapsing) と呼ばれます。
マージンの相殺は、**垂直方向**のみ発生します。

これらすべてを説明する簡単な例を見てみましょう。

```html-nolint
<h1>基本的な文書フロー</h1>

<p>
  これは基本的なブロックレベル要素です。隣接するブロックレベル要素は下の新しい行に配置されています。
</p>

<p>
  既定では、親要素の幅の 100% にわたり、子コンテンツと同じ高さになります。幅と高さの合計は、コンテンツ + パディング + 境界の幅/高さです。
</p>

<p>
  マージンによって分けられています。マージンの相殺のため、両方ではなく、 1 つのマージンの幅で区切られます。
</p>

<p>
  <span>これ</span>と<span>これ</span>のようなインライン要素は、同じ行に配置され、同じ行に空間がある場合は隣接するテキストノードに配置されます。
  あふれたインライン要素は、<span>可能であれば（このようにテキストを含むものであれば）新しい行に折り返され</span>、そうでなければ次の画像のように単に新しい行に進みます。
  <img src="long.jpg" alt="snippet of cloth" />
</p>
```

```css
body {
  width: 500px;
  margin: 0 auto;
}

p {
  background: rgb(255 84 104 / 30%);
  border: 2px solid rgb(255 84 104);
  padding: 10px;
  margin: 10px;
}

span {
  background: white;
  border: 1px solid black;
}
```

{{ EmbedLiveSample('How_are_elements_laid_out_by_default', '100%', 600) }}

## まとめ

この学習では、通常フロー（CSS 要素の既定レイアウト）の基本を学びました。既定ではインライン要素、ブロック要素、マージンがどのように動作するかを理解することで、将来的にそれらの動作を変更しやすくなります。

次の記事では、[フレックスボックス](/ja/docs/Learn/CSS/CSS_layout/Flexbox)を使用して CSS 要素に変更を加えることで、この知識をベースにしていきます。

{{PreviousMenuNext("Learn/CSS/CSS_layout/Introduction", "Learn/CSS/CSS_layout/Flexbox", "Learn/CSS/CSS_layout")}}
