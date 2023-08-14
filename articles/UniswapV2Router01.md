---
title: "[Bunzz Decipher] UniswapV2Router01のコントラクトを理解しよう！"
emoji: "🦄"
type: "tech"
topics:
  - "blockchain"
  - "ブロックチェーン"
  - "solidity"
  - "ethereum"
  - "defi"
published: true
published_at: "2023-08-14 07:02"
publication_name: "cryptogames"
---

# はじめに

初めまして。
**CryptoGames**というブロックチェーンゲーム企業でエンジニアをしている **cardene（かるでね）** です！
スマートコントラクトを書いたり、フロントエンド・バックエンド・インフラと幅広く触れています。

https://cryptogames.co.jp/

代表的なゲームは**クリプトスペルズ**というブロックチェーンゲームです。

https://cryptospells.jp/

今回はBunzzの新機能『**DeCipher**』を使用して、UniswapV2の「**UniswapV2Router01**」のコントラクトを見てみようと思います。

:::message alert
公式のドキュメントでも書かれているように、**UniswapV2Router01**は非推奨になっており、**UniswapV2Router02**を使用することが推奨されています。

> UniswapV2Router01 should not be used any longer, because of the discovery of a low severity bug and the fact that some methods do not work with tokens that take fees on transfer. The current recommendation is to use UniswapV2Router02.
:::

『**DeCipher**』はAIを使用してコントラクトのドキュメントを自動生成してくれるサービスです。

https://www.bunzz.dev/decipher

詳しい使い方に関しては以下の記事を参考にしてください！

https://zenn.dev/heku/articles/33266f0c19d523

:::message
この記事はあくまで個人が「**DeCipher**」を使用してみての記事になります。
間違った部分などもあるかと思いますが、その際はコメントなどで教えてください。
:::

今回使用する『**DeCipher**』のリンクは以下になります。

https://app.bunzz.dev/decipher/chains/1/addresses/0xf164fC0Ec4E93095b804a4795bBe1e041497b92a

# 概要

**UniswapV2Router01**コントラクトは、Ethereumブロックチェーン上に構築された分散型取引所（DEX）であるUniswapプロトコルの重要な部分です。
このコントラクトは、**Uniswap**取引所への流動性の追加やトークンのスワップを促進する役割を果たしています。
**Uniswap**エコシステムの鍵となるコンポーネントであり、ユーザーがプロトコルと安全かつ標準的な方法でやり取りすることができます。

## コントラクトの目的
**UniswapV2Router01**コントラクトの主な目的は、**Uniswap**プロトコルとやり取りするためのインターフェースを提供することです。
これには、取引所への流動性の追加、トークンのスワップ、流動性の引き出しが含まれます。
コントラクトは柔軟で適応力があり、**Uniswap**エコシステム内でさまざまなトークンを使用できるように設計されています。

## コントラクトの責任
**UniswapV2Router01**コントラクトにはいくつかの主な責任があります。

### 流動性の追加
コントラクトは、**Uniswap**取引所に流動性を追加するための関数を提供します。
これには、特定の流動性プールに一対のトークンを預けることが含まれます。
コントラクトは、正しい量の各トークンが預けられ、トランザクションが正しく処理されるようにします。

### トークンのスワッ
コントラクトは、トークンのスワップもサポートしています。
これには、現在の流動性プールの交換レートに基づいて、トークンを交換することが含まれます。
コントラクトは、スワップが正しく実行され、正しい量の各トークンが転送されるようにします。

### 流動性の引き出し
コントラクトはユーザーが取引所から流動性を引き出すことを可能にします。
これには、特定の量の流動性トークンをプールから取り出し、その代わりに基になるトークンを受け取ることが含まれます。
コントラクトは、引き出しが正しく処理され、ユーザーが正しい量の各トークンを受け取るようにします。

### ERC20トークンとのインターフェース
コントラクトは、Ethereumブロックチェーンで使用される標準的なタイプのトークンであるERC20トークンとやり取りします。
これには、トークンの転送、引き出しのための承認、アカウントの残高の確認が含まれます。

### トランザクションの妥当性の確保
コントラクトには、トランザクションが妥当であることを確保するためのいくつかのセーフガードが組み込まれています。
これには、トランザクションの期限が過ぎていないことの確認や、正しい量のトークンが転送されていることの確認が含まれます。

:::message
まとめると、**UniswapV2Router01**コントラクトはUniswapプロトコルの重要な部分であり、取引所とやり取りするための安全で標準化されたインターフェースを提供します。
トランザクションが正しく処理され、ユーザーが安全な方法で流動性を追加し、引き出すことができるように保証します。
:::

# 使い方

**UniswapV2Router01**コントラクトは、**Uniswap**プロトコルの一部であり、Ethereumブロックチェーン上のアップグレード不可能なスマートコントラクトのシステムに実装された、定数積算式に基づく自動流動性プロトコルです。
これは高度に分散化され、操作可能な金融基盤を提供し、誰でも分散化された方法で新しい金融市場を作成できる世界を目指しています。

:::message
定数積算式に基づく自動流動性プロトコルとは、分散型取引所（DEX）や流動性プールを実現するための仕組みのことを指します。
具体的には、2つのトークン（通常は異なる種類のトークン）を1対1で交換できる取引所や流動性プールがあります。
ここで重要なのは、トークンAとトークンBの積（バランス）が常に一定であることです。
例えば、トークンAとトークンBのバランスがそれぞれ10と100の場合、その積は1,000です。この数値は変わりません。

ユーザーがトークンAをトークンBに交換すると、トークンAのバランスが少し減少し、トークンBのバランスが少し増加します。
しかしその積は変わらないため、取引価格はトークンAとトークンBのバランスの比率によって決まります。
これにより、市場の需要と供給に応じて価格が自動的に調整される仕組みが生まれます。

また、ユーザーは自分のトークンを流動性プールに提供することで手数料を得ることができます。
このときも、定数積算式が保たれるように、トークンのバランスが適切に調整されます。

このプロトコルの利点は、取引価格の変動が供給と需要に応じて自動的に行われるため、マーケットメイカーや流動性プロバイダーが継続的に市場を監視し調整する必要がないことです。
これにより、より分散化された、効率的な取引環境が実現されます。
:::

**UniswapV2Router01**コントラクトの目標は、**Uniswap V2**ペアとのやり取りを容易にすることです。
流動性の追加、流動性の削除、トークンのスワップのための関数を提供しています。

**UniswapV2Router01**コントラクトを使用する手順は以下の通りです。

### 1. コントラクトのデプロイ

**UniswapV2Router01**コントラクトをEthereumブロックチェーン上にデプロイします。
コンストラクタには、**Uniswap**ファクトリーコントラクトと**WETH**コントラクトのアドレスが必要です。

### 2. 流動性の追加

`addLiquidity`または`addLiquidityETH`関数を呼び出して、ペアに流動性を追加します。
この関数には、2つのトークンのアドレス、各トークンの希望数量と最小数量、流動性トークンを受け取るアドレス、期限が必要です。

### 3. 流動性の削除

`removeLiquidity`または`removeLiquidityETH`関数を呼び出して、ペアから流動性を削除します。
この関数には、2つのトークンのアドレス、削除する流動性トークンの数量、受け取る各トークンの最小数量、トークンを受け取るアドレス、期限が必要です。

### 4. トークンのスワップ

`swapExactTokensForTokens`、`swapTokensForExactTokens`、`swapExactETHForTokens`、`swapTokensForExactETH`、`swapExactTokensForETH`、`swapETHForExactTokens`のいずれかの関数を呼び出して、トークンをスワップします。
これには、入力トークンの数量、最小の出力トークンの数量、入力と出力トークンのアドレス、出力トークンを受け取るアドレス、期限が必要です。

### 5. 受け取ったETHの処理
**WETH**コントラクトから受け取ったETHを処理するためのフォールバック関数を実装します。

:::message
トークンの転送を含むすべての関数は、トークンコントラクトから事前に承認を受ける必要があります。
トークンコントラクトの`approve`関数を、**UniswapV2Router01**コントラクトのアドレスと承認するトークンの数量とともに呼び出す必要があります。
:::

# パラメータ

## _factory

**UniswapV2Factory**コントラクトのアドレス。
新しいトークンペアを作成し、既存のトークンペアを取得するために使用されます。

:::message
**UniswapV2Factory**については以下を参考にしてください。
:::

https://zenn.dev/cryptogames/articles/89744d2e9629f4

## _WETH

**Wrapped Ether（WETH）** コントラクトのアドレス。
**Uniswap**上の取引ペアの基軸通貨として使用されます。
つまり、**WETH**はEthereumの**ETH**をトークン化したものであり、**Uniswap**内でトークンとして取り扱うことができます。

# 定数・修飾子

## 定数

### WETH

```solidity
address public immutable override WETH;
```

WETHトークンのコントラクトアドレスを格納した定数。

### factory

```solidity
address public immutable override factory;
```

**Factory**コントラクトのコントラクトアドレスを格納した定数。

## 修飾子

### ensure

```solidity
modifier ensure(uint deadline) {
        require(deadline >= block.timestamp, 'UniswapV2Router: EXPIRED');
        _;
}
```

ある操作が行われる前に、与えられた期限（deadline）がまだ有効であるかどうかを確認し、期限が切れている場合には操作を拒否する役割を持つ修飾子。

1. 引数として渡された「`deadline`」（期限）を確認します。
2. もし現在のブロックのタイムスタンプ（時間）が「`deadline`」よりも後であれば、条件を満たしていないとみなします。
3. もし条件を満たしていない場合、例外を発生させて、エラーメッセージ「**UniswapV2Router: EXPIRED**」を表示します。
4. もし条件を満たしている場合、モディファイア内のコードブロック（「**_;**」）が実行されます。

# その他

## constructor

```solidity
constructor(address _factory, address _WETH) public {
        factory = _factory;
        WETH = _WETH;
}
```

**WETH**と**Factory**コントラクトのアドレスを引数で受け取り設定しています。

## receive

```solidity
receive() external payable {
        assert(msg.sender == WETH); // only accept ETH via fallback from the WETH contract
}
```

**WETH**コントラクトからのみ送信されたEtherを受け入れるための仕組みを提供します。

1. 外部から**ETH**がこのコントラクトに送られてきたときに、自動的に呼び出されます。
2. 「`assert`」文は、条件が成立しない場合に例外を発生させます。
この場合、条件は「`msg.sender == WETH`」となっています。
3. 条件式「`msg.sender == WETH`」は、Etherを送信したアドレス（送信者）が**WETH**コントラクトであるかどうかを確認しています。
4. もし送信者が**WETH**コントラクトでない場合、条件を満たさないと判断され、例外が発生してトランザクションが中止されます。
5. 送信者が**WETH**コントラクトである場合、条件を満たすと判断され、**ETH**の受け入れが許可されます。


# 関数

## READ

### getAmountIn

トークンスワップにおいて特定の出力量（`amountOut`）を得るために必要な入力量（`amountIn`）を計算する関数。

**引数**

- `amountOut`
	- トークンスワップの目標の出力トークン量。
- `reserveIn`
	- トークンペア内の入力トークンのリザーブ（保有）量。
	- トークンスワップ時に、ユーザーが渡すトークン量。
- `reserveOut`
	- トークンペア内の出力トークンのリザーブ（保有）量。
	- トークンスワップ時に、ユーザーが得たいトークン量。

### getAmountOut

特定の量の入力トークン（`amountIn`）をトークン交換でスワップした際に受け取る出力トークン（`amountOut`）の量を計算する関数。

**引数**

- `amountOut`
	- トークンスワップの目標の出力トークン量。
- `reserveIn`
	- トークンペア内の入力トークンのリザーブ（保有）量。
	- トークンスワップ時に、ユーザーが渡すトークン量。
- `reserveOut`
	- トークンペア内の出力トークンのリザーブ（保有）量。
	- トークンスワップ時に、ユーザーが得たいトークン量。

### getAmountsIn

指定されたトークンのパス内で特定の出力トークン量を得るために必要な入力トークン量を計算する関数。
スワップを行う際に目標の出力トークン量を達成するために必要な入力トークン量を把握したい場合に、この関数を使用します。

**引数**

- `amountOut`
	- 出力トークンの目標量。
	- ユーザーが得たい出力トークンの量。
- `path`
	- トークンのパス。
	- スワップを行う際のトークンのスワップ順序を示すアドレスの配列。

例えば、ユーザーが以下のようなトークンパスを持つトークンスワップを行いたいとします。

トークンパス: `[TokenA, TokenB, TokenC]`
出力トークン量: `100 TokenC`

この例では、「**TokenA**」から始まり、「**TokenB**」を経由して最終的に「**TokenC**」を得るためのトークンパスが指定されています。

戻り値である「`amounts`」配列には、各トークンが必要とする入力トークン量が含まれています。
例えば、「amounts[0]」は「TokenA」の必要な入力量、「amounts[1]」は「TokenB」の必要な入力量を示します。

### getAmountsOut

指定されたトークンのパス内で特定の入力トークン量で得られる出力トークン量を計算する関数。

**引数**

- `amountIn`
	- 入力トークン量。
	- ユーザーが渡すことができるトークン量。
- `path`
	- トークンのパス。
	- スワップを行う際のトークンのスワップ順序を示すアドレスの配列。

例えば、ユーザーが以下のようなトークンパスを持つトークンスワップを行いたいとします。

トークンパス内の各トークンペアに対する期待される出力トークン量が格納された「`amounts`」配列を返します。
これにより、トークンスワップを行う際の予測が可能となります。

### quote

UniswapV2プール内のトークンAとトークンBの現在のリザーブに基づいて、与えられた量のトークンAと引き換えに受け取ることができるトークンBの量を計算する関数。

**引数**

- `amountA`
	- 交換されるトークンAの量。
- `reserveA`
	- UniswapV2プール内のトークンAの現在のリザーブ（保有量）。
- `reserveB`
	- UniswapV2プール内のトークンBの現在のリザーブ（保有量）。

渡されたトークンAの量に対して、現在のトークンAとトークンBのリザーブを基にして、受け取ることができるトークンBの量を計算します。
計算結果である、与えられたトークンAの量に対して交換できるトークンBの量は「`amountB`」という値として返されます。


## WRITE

### addLiquidity

ユーザーが分散型取引所のトークンペアに対して流動性を追加する関数。

**引数**

- `tokenA`
	- ペア内の最初のトークンのアドレス。
- `tokenB`
	- ペア内の2番目のトークンのアドレス。
- `amountADesired`
	- 流動性として追加する希望のトークンAの量。
- `amountBDesired`
	- 流動性として追加する希望のトークンBの量。
- `amountAMin`
	- 公正な交換レートを確保するために最小限追加しなければならないトークンAの量。
- `amountBMin`
	- 公正な交換レートを確保するために最小限追加しなければならないトークンBの量。
- `to`
	- 流動性トークンが発行されるアドレス。
- `deadline`
	- 取引が有効な期限のタイムスタンプ。

**戻り値**

- `amountA`
	- 実際に流動性として追加されたトークンAの量。
- `amountB`
	- 実際に流動性として追加されたトークンBの量。
- `liquidity`
	- 発行された流動性トークンの量。

希望のトークンAとトークンBの量を指定して流動性を提供します。
また、公正な交換レートを確保するために最小限追加しなければならないトークンAとトークンBの最小量も指定します。
流動性トークンは発行されて指定されたアドレスに送信されます。
重要な点として、指定された期限までに取引を実行しなければならないことです。
期限を過ぎると取引は無効とされます。

1. `_addLiquidity`関数を呼び出して、指定されたトークンAとトークンBの流動性を追加します
2. ペアのアドレスを計算して取得します。
3. ユーザーから指定されたトークンAとトークンBをトークンペアに送信します。
4. トークンペアに流動性を追加し、追加された流動性トークンの数量を取得します。

:::message
`amountAMin`と`amountBMin`が活用される場面としては以下が挙げられます。
**価格変動によるスリッページの防止**
トークンの価格は変動するため、ユーザーが指定した量のトークンAを提供しても、実際に受け取るトークンBの量が期待と異なる場合があります。
例えば、トークンAの価格が急激に変動した場合、取引が予想よりも不利な条件で行われる可能性があります。
こうしたスリッページを防ぐために、「`amountAMin`」を設定し、一定のレベルの保護を確保できます。

**取引の信頼性確保**
「`amountAMin`」を適切に設定することで、他のトランザクションや価格の変動によってもたらされる不利な状況に対処できます。
ユーザーは最小限の取引条件を指定することで、意図した通りの交換レートで取引が行われることを確実にすることができます。

**攻撃からの保護**
悪意あるユーザーや攻撃者が意図的に取引価格を歪めようとする場合、最小限の取引条件を設定することで、その影響を軽減することができます。
:::

### addLiquidityETH

トークンと最小限のETHを提供して、トークンペアに流動性を追加する関数。

**引数**

- `token`
	- 流動性を追加するトークンのアドレス。
- `amountTokenDesired`
	- 流動性として追加する希望のトークン量。
- `amountTokenMin`
	- 最小限に追加するトークンの数量。
- `amountETHMin`
	- 最小限に追加するETHの数量。
- `to`
	- 流動性トークンを受け取るアドレス
- `deadline`
	- 取引が有効な期限のタイムスタンプ。

**戻り値**

- `amountToken`
	- 実際に追加されたトークンの数量。
- `amountETH`
	- 実際に追加されたETHの数量。
- `liquidity`
	- 発行された流動性トークンの量。

希望するトークンの数量と最小限のETHを提供することで、トークンペアに流動性を追加できます。
ユーザーが提供した希望数量と最小数量に基づいて、流動性プールの現在のリザーブ（保有量）に基づいて実際のトークンとETHの数量を計算します。
対応する量の流動性トークンをユーザーが指定したアドレスにMintします。

1. `_addLiquidity`関数を呼び出して、指定されたトークンとETHを流動性に追加します。
2. ペアのアドレスを計算して取得します。
3. ユーザーから指定されたトークンをトークンペアに送信します。
4. ETHをWETHに変換して、トークンペアに送信します。
5. トークンペアに流動性を追加し、追加された流動性トークンの数量を取得します。
6. もし余分なETHがある場合、ユーザーに返金します。

### removeLiquidity

リザーブプールから流動性を取り出す関数。

**引数**

- `tokenA`
	- プール内の片方のトークンのアドレス。
- `tokenB`
	- プール内のもう片方のトークンのアドレス。
- `liquidity`
	- 取り出す流動性の量。
- `amountAMin`
	- 受け取りたい最小のトークンAの量。
- `amountBMin`
	- 受け取りたい最小のトークンBの量。
- `to`
	- トークンの送付先アドレス。
- `deadline`
	- 取引が有効な期限のタイムスタンプ。

1. 実行期限が過ぎているかどうかをチェックし、もし期限が過ぎている場合はトランザクションを巻き戻します。
2. ペアのアドレスを計算して取得します。
3. ユーザーから指定された流動性をトークンペアに送信します。
4. トークンペアからトークンAとトークンBを取り出し、その数量を取得します。
5. トークンAとトークンBのアドレスをソートして取得します。
6. 正しいトークンの数量を設定します。
7. 取り出すトークンの数量がユーザーが指定した最低限の条件を満たしているかをチェックします。
もし条件を満たしていない場合、トランザクションは失敗します。
8. 条件を満たしている場合、取り出したトークンの数量とともに処理が終了します。

### removeLiquidityETH

ETHとトークンを指定して流動性を取り除く関数。

**引数**

- `token`
	- 流動性を削除するトークンのアドレス。
- `liquidity`
	- 削除する流動性トークンの数量。
- `amountTokenMin`
	- 受け取りたい最小限のトークンの数量。
- `amountETHMin`
	- 受け取りたい最小限のETHの数量。
- `to`
	- トークンとETHを受け取るアドレス。
- `deadline`
	- 取引が有効な期限のタイムスタンプ。

1. 削除された流動性トークンから受け取る予定のトークンの数量とETHの数量を取得します。
2. 受け取りたい最小限のトークンの数量とETHの数量を確認します。
もし条件を満たさない場合、トランザクションは失敗します。
3. 削除されたトークンを指定したアドレス（`to`）に送信します。
4. 削除されたWETHをETHに変換して取得します。
5. 取得したETHを指定したアドレス（`to`）に送信します。

### removeLiquidityETHWithPermit

ERC20トークンとETHの流動性を削除する操作をコントラクトに許可して実行する関数。
ユーザーは別のアドレスに操作許可を与えつつ指定したトークンとETHを使って流動性を削除し、受け取るトークンとETHの数量を制御できます。

**引数**

- `token`
	- 流動性を削除するトークンのアドレス。
- `liquidity`
	- 削除する流動性トークンの数量。
- `amountTokenMin`
	- 受け取りたい最小限のトークンの数量。
- `amountETHMin`
	- 受け取りたい最小限のETHの数量。
- `to`
	- トークンとETHを受け取るアドレス。
- `deadline`
	- 取引が有効な期限のタイムスタンプ。
- `approveMax`
	- 最大値を承認するかどうかのフラグ。
- `v`, `r`, `s`
	- 許可を与えるための署名関連のパラメータ。

1. 指定されたトークンとWETHのペアのアドレスを取得します。
2. `approveMax`が`true`の場合、他のアドレスに`transfer`許可するトークンの数量を最大値（無限大）に設定します。
そうでない場合、指定された流動性トークンの数量を設定します。
3. `IUniswapV2Pairのpermit`関数を呼び出して、署名検証後スマートコントラクトにユーザーのトークンを操作する許可を与えます。
4. `removeLiquidityETH`関数を呼び出して、指定されたトークンとETHの流動性を削除します。
5. 削除されたトークンとETHの数量を取得します。

### removeLiquidityWithPermit

ERC20トークンの流動性を削除する操作をコントラクトに許可して実行する関数。

**引数**

- `tokenA`
	- 流動性を削除するトークンAのアドレス。
- `tokenB`
	- 流動性を削除するトークンBのアドレス。
- `liquidity`
	- 削除する流動性トークンの数量。
- `amountAMin`
	- 受け取りたい最小限のトークンAの数量。
- `amountBMin`
	- 受け取りたい最小限のトークンBの数量。
- `to`
	- トークンを受け取るアドレス。
- `deadline`
	- 取引が有効な期限のタイムスタンプ。
- `approveMax`
	- 最大値を承認するかどうかのフラグ。
- `v`, `r`, `s`
	- 許可を与えるための署名関連のパラメータ。

1. `pair`コントラクトのアドレスを取得し、指定されたトークンAとトークンBのペアを特定します。
2. `approveMax`の値に基づいて、トークンの数量を許可するかどうかを決定し、それに応じて `value`変数を設定します。
3. **IUniswapV2Pair(pair)** インターフェースを使用して、ユーザーの許可を特定のトークンに与える`permit`関数を呼び出します。
これにより、ユーザーはトークンの流動性を削除する操作をコントラクトに許可します。
4. `removeLiquidity`関数を呼び出して、実際に流動性を削除します。
5. これにより、トークンAとトークンBを取得してトークンAとトークンBの数量を返します。

### swapETHForExactTokens

特定の量のETHを一定の量のトークンに交換するための関数。

**引数**

- `amountOut`
	- ユーザーがスワップで受け取りたいトークン量。
- `path`
	- トークンスワップのパスを示すアドレスの配列。
	- 配列の最初のアドレスは受け取るトークンであり、最後のアドレスは提供するトークン。
- `to`
	- 交換されたトークンが送信されるアドレス。
- `deadline`
	- 取引が有効な期限のタイムスタンプ。

トークンスワップを処理する外部のコントラクトやサービスを呼び出してスワップを実行します。

### swapExactETHForTokens

特定の量のETHを使用して、最小限のトークン量にスワップする関数。

**引数**

- `amountOutMin`
	- スワップで受け取ると予想している最小のトークン量。
- `path`
	- トークンスワップのパスを示すアドレスの配列。
	- 配列の最初のアドレスは受け取るトークンであり、最後のアドレスはETHと交換するためのトークンのアドレス。
- `to`
	- 交換されたトークンが送信されるアドレス。
- `deadline`
	- 取引が有効な期限のタイムスタンプ。

:::message
`amountOutMin`でユーザーが最低限受け取りたいトークンの量を指定します。
:::

### swapExactTokensForETH

特定の量のトークンをETHにスワップするための関数。
特定の量のトークンを受け取り、最小限のETHを期待通りに受け取るための手順を提供します。

**引数**

- `amountIn`
	- 交換したいトークン量。
- `amountOutMin`
        - スワップで受け取ると予想している最小のETH量。
- `path`
        - トークンスワップのパスを示すアドレスの配列。
        - 配列の最初のアドレスは受け取るトークンであり、最後のアドレスはETHと交換するためのトークンのアドレス。
- `to`
        - 交換されたトークンが送信されるアドレス。
- `deadline`
        - 取引が有効な期限のタイムスタンプ。

1. パスの最後のアドレスがWETH（ETHのラップトークン）であることを確認します。
2. トークンスワップのためのトークンパスに基づいて、ユーザーが指定したトークン量から計算されたETHの予想収益を算出します。
3. `amountOutMin`で指定した最小のETH収益を確認し、その収益を期待通りに受け取るためのトークン量が十分であることを確認します。
4. `path[0]`のトークンをユーザーから受け取り、トークンをスワップするためのペアに送信します。
5. `_swap`関数を呼び出してトークンスワップを実行します。
6. `to`で指定したアドレスへトークンを送る。
7. **4**~**6**の操作を`path`配列分実行する。
9. **WETH**をトークンに戻すために、**WETH**を`withdraw`し、そのETHを指定したアドレス `to`に送信します。

### swapExactTokensForTokens

特定の量の入力トークンを期待する最小の出力トークン量に交換する関数。
特定の量の入力トークンを最小の出力トークン収益に交換するための手順を提供します。

**引数**

- `amountIn`
	- 交換したいトークン量。
- `amountOutMin`
        - スワップで受け取ると予想している最小のトークン量。
- `path`
        - トークンスワップのパスを示すアドレスの配列。
        - 配列の最初のアドレスは受け取るトークンであり、最後のアドレスはトークンと交換するためのトークンのアドレス。
- `to`
        - 交換されたトークンが送信されるアドレス。
- `deadline`
        - 取引が有効な期限のタイムスタンプ。

1. トークンスワップのためのトークンパスに基づいて、ユーザーが指定した入力トークン量から計算された出力トークンの予想収益を算出します。
2. `amountOutMin`で指定した最小の出力トークン収益を確認し、その収益を期待通りに受け取るためのトークン量が十分であることを確認します。
3. `path[0]`のトークンをユーザーから受け取り、トークンをスワップするためのペアに送信します。
4. `_swap`関数を呼び出してトークンスワップを実行します。
5. `to`で指定したアドレスへトークンを送る。
6. **3**~**5**の操作を`path`配列分実行する。
### swapTokensForExactETH

受け取りたいETHの正確な量を指定してトークンとスワップする関数。
スワップで受け取るETHの量が正確に指定されていて、最大の入力トークン量を指定しその範囲内でトークンを交換します。

**引数**

- `amountOut`
	- スワップで受け取りたいETHの量。
- `amountInMax`
        - スワップする最大のトークン量。
- `path`
        - トークンスワップのパスを示すアドレスの配列。
        - 配列の最初のアドレスは受け取るトークンであり、最後のアドレスはETHと交換するためのトークンのアドレス。
- `to`
        - スワップされたETHが送信されるアドレス。
- `deadline`
        - 取引が有効な期限のタイムスタンプ。

:::message
`swapTokensForExactETH`と`swapExactTokensForETH`の違いは以下になります。

**swapTokensForExactETH**

- ユーザーが受け取りたいETHの正確な量を指定して、その量に合わせてトークンを交換します。
- ユーザーがスワップで受け取るETHの量が正確に指定されます。
- ユーザーは最大の入力トークン量を指定し、その範囲内でトークンを交換します。
- パスにはトークンからETHへのパスを指定します。

**swapExactTokensForETH**

- ユーザーが交換する正確なトークン量を指定して、そのトークン量に対応するETHの最小受け取り量を指定します。
- ユーザーがスワップで受け取るETHの最小量が指定されます。
- 実際に受け取るETHの量は最小値以上になります。
- ユーザーは交換するトークンの量を正確に指定します。
- パスにはトークンからETHへのパスを指定します。
:::

### swapTokensForExactTokens

指定されたトークン量を受け取るために、最大でどれだけのトークンを使用するかを指定してトークンスワップを行う関数。

**引数**

- `amountOut`
	- スワップで受け取りたいトークンの量。
- `amountInMax`
        - スワップする最大のトークン量。
- `path`
        - トークンスワップのパスを示すアドレスの配列。
        - 配列の最初のアドレスは受け取るトークンであり、最後のアドレスはトークンと交換するためのトークンのアドレス。
- `to`
        - スワップされたトークンが送信されるアドレス。
- `deadline`
        - 取引が有効な期限のタイムスタンプ。

1. `amounts`配列を計算
`UniswapV2Library.getAmountsIn`関数を使用して、指定された`amountOut`と`path`に基づいて、必要なトークン量の配列`amounts`を計算します。
この配列には、各トークンスワップステップごとに必要なトークン量が含まれています。
2. 最大入力制限の確認
`amounts[0]`が`amountInMax`以下であることを確認し、指定された最大入力量を超えないようにします。
3. トークン送信
ユーザーから最初のトークンを`path`の最初のトークンへ送信します。
これにより、スワップの準備が整います。
4. `_swap`関数の呼び出し
`_swap`関数を呼び出して、実際のトークンスワップを実行します。
これにより、指定されたトークン量をスワップして、受け取るべき正確なトークン量が確保されます。
5. スワップされたトークンの受け取り
スワップされたトークンが`to`アドレスに送信されます。
6. **3**~**5**の動作を繰り返す。

# イベント

特になし

# コード

## IUniswapV2Factory

:::details IUniswapV2Factory
```solidity
// File: @uniswap/v2-core/contracts/interfaces/IUniswapV2Factory.sol

pragma solidity >=0.5.0;

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
```
:::

## TransferHelper

:::details TransferHelper
```solidity
// File: @uniswap/lib/contracts/libraries/TransferHelper.sol

pragma solidity >=0.6.0;

// helper methods for interacting with ERC20 tokens and sending ETH that do not consistently return true/false
library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}
```
:::

## IUniswapV2Pair
:::details IUniswapV2Pair
```solidity
// File: @uniswap/v2-core/contracts/interfaces/IUniswapV2Pair.sol

pragma solidity >=0.5.0;

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}
```
:::

## SafeMath

:::details SafeMath
```solidity
// File: contracts/libraries/SafeMath.sol

pragma solidity =0.6.6;

// a library for performing overflow-safe math, courtesy of DappHub (https://github.com/dapphub/ds-math)

library SafeMath {
    function add(uint x, uint y) internal pure returns (uint z) {
        require((z = x + y) >= x, 'ds-math-add-overflow');
    }

    function sub(uint x, uint y) internal pure returns (uint z) {
        require((z = x - y) <= x, 'ds-math-sub-underflow');
    }

    function mul(uint x, uint y) internal pure returns (uint z) {
        require(y == 0 || (z = x * y) / y == x, 'ds-math-mul-overflow');
    }
}
```
:::

## UniswapV2Library

:::details UniswapV2Library
```solidity
// File: contracts/libraries/UniswapV2Library.sol

pragma solidity >=0.5.0;



library UniswapV2Library {
    using SafeMath for uint;

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'UniswapV2Library: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'UniswapV2Library: ZERO_ADDRESS');
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f' // init code hash
            ))));
    }

    // fetches and sorts the reserves for a pair
    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IUniswapV2Pair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }

    // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB) {
        require(amountA > 0, 'UniswapV2Library: INSUFFICIENT_AMOUNT');
        require(reserveA > 0 && reserveB > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        amountB = amountA.mul(reserveB) / reserveA;
    }

    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        require(amountIn > 0, 'UniswapV2Library: INSUFFICIENT_INPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        uint amountInWithFee = amountIn.mul(997);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(1000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }

    // given an output amount of an asset and pair reserves, returns a required input amount of the other asset
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) internal pure returns (uint amountIn) {
        require(amountOut > 0, 'UniswapV2Library: INSUFFICIENT_OUTPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        uint numerator = reserveIn.mul(amountOut).mul(1000);
        uint denominator = reserveOut.sub(amountOut).mul(997);
        amountIn = (numerator / denominator).add(1);
    }

    // performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(address factory, uint amountIn, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'UniswapV2Library: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[0] = amountIn;
        for (uint i; i < path.length - 1; i++) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i], path[i + 1]);
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }

    // performs chained getAmountIn calculations on any number of pairs
    function getAmountsIn(address factory, uint amountOut, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'UniswapV2Library: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[amounts.length - 1] = amountOut;
        for (uint i = path.length - 1; i > 0; i--) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i - 1], path[i]);
            amounts[i - 1] = getAmountIn(amounts[i], reserveIn, reserveOut);
        }
    }
}
```
:::

## IUniswapV2Router01

:::details IUniswapV2Router01
```solidity
// File: contracts/interfaces/IUniswapV2Router01.sol

pragma solidity >=0.6.2;

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}
```
:::

## IERC20

:::details IERC20
```solidity
// File: contracts/interfaces/IERC20.sol

pragma solidity >=0.5.0;

interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
}
```
:::

## IWETH
:::details IWETH
```solidity
// File: contracts/interfaces/IWETH.sol

pragma solidity >=0.5.0;

interface IWETH {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}
```
:::

## UniswapV2Router01
:::details UniswapV2Router01
```solidity
// File: contracts/UniswapV2Router01.sol

pragma solidity =0.6.6;

// File: @uniswap/v2-core/contracts/interfaces/IUniswapV2Factory.sol

pragma solidity >=0.5.0;

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

// File: @uniswap/lib/contracts/libraries/TransferHelper.sol

pragma solidity >=0.6.0;

// helper methods for interacting with ERC20 tokens and sending ETH that do not consistently return true/false
library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}

// File: @uniswap/v2-core/contracts/interfaces/IUniswapV2Pair.sol

pragma solidity >=0.5.0;

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

// File: contracts/libraries/SafeMath.sol

pragma solidity =0.6.6;

// a library for performing overflow-safe math, courtesy of DappHub (https://github.com/dapphub/ds-math)

library SafeMath {
    function add(uint x, uint y) internal pure returns (uint z) {
        require((z = x + y) >= x, 'ds-math-add-overflow');
    }

    function sub(uint x, uint y) internal pure returns (uint z) {
        require((z = x - y) <= x, 'ds-math-sub-underflow');
    }

    function mul(uint x, uint y) internal pure returns (uint z) {
        require(y == 0 || (z = x * y) / y == x, 'ds-math-mul-overflow');
    }
}

// File: contracts/libraries/UniswapV2Library.sol

pragma solidity >=0.5.0;



library UniswapV2Library {
    using SafeMath for uint;

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'UniswapV2Library: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'UniswapV2Library: ZERO_ADDRESS');
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f' // init code hash
            ))));
    }

    // fetches and sorts the reserves for a pair
    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IUniswapV2Pair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }

    // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB) {
        require(amountA > 0, 'UniswapV2Library: INSUFFICIENT_AMOUNT');
        require(reserveA > 0 && reserveB > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        amountB = amountA.mul(reserveB) / reserveA;
    }

    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        require(amountIn > 0, 'UniswapV2Library: INSUFFICIENT_INPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        uint amountInWithFee = amountIn.mul(997);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(1000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }

    // given an output amount of an asset and pair reserves, returns a required input amount of the other asset
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) internal pure returns (uint amountIn) {
        require(amountOut > 0, 'UniswapV2Library: INSUFFICIENT_OUTPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        uint numerator = reserveIn.mul(amountOut).mul(1000);
        uint denominator = reserveOut.sub(amountOut).mul(997);
        amountIn = (numerator / denominator).add(1);
    }

    // performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(address factory, uint amountIn, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'UniswapV2Library: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[0] = amountIn;
        for (uint i; i < path.length - 1; i++) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i], path[i + 1]);
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }

    // performs chained getAmountIn calculations on any number of pairs
    function getAmountsIn(address factory, uint amountOut, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'UniswapV2Library: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[amounts.length - 1] = amountOut;
        for (uint i = path.length - 1; i > 0; i--) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i - 1], path[i]);
            amounts[i - 1] = getAmountIn(amounts[i], reserveIn, reserveOut);
        }
    }
}

// File: contracts/interfaces/IUniswapV2Router01.sol

pragma solidity >=0.6.2;

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

// File: contracts/interfaces/IERC20.sol

pragma solidity >=0.5.0;

interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
}

// File: contracts/interfaces/IWETH.sol

pragma solidity >=0.5.0;

interface IWETH {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}

// File: contracts/UniswapV2Router01.sol

pragma solidity =0.6.6;







contract UniswapV2Router01 is IUniswapV2Router01 {
    address public immutable override factory;
    address public immutable override WETH;

    modifier ensure(uint deadline) {
        require(deadline >= block.timestamp, 'UniswapV2Router: EXPIRED');
        _;
    }

    constructor(address _factory, address _WETH) public {
        factory = _factory;
        WETH = _WETH;
    }

    receive() external payable {
        assert(msg.sender == WETH); // only accept ETH via fallback from the WETH contract
    }

    // **** ADD LIQUIDITY ****
    function _addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin
    ) private returns (uint amountA, uint amountB) {
        // create the pair if it doesn't exist yet
        if (IUniswapV2Factory(factory).getPair(tokenA, tokenB) == address(0)) {
            IUniswapV2Factory(factory).createPair(tokenA, tokenB);
        }
        (uint reserveA, uint reserveB) = UniswapV2Library.getReserves(factory, tokenA, tokenB);
        if (reserveA == 0 && reserveB == 0) {
            (amountA, amountB) = (amountADesired, amountBDesired);
        } else {
            uint amountBOptimal = UniswapV2Library.quote(amountADesired, reserveA, reserveB);
            if (amountBOptimal <= amountBDesired) {
                require(amountBOptimal >= amountBMin, 'UniswapV2Router: INSUFFICIENT_B_AMOUNT');
                (amountA, amountB) = (amountADesired, amountBOptimal);
            } else {
                uint amountAOptimal = UniswapV2Library.quote(amountBDesired, reserveB, reserveA);
                assert(amountAOptimal <= amountADesired);
                require(amountAOptimal >= amountAMin, 'UniswapV2Router: INSUFFICIENT_A_AMOUNT');
                (amountA, amountB) = (amountAOptimal, amountBDesired);
            }
        }
    }
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external override ensure(deadline) returns (uint amountA, uint amountB, uint liquidity) {
        (amountA, amountB) = _addLiquidity(tokenA, tokenB, amountADesired, amountBDesired, amountAMin, amountBMin);
        address pair = UniswapV2Library.pairFor(factory, tokenA, tokenB);
        TransferHelper.safeTransferFrom(tokenA, msg.sender, pair, amountA);
        TransferHelper.safeTransferFrom(tokenB, msg.sender, pair, amountB);
        liquidity = IUniswapV2Pair(pair).mint(to);
    }
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external override payable ensure(deadline) returns (uint amountToken, uint amountETH, uint liquidity) {
        (amountToken, amountETH) = _addLiquidity(
            token,
            WETH,
            amountTokenDesired,
            msg.value,
            amountTokenMin,
            amountETHMin
        );
        address pair = UniswapV2Library.pairFor(factory, token, WETH);
        TransferHelper.safeTransferFrom(token, msg.sender, pair, amountToken);
        IWETH(WETH).deposit{value: amountETH}();
        assert(IWETH(WETH).transfer(pair, amountETH));
        liquidity = IUniswapV2Pair(pair).mint(to);
        if (msg.value > amountETH) TransferHelper.safeTransferETH(msg.sender, msg.value - amountETH); // refund dust eth, if any
    }

    // **** REMOVE LIQUIDITY ****
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) public override ensure(deadline) returns (uint amountA, uint amountB) {
        address pair = UniswapV2Library.pairFor(factory, tokenA, tokenB);
        IUniswapV2Pair(pair).transferFrom(msg.sender, pair, liquidity); // send liquidity to pair
        (uint amount0, uint amount1) = IUniswapV2Pair(pair).burn(to);
        (address token0,) = UniswapV2Library.sortTokens(tokenA, tokenB);
        (amountA, amountB) = tokenA == token0 ? (amount0, amount1) : (amount1, amount0);
        require(amountA >= amountAMin, 'UniswapV2Router: INSUFFICIENT_A_AMOUNT');
        require(amountB >= amountBMin, 'UniswapV2Router: INSUFFICIENT_B_AMOUNT');
    }
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) public override ensure(deadline) returns (uint amountToken, uint amountETH) {
        (amountToken, amountETH) = removeLiquidity(
            token,
            WETH,
            liquidity,
            amountTokenMin,
            amountETHMin,
            address(this),
            deadline
        );
        TransferHelper.safeTransfer(token, to, amountToken);
        IWETH(WETH).withdraw(amountETH);
        TransferHelper.safeTransferETH(to, amountETH);
    }
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external override returns (uint amountA, uint amountB) {
        address pair = UniswapV2Library.pairFor(factory, tokenA, tokenB);
        uint value = approveMax ? uint(-1) : liquidity;
        IUniswapV2Pair(pair).permit(msg.sender, address(this), value, deadline, v, r, s);
        (amountA, amountB) = removeLiquidity(tokenA, tokenB, liquidity, amountAMin, amountBMin, to, deadline);
    }
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external override returns (uint amountToken, uint amountETH) {
        address pair = UniswapV2Library.pairFor(factory, token, WETH);
        uint value = approveMax ? uint(-1) : liquidity;
        IUniswapV2Pair(pair).permit(msg.sender, address(this), value, deadline, v, r, s);
        (amountToken, amountETH) = removeLiquidityETH(token, liquidity, amountTokenMin, amountETHMin, to, deadline);
    }

    // **** SWAP ****
    // requires the initial amount to have already been sent to the first pair
    function _swap(uint[] memory amounts, address[] memory path, address _to) private {
        for (uint i; i < path.length - 1; i++) {
            (address input, address output) = (path[i], path[i + 1]);
            (address token0,) = UniswapV2Library.sortTokens(input, output);
            uint amountOut = amounts[i + 1];
            (uint amount0Out, uint amount1Out) = input == token0 ? (uint(0), amountOut) : (amountOut, uint(0));
            address to = i < path.length - 2 ? UniswapV2Library.pairFor(factory, output, path[i + 2]) : _to;
            IUniswapV2Pair(UniswapV2Library.pairFor(factory, input, output)).swap(amount0Out, amount1Out, to, new bytes(0));
        }
    }
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external override ensure(deadline) returns (uint[] memory amounts) {
        amounts = UniswapV2Library.getAmountsOut(factory, amountIn, path);
        require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        TransferHelper.safeTransferFrom(path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]);
        _swap(amounts, path, to);
    }
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external override ensure(deadline) returns (uint[] memory amounts) {
        amounts = UniswapV2Library.getAmountsIn(factory, amountOut, path);
        require(amounts[0] <= amountInMax, 'UniswapV2Router: EXCESSIVE_INPUT_AMOUNT');
        TransferHelper.safeTransferFrom(path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]);
        _swap(amounts, path, to);
    }
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        override
        payable
        ensure(deadline)
        returns (uint[] memory amounts)
    {
        require(path[0] == WETH, 'UniswapV2Router: INVALID_PATH');
        amounts = UniswapV2Library.getAmountsOut(factory, msg.value, path);
        require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        IWETH(WETH).deposit{value: amounts[0]}();
        assert(IWETH(WETH).transfer(UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]));
        _swap(amounts, path, to);
    }
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        override
        ensure(deadline)
        returns (uint[] memory amounts)
    {
        require(path[path.length - 1] == WETH, 'UniswapV2Router: INVALID_PATH');
        amounts = UniswapV2Library.getAmountsIn(factory, amountOut, path);
        require(amounts[0] <= amountInMax, 'UniswapV2Router: EXCESSIVE_INPUT_AMOUNT');
        TransferHelper.safeTransferFrom(path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]);
        _swap(amounts, path, address(this));
        IWETH(WETH).withdraw(amounts[amounts.length - 1]);
        TransferHelper.safeTransferETH(to, amounts[amounts.length - 1]);
    }
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        override
        ensure(deadline)
        returns (uint[] memory amounts)
    {
        require(path[path.length - 1] == WETH, 'UniswapV2Router: INVALID_PATH');
        amounts = UniswapV2Library.getAmountsOut(factory, amountIn, path);
        require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        TransferHelper.safeTransferFrom(path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]);
        _swap(amounts, path, address(this));
        IWETH(WETH).withdraw(amounts[amounts.length - 1]);
        TransferHelper.safeTransferETH(to, amounts[amounts.length - 1]);
    }
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        override
        payable
        ensure(deadline)
        returns (uint[] memory amounts)
    {
        require(path[0] == WETH, 'UniswapV2Router: INVALID_PATH');
        amounts = UniswapV2Library.getAmountsIn(factory, amountOut, path);
        require(amounts[0] <= msg.value, 'UniswapV2Router: EXCESSIVE_INPUT_AMOUNT');
        IWETH(WETH).deposit{value: amounts[0]}();
        assert(IWETH(WETH).transfer(UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]));
        _swap(amounts, path, to);
        if (msg.value > amounts[0]) TransferHelper.safeTransferETH(msg.sender, msg.value - amounts[0]); // refund dust eth, if any
    }

    function quote(uint amountA, uint reserveA, uint reserveB) public pure override returns (uint amountB) {
        return UniswapV2Library.quote(amountA, reserveA, reserveB);
    }

    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) public pure override returns (uint amountOut) {
        return UniswapV2Library.getAmountOut(amountIn, reserveIn, reserveOut);
    }

    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) public pure override returns (uint amountIn) {
        return UniswapV2Library.getAmountOut(amountOut, reserveIn, reserveOut);
    }

    function getAmountsOut(uint amountIn, address[] memory path) public view override returns (uint[] memory amounts) {
        return UniswapV2Library.getAmountsOut(factory, amountIn, path);
    }

    function getAmountsIn(uint amountOut, address[] memory path) public view override returns (uint[] memory amounts) {
        return UniswapV2Library.getAmountsIn(factory, amountOut, path);
    }
}
```
:::

# 最後に

今回の記事では、Bunzzの新機能『DeCipher』を使用して、UniswapV2の「**UniswapV2Router01**」のコントラクトを見てきました。
いかがだったでしょうか？
今後も特定のNFTやコントラクトをピックアップしてまとめて行きたいと思います。

普段はブログやQiitaでブロックチェーンやAIに関する記事を挙げているので、よければ見ていってください！

https://chaldene.net/

https://qiita.com/cardene
