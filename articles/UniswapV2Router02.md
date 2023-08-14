---
title: "[Bunzz Decipher] UniswapV2Router02のコントラクトを理解しよう！"
emoji: "🦄"
type: "tech"
topics:
  - "blockchain"
  - "ブロックチェーン"
  - "solidity"
  - "ethereum"
  - "defi"
published: true
published_at: "2023-08-14 07:03"
publication_name: "cryptogames"
---

# はじめに

初めまして。
**CryptoGames**というブロックチェーンゲーム企業でエンジニアをしている **cardene（かるでね）** です！
スマートコントラクトを書いたり、フロントエンド・バックエンド・インフラと幅広く触れています。

https://cryptogames.co.jp/

代表的なゲームは**クリプトスペルズ**というブロックチェーンゲームです。

https://cryptospells.jp/

今回はBunzzの新機能『**DeCipher**』を使用して、UniswapV2の「**UniswapV2Router02**」のコントラクトを見てみようと思います。

『**DeCipher**』はAIを使用してコントラクトのドキュメントを自動生成してくれるサービスです。

https://www.bunzz.dev/decipher

詳しい使い方に関しては以下の記事を参考にしてください！

https://zenn.dev/heku/articles/33266f0c19d523

:::message
この記事はあくまで個人が「**DeCipher**」を使用してみての記事になります。
間違った部分などもあるかと思いますが、その際はコメントなどで教えてください。
:::

今回使用する『**DeCipher**』のリンクは以下になります。

https://app.bunzz.dev/decipher/chains/1/addresses/0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D

# 概要

## 要約
**UniswapV2Router02**コントラクトは、Ethereumブロックチェーン上に構築された分散型取引所（DEX）である**Uniswap**プロトコルの重要なコンポーネントです。
このコントラクトは、**Uniswap**エコシステム内での流動性の追加と削除、トークンのスワップを容易にする役割を果たしています。

## コントラクトの目的
**UniswapV2Router02**コントラクトの主な目的は、**Uniswap**プロトコルとのシームレスな対話を可能にすることです。
ユーザーが流動性プールに流動性を追加または削除し、Ethereumブロックチェーン上でトークンをスワップするための一連の機能を提供しています。

このコントラクトは、**WETH**（Wrapped Ether）コントラクトとも連携しており、これは**ERC20**標準に準拠したEtherのトークン化バージョンです。
これにより、他の**ERC20**トークンを期待するコントラクトとの互換性が高まります。

## コントラクトの責任

### 流動性の管理
このコントラクトは、流動性を追加および削除するための機能を提供しています。
`addLiquidity`および`addLiquidityETH`関数は、ユーザーがトークンのペアを流動性プールに預け入れ、代わりに流動性トークンを受け取ることを可能にします。
これらの流動性トークンは、ユーザーのプール内でのシェアを表します。

`removeLiquidity`関数は、ユーザーが自身の流動性トークンを燃やしてプール内のトークンのシェアを引き出すことを可能にします。
これは、ユーザーがポジションを解消したい場合に役立ちます。

### トークンのスワップ
`swap`関数は、ユーザーがEthereumブロックチェーン上でトークンを直接交換することを可能にします。
これが**Uniswap**プロトコルの主要な機能であり、分散型取引所としての機能を提供します。

### WETHとの連携
このコントラクトは、**WETH**コントラクトと連携して**Ether**の預け入れと引き出しを行うことができるようになっています。
`deposit`関数を使用してユーザーは**Ether**を**WETH**に変換し、`withdraw`関数を使用して**WETH**を再度**Ether**に変換することができます。

### コントラクトの初期化
`initialize`関数は、コントラクトが初めてデプロイされる際に設定を行うために使用されます。
この関数は、**Factory**コントラクトと**WETH**コントラクトのアドレスを設定し、これらのアドレスはコントラクトの多くの機能で使用されます。

:::message
**Factory**コントラクトについては以下を参照してください。
:::

https://zenn.dev/cryptogames/articles/89744d2e9629f4

### 安全対策
このコントラクトには、**WETH**コントラクトから送信された**Ether**のみを受け付ける`receive`関数が含まれています。
これは、コントラクトが処理できない方法で**Ether**が送信されるのを防ぐための安全対策です。

コントラクトには、トランザクションの締め切りが過ぎていないことを確認する`ensure`修飾子も含まれています。
これは、トランザクションが期限を過ぎた後にブロックに含まれるのを防ぐために使用されます。

### まとめ
**UniswapV2Router02**コントラクトは、**Uniswap**プロトコルの重要な部分であり、ユーザーがプロトコルと対話するために必要な機能を提供しています。
流動性の追加と削除、トークンのスワップ、**WETH**コントラクトとの連携を処理し、プロトコルの安全な運用を確保するための安全対策も含まれています。

# 使い方

**UniswapV2Router02**コントラクトは、Ethereum上の自動流動性プロトコルである**Uniswap**プロトコルの一部です。
このコントラクトは、流動性の追加と削除、トークンのスワップ、およびトークンペアに関する情報のクエリ機能を提供します。

**UniswapV2Router0**2コントラクトを使用する手順は以下の通りです。

### 1. コントラクトのデプロイ
**UniswapV2Router02**コントラクトをEthereumネットワーク上にデプロイします。
コンストラクタには**UniswapV2Factory**コントラクトと**WETH**コントラクトのアドレスが必要です。

### 2. 流動性の追加

`addLiquidity`または`addLiquidityETH`関数を呼び出して、ペアに流動性を追加します。
- `addLiquidity`関数は、2つのトークンのアドレス、各トークンの希望する量と最小量、流動性トークンを受け取るアドレス、および期限が必要です。
- `addLiquidityETH`関数は似ていますが、トークンの1つがEthereumの場合に使用されます。

:::message
**Pair**コントラクトについては以下を参照してください。
:::

https://zenn.dev/cryptogames/articles/dde2314cb2417b

### 3. 流動性の削除
`removeLiquidity`関数を呼び出して、ペアから流動性を削除します。
この関数には、2つのトークンのアドレス、削除する流動性の量、各トークンを受け取る最小量、トークンを受け取るアドレス、および処理の有効期限が必要です。

### 4. トークンのスワップ
`swap`関数を呼び出して、トークンをスワップします。
この関数には、スワップする各トークンの量、トークンを受け取るアドレス、および追加データが必要です。

### 5. 情報のクエリ
`getReserves`関数を呼び出して、ペアのリザーブバランスを取得します。
`price0CumulativeLast`および`price1CumulativeLast`関数を呼び出して、ペア内のトークンの最後の累積価格を取得します。
`kLast`関数を呼び出して、ペアの最後のリザーブバランスの積を取得します。

### 6. その他の関数
- `mint`関数は、流動性トークンを発行するために使用されます。

- `burn`関数は、流動性トークンを焼却するために使用されます。

- `skim`関数は、コントラクトに誤って送信されたトークンを削除するために使用されます。

- `sync`関数は、ペアのリザーブバランスを更新するために使用されます。

すべてのコントラクトの状態を変更する関数は、トランザクションがEthereumアカウントから送信される必要があります。
アカウントにはトランザクションのガス手数料を支払うための十分な残高が必要です。

# パラメータ

## _factory

**UniswapV2Factory**コントラクトのアドレス。
新しいペアを作成したり既存のペアを取得するために使用されます。

## _WETH

**Wrapped Ether（WETH）** コントラクトのアドレス。
Etherの**ERC20**トークンバージョンを表し、**Uniswap**の取引ペアで使用されます。

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

:::message
この章では、「**受け入れ可能な**」という言葉を使用していますが、これは取引の際における最低限の条件を表すものです。
これらの条件を指定することで、ユーザーが特定のトランザクションを実行する際に、取引の最小限の条件を保証することができます。

具体的な例を挙げると、`amountTokenMin`が100、`amountETHMin`が0.5と設定されている場合を考えてみましょう。
ユーザーが関数を呼び出す際に、プールに追加するトークンの数量は少なくとも`100`以上であり、同様にETHの数量も少なくとも`0.5`以上である必要があります。
これは、取引の最低限の条件を保証するためのもので、これらの条件を満たさない場合、関数は実行されずにエラーが発生します。
:::

## READ

### quote

指定された数量のトークンAが、指定されたトークンAとトークンBのリザーブ（保有数量）に基づいて相当する数量のトークンBを計算する関数。

**引数**

- `amountA`
	- トークンAの数量。
	- この数量に対して、相当するトークンBの数量が計算される。
- `reserveA`
	- トークンAのリザーブ（保有数量）。
	- この数量は**Uniswap**プール内のトークンAの保有数量。
- `reserveB`
	- トークンBのリザーブ（保有数量）。
	- この数量は**Uniswap**プール内のトークンBの保有数量。

**処理**

1. 指定された数量のトークンAが、指定されたトークンAとトークンBのリザーブに基づいて相当する数量のトークンBを計算します。
2. 計算された相当するトークンBの数量を`amountB`として返します。

### getAmountOut

指定された数量の入力トークンが、指定された入力トークンと出力トークンのリザーブ（保有数量）に基づいて交換時の予測される出力トークンの数量を計算する関数。

**引数**

- `amountIn`
	- 入力トークンの数量。
	- この数量がどれだけ交換に使われるかを示す。
- `reserveIn`
	- 入力トークンのリザーブ（保有数量）。
- `reserveOut`
	- 出力トークンのリザーブ（保有数量）。

**処理**

1. 指定された数量の入力トークンが、指定された入力トークンと出力トークンのリザーブに基づいて交換時の予測される出力トークンの数量を計算します。
2. 計算された予測出力トークンの数量を`amountOut`として返します。

### getAmountIn

指定された数量の出力トークンが、指定された入力トークンと出力トークンのリザーブ（保有数量）に基づいて交換時に必要な入力トークンの数量を計算する関数。

**引数**

- `amountOut`
        - 出力トークンの数量。
        - この数量がどれだけの出力トークンを得たいかを示す。
- `reserveIn`
        - 入力トークンのリザーブ（保有数量）。
- `reserveOut`
        - 出力トークンのリザーブ（保有数量）。

**処理**

1. 指定された数量の出力トークンが、指定された入力トークンと出力トークンのリザーブに基づいて交換時に必要な入力トークンの数量を計算します。
2. 計算された必要な入力トークンの数量を`amountIn`として返します。

### getAmountsOut

指定された入力トークンの数量とトークンのパス（トークン間の交換経路）を使用して、それぞれのトークンの数量を予測数量として計算する関数。

**引数**

- `amountIn`
        - 入力トークンの数量。
        - この数量が交換に際してどれだけの入力トークンを使用するかを示す。
- `path`
        - 交換するトークン間の順番通りのアドレスが含まれる配列。
        - 例えば、[TokenA, TokenB, TokenC]のように、TokenAからTokenBを経由してTokenCに交換する場合、このパスは[TokenAのアドレス, TokenBのアドレス, TokenCのアドレス]となる。

**処理**

1. 指定された入力トークンの数量とトークンのパスを使用して、それぞれのトークンの数量を`path`配列内の各トークンの予測数量として計算します。
2. 計算された予測数量の配列を`amounts`として返します。
3. この配列は、`path`配列内の各トークンの数量が含まれており、交換後のトークンの数量を表します。

### getAmountsIn

指定された出力トークンの数量とトークンのパス（トークン間の交換経路）を使用して、それぞれのトークンの必要数量を計算する関数。

**引数**

- `amountOut`
        - 出力トークンの数量。
        - この数量が交換によって得られる出力トークンの数量を示す。
- `path`
        - 交換するトークン間の順番通りのアドレスが含まれる配列。
        - 例えば、[TokenA, TokenB, TokenC]のように、TokenAからTokenBを経由してTokenCに交換する場合、このパスは[TokenAのアドレス, TokenBのアドレス, TokenCのアドレス]となる。

**処理**

1. 指定された出力トークンの数量とトークンのパスを使用して、それぞれのトークンの数量を`path`配列内の各トークンの必要数量として計算します。
2. 計算された必要数量の配列を`amounts`として返します。
3. この配列は、`path`配列内の各トークンの数量が含まれており、交換後のトークンの数量を表します。

## WRITE

### _addLiquidity

**Uniswap**の流動性を追加するためのロジックを実装している関数。
流動性プールにトークンを追加する際に使用されるもので、トークンの交換率やリザーブ（保有量）の状態に基づいて適切な量を計算して流動性を提供します。
また、新しいトークンペアを作成することで、まだ存在しないトークンペアの追加も行えます。

**引数**

- `tokenA`
	- 流動性プールに追加するトークンAのアドレス。
- `tokenB`
	- 流動性プールに追加するトークンBのアドレス。
- `amountADesired`
	- 預け入れるトークンAの数量。
- `amountBDesired`
	- 預け入れるトークンBの数量。
- `amountAMin`
	- 受け入れ可能な最小トークンAの数量。
- `amountBMin`
	- 受け入れ可能な最小トークンBの数量。

**処理**

1. ペアが存在しない場合は、指定されたトークンAとトークンBのペアを作成します。
これによって流動性プールが初めて作成されることがあります。
2. 指定されたトークンAとトークンBのリザーブ（保有量）を取得します。
3. もし両方のトークンのリザーブがゼロであれば、預け入れる数量そのままを設定します。
4. もしリザーブがゼロでない場合、最適な取引量を計算します。
トークンAを指定数量で交換した場合の、トークンBの最適な数量を計算します。
その後、トークンBの最適な数量が望む数量以下であれば、最小数量を満たすか確認します。
条件を満たす場合、トークンAとトークンBの数量を設定します。
5. そうでなければ、トークンBを指定数量で交換した場合の、トークンAの最適な数量を計算します。計算された最適な数量が望む数量以上であるか確認し、最小数量を満たすか確認します。
条件を満たす場合、トークンAとトークンBの数量を設定します。

### addLiquidity

Uniswapの流動性を追加する関数。
指定されたトークンAとトークンBの数量に基づいて流動性プールにトークンを供給し、対応する流動性トークンを受け取ります。

**引数**

- `tokenA`
	- 流動性プールに追加するトークンAのアドレス。
- `tokenB`
	- 流動性プールに追加するトークンBのアドレス。
- `amountADesired`
	- 預け入れるトークンAの数量。
- `amountBDesired`
	- 預け入れるトークンBの数量。
- `amountAMin`
	- 受け入れ可能な最小トークンAの数量。
- `amountBMin`
	- 受け入れ可能な最小トークンBの数量。
- `to`
	- 流動性トークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. 有効期限が確認されます。
トランザクションの実行が期限内であることを確認するための修飾子「`ensure`」が適用されています。
2. 内部関数「`_addLiquidity`」を呼び出し、指定された引数に基づいて最適な取引量と供給するトークンの数量を計算します。
3. トークンAとトークンBの数量が計算されると、それらのトークンを指定の流動性プールへ移動させるために「`TransferHelper.safeTransferFrom`」関数が使用されます。
4. 最後に、計算された数量と指定された受取アドレス「`to`」を使用して、流動性トークンを発行するための「`IUniswapV2Pair(pair).mint(to)`」が呼び出されます。
この操作によって、新しい流動性トークンが発行され、供給者に発行された流動性トークンが送られます。

### addLiquidityETH

ETH（Ether）とトークンを使用してUniswapの流動性を追加する関数。
ETHとトークンを使用してUniswapの流動性を追加するためのものであり、ETHを受け入れつつトークンを供給する場合に利用されます。

**引数**

- `token`
	- 流動性プールに追加するトークンのアドレス。
- `amountTokenDesired`
	- 預け入れるトークンの数量。
- `amountTokenMin`
	- 受け入れ可能な最小トークンの数量。
- `amountETHMin`
	- 受け入れ可能な最小ETHの数量。
- `to`
	- 流動性トークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. 有効期限が確認されます。
トランザクションの実行が期限内であることを確認するための修飾子「`ensure`」が適用されています。
2. 内部関数「`_addLiquidity`」を呼び出し、指定された引数に基づいて最適な取引量と供給するトークンとETHの数量を計算します。
この関数は、トークンとETHの両方を受け取り、トークンとETHを流動性プールに供給するための取引量を計算します。
3. トークンとETHの数量が計算されると、トークンを指定の流動性プールへ移動させるために「`TransferHelper.safeTransferFrom`」関数が使用されます。
また、ETHも受け入れるために、内部で「`IWETH(WETH).deposit{value: amountETH}()`」と「`IWETH(WETH).transfer(pair, amountETH)`」が呼び出されます。
4. 最後に、計算された数量と指定された受け取りアドレス「`to`」を使用して、流動性トークンを発行するための「`IUniswapV2Pair(pair).mint(to)`」が呼び出されます。
この操作によって、新しい流動性トークンが発行され、供給者に発行された流動性トークンが送られます。
5. トランザクションに余分なETHが含まれている場合、その余分なETHは送り返されます。

### removeLiquidity

Uniswapの流動性を取り出す関数。
指定数量の流動性トークンを取り出して、対応するトークンAとトークンBを返すためのものです。

**引数**

- `tokenA`
	- 流動性プールから取り出すトークンAのアドレス。
- `tokenB`
	- 流動性プールから取り出すトークンBのアドレス。
- `liquidity`
	- 取り出す流動性トークンの数量。
- `amountAMin`
	- 受け入れ可能な最小トークンAの数量。
- `amountBMin`
	- 受け入れ可能な最小トークンBの数量。
- `to`
	- 預け入れていたトークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. 有効期限が確認されます。
トランザクションの実行が期限内であることを確認するための修飾子「`ensure`」が適用されています。
2. トークンAとトークンBのアドレスを使用して、対応する流動性プールのアドレスを計算します。
3. 「`IUniswapV2Pair(pair).transferFrom(msg.sender, pair, liquidity)`」を呼び出して、指定数量の流動性トークンを持っているユーザーから流動性プールへ送信します。
これによって、指定数量の流動性トークンが流動性プールに戻されます。
4. 「`IUniswapV2Pair(pair).burn(to)`」を呼び出して、流動性プールから指定された受け取りアドレス「`to`」に向けてトークンAとトークンBを取り出します。
この操作により、トークンAとトークンBの数量が計算されます。
5. トークンAとトークンBのアドレスをソートし、トークンAのアドレスがどちらかを特定します。
そして、それに基づいて数量を「`amountA`」と「`amountB`」に割り当てます。
6. 最後に、「`amountAMin`」と「`amountBMin`」の条件を確認し、取り出すトークンの数量が受け入れ可能な最小数量を下回らないことを確認します。
条件を満たしていない場合、トランザクションは中断されます。

### removeLiquidityETH

ETHと任意のトークン間での流動性を取り出す関数。
ETHと任意のトークンの流動性トークンを取り出し、指定数量のトークンとETHを受け取るためのものです。

**引数**

- `token`
	- 取り出すトークンのアドレス。
- `liquidity`
	- 取り出す流動性トークンの数量。
- `amountTokenMin`
	- 受け入れ可能な最小トークンの数量。
- `amountETHMin`
	- 受け入れ可能な最小ETHの数量。
- `to`
	- 取り出したトークンとETHを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. 有効期限が確認されます。
トランザクションの実行が期限内であることを確認するための修飾子「`ensure`」が適用されています。
2. 内部で定義された「`removeLiquidity`」関数を呼び出し、指定数量のトークンとETHの流動性トークンを取り出します。
この際、関数の引数として指定された「`token`」に加えて、**WETH**（ETHをラップしたトークン）も引数として渡します。
3. 「`removeLiquidity`」関数から返された「`amountToken`」と「`amountETH`」を使用して、指定数量のトークンとETHを受け取りアドレス「`to`」に送信します。
4. 取り出したETHは**WETH**トークンにラップされているため、ETHを取り出すために「`IWETH(WETH).withdraw(amountETH)`」を呼び出します。
これにより、指定数量のETHが**WETH**トークンから取り出されます。
5. 最後に、「`TransferHelper.safeTransferETH(to, amountETH)`」を使用して、指定数量のETHを受け取りアドレス「`to`」に送信します。
これにより、取り出されたETHが受け取りアドレスに送信されます。

### removeLiquidityWithPermit

**UniswapV2**のプールからトークンを取り出す際に、トークンの所有者が事前に署名した許可を使用してトランザクションを実行する関数。
トークンの所有者が署名を用いて許可を与えた上で、プールからトークンを取り出すためのものです。

**引数**

- `tokenA`
	- 取り出すトークンAのアドレス。
- `tokenB`
	- 取り出すトークンBのアドレス。
- `liquidity`
	- 取り出す流動性トークンの数量。
- `amountAMin`
	- 受け入れ可能な最小トークンAの数量。
- `amountBMin`
	- 受け入れ可能な最小トークンBの数量。
- `to`
	- 取り出したトークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。
- `approveMax`
	- 署名の許可を与えるためのフラグ。
	- `true`ならば無制限、`false`ならば`liquidity`で指定した数量までの許可が与えられる。
- `v`, `r`, `s`
	- 署名のパラメータ

**処理**

1. `pair`変数を使用して、`tokenA`と`tokenB`のアドレスからプールのペアのアドレスを取得します。
2. `approveMax`が`true`であれば、`value`に最大値（`2^256 - 1`）を設定し、それ以外の場合は`liquidity`の値を設定します。
これはトークンの許可を与える量を指定します。
3. `IUniswapV2Pairのpermit`関数を呼び出して、トークンの所有者が事前に署名した許可情報を使用してトランザクションを起こします。
4. `removeLiquidity`関数を呼び出して、実際にトークンを取り出す処理を行います。
この関数はトークンの数量や最小受け入れ可能量を考慮してトークンを取り出します。
5. 最終的に、`amountA`と`amountB`に取り出されたトークンの数量が代入され、この値が関数の戻り値として返されます。

### removeLiquidityETHWithPermit

UniswapV2のプールからトークンとETHを同時に取り出す操作を行う際に、トークンの所有者があらかじめ署名した許可情報を使用してトランザクションを実行する関数。

**引数**

- `token`
	- 取り出すトークンのアドレス。
- `liquidity`
	- 取り出す流動性トークンの数量。
- `amountTokenMin`
	- 受け入れ可能な最小トークンの数量。
- `amountETHMin`
	- 受け入れ可能な最小ETHの数量。
- `to`
	- 取り出したトークンとETHを受け取るアドレス。
- `deadline`
	- 処理の有効期限。
- `approveMax`
	- 署名の許可を与えるためのフラグ。
	- `true`ならば無制限、`false`ならば`liquidity`で指定した数量までの許可が与えられる。
- `v`, `r`, `s`
	- 署名のパラメータ

**処理**

1. `pair`変数を使用して、`token`と**WETH**（ラップされたEther）のアドレスからプールのペアのアドレスを取得します。
2. `approveMax`が`true`であれば、`value`に最大値（`2^256 - 1`）を設定し、それ以外の場合は`liquidity`の値を設定します。
これはトークンの許可を与える量を指定します。
3. `IUniswapV2Pairのpermit`関数を呼び出して、トークンの所有者が事前に署名した許可情報を使用してトランザクションを起こします。
4. `removeLiquidityETH`関数を呼び出して、実際にトークンとETHを同時に取り出す処理を行います。
この関数はトークンとETHの数量や最小受け入れ可能量を考慮してトークンとETHを取り出します。
5. 最終的に、`amountToken`と`amountETH`に取り出されたトークンとETHの数量が代入され、この値が関数の戻り値として返されます。

### removeLiquidityETHSupportingFeeOnTransferTokens

**UniswapV2**のプールからトークンとETHを同時に取り出す操作を行う際に、フィー（手数料）を支払うトークン（fee-on-transfer tokens）をサポートするための関数。

**引数**

- `token`
	- 取り出すトークンのアドレス。
- `liquidity`
	- 取り出す流動性トークンの数量。
- `amountTokenMin`
	- 受け入れ可能な最小トークンの数量。
- `amountETHMin`
	- 受け入れ可能な最小ETHの数量。
- `to`
	- 取り出したトークンとETHを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. `removeLiquidity`関数を呼び出して、トークンとETHを取り出す操作を行います。
この関数の戻り値である`amountETH`は取り出したETHの数量を示します。
2. トークンのバランスを取得し、「`TransferHelper.safeTransfer`」を使用して指定したアドレスにトークンを送信します。
これにより、フィー（手数料）を支払うトークンを受け取るための準備が行われます。
3. **WETH**コントラクトの`withdraw`関数を呼び出して、取り出したETHを元のEtherに戻します。
4. 「`TransferHelper.safeTransferETH`」を使用して、指定したアドレスに取り出したETHを送信します。
これにより、ETHが受け取り先に送信されます。

### removeLiquidityETHWithPermitSupportingFeeOnTransferTokens

フィー（手数料）を支払うトークンをサポートしながら、**UniswapV2**のプールからトークンとETHを同時に取り出す操作を行う関数。

**引数**

- `token`
	- 取り出すトークンのアドレス。
- `liquidity`
	- 取り出す流動性トークンの数量。
- `amountTokenMin`
	- 受け入れ可能な最小トークンの数量。
- `amountETHMin`
	- 受け入れ可能な最小ETHの数量。
- `to`
	- 取り出したトークンとETHを受け取るアドレス。
- `deadline`
	- 処理の有効期限。
- `approveMax`
	- 署名の許可を与えるためのフラグ。
	- `true`ならば無制限、`false`ならば`liquidity`で指定した数量までの許可が与えられる。
- `v`, `r`, `s`
	- 署名のパラメータ

**処理**

1. `pairFor`関数を使用してトークンと**WETH**のペアのアドレスを取得します。
2. `approveMax`の値に応じて、`permit`の許可数量を設定します。
`approveMax`が`true`の場合は無制限の許可、`false`の場合は`liquidity`の数量を許可します。
3. **IUniswapV2Pair**インターフェースの`permit`関数を呼び出して、トークンの許可を行います。
これにより、フィー（手数料）を支払うトークンを許可します。
4. `removeLiquidityETHSupportingFeeOnTransferTokens`関数を呼び出して、トークンとETHの同時取り出し操作を行います。
この関数の戻り値である`amountETH`を返します。

### _swap

**UniswapV2**のプール内でトークンをスワップする関数。

**引数**

- `amounts`
	- スワップするトークンの量の配列。
	- `amounts[0]`は入力トークンの数量で、`amounts[n]`は出力トークンの数量（nはスワップするトークンの数）。
- `path`
	- スワップするトークンのパスを示す配列。
	- パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `_to`
	- スワップのしたトークンを受け取るアドレス。


**処理**

1. `path`の要素数から`1`を引いた数（`path.length - 1`）だけ繰り返します。
これはパス内のトークンのペアごとに処理を行うためです。
2. 現在のトークンと次のトークンを取得し、ソートして`input`と`output`に設定します。
ソートによって、トークンの順序が正しくなるように決定されます。
3. スワップする出力トークンの数量を`amountOut`に設定します。
4. `input`と`output`がどちらがトークン`0`かを判定し、それに応じて`amount0Out`と`amount1Out`を設定します。
これは、スワップペアによって異なる量のトークンが生成されるためです。
5. 現在のトークンと次のトークンから、スワップの結果を受け取るアドレス`to`を決定します。
`i`が`path.length - 2`未満の場合は次のトークンまでのペアのアドレス、それ以外の場合は`_to`になります。
6. `UniswapV2Library.pairFor`関数を使用して、現在のトークンと次のトークンのペアのアドレスを取得します。
7. **IUniswapV2Pair**インターフェースの`swap`関数を呼び出して、トークンをスワップします。
`amount0Out`と`amount1Out`に応じて、適切な量のトークンがスワップされます。

### swapExactTokensForTokens

指定したトークンの入力量に対して一定量の出力トークンを得るために、**UniswapV2**のプールでトークンをスワップする関数。

**引数**

- `amountIn`
	- 入力トークンの数量。
- `amountOutMin`
	- 受け入れ可能な最小出力トークンの数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. `UniswapV2Library.getAmountsOut`関数を使用して、指定された入力トークン量とトークンパスに基づいて、スワップ後の出力トークン量の配列`amounts`を取得します。
`amounts[0]`は入力トークンから最初のトークンへのスワップで得られる数量で、`amounts[n]`は最終的な出力トークンの数量です。
2. `amounts`配列の最後の要素（`amounts[amounts.length - 1]`）が`amountOutMin`以上であることを確認します。
これにより、最低限必要な出力トークンの数量を保証します。
3. `TransferHelper.safeTransferFrom`関数を使用して、入力トークンを送信します。
具体的には、`msg.sender`から最初のトークンへのスワップペアのアドレスに対して、指定された入力トークン量を送信します。
4. `_swap`関数を呼び出して、トークンを実際にスワップします。
これにより、指定されたトークンパスに基づいてスワップが行われます。

### swapTokensForExactTokens

指定した出力トークンの数量に対して、最大限許容される入力トークンの量を計算し、その入力トークン量でトークンをスワップする関数。

**引数**

- `amountOut`
	- 受け入れ可能な最小出力トークンの数量。
- `amountInMax`
	- 受け入れ可能な最大入力トークンの数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. `UniswapV2Library.getAmountsIn`関数を使用して、指定された出力トークン量とトークンパスに基づいて、スワップに必要な最大入力トークン量の配列`amounts`を取得します。
`amounts[0]`は入力トークンから最初のトークンへのスワップで得られる数量で、`amounts[n]`は最終的な出力トークンの数量です。
2. `amounts`配列の最初の要素（`amounts[0]`）が`amountInMax`以下であることを確認します。
これにより、許容される最大の入力トークン量を超えないようにします。
3. `TransferHelper.safeTransferFrom`関数を使用して、入力トークンを送信します。
具体的には、`msg.sender`から最初のトークンへのスワップペアのアドレスに対して、指定された最大入力トークン量を送信します。
4. `_swap`関数を呼び出して、トークンを実際にスワップします。
これにより、指定されたトークンパスに基づいてスワップが行われます。

### swapExactETHForTokens

指定したETHの数量を使用して、ETHをトークンにスワップする関数。

**引数**

- `amountOutMin`
	- 受け入れ可能な最小出力トークンの数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. `path`配列の最初の要素が**WETH**（ETHのラップトークン）であることを確認します。
これはETHからトークンにスワップするためのパスを指定する必要があることを示します。
2. `UniswapV2Library.getAmountsOut`関数を使用して、指定されたETH量とトークンパスに基づいて、スワップによって得られる出力トークンの数量の配列`amounts`を計算します。
`amounts[0]`はETHからのスワップに必要なETHの数量であり、`amounts[n]`は出力トークンまでのスワップによって得られるトークンの数量です。
3. `amounts`配列の最後の要素（`amounts[amounts.length - 1]`）が`amountOutMin`以上であることを確認します。
これにより、望む出力トークンの最小数量を満たすことを保証します。
4. `IWETH(WETH).deposit{value: amounts[0]}()`を使用して、指定されたETH量を**WETH**に変換し、**WETH**を受け取ります。
5. `assert(IWETH(WETH).transfer(...))`を使用して、**WETH**をスワップの最初のペアに送信します。
6. `_swap`関数を呼び出して、トークンのスワップを実行します。
これにより、指定されたトークンパスに基づいてETHからトークンにスワップが行われます。

### swapTokensForExactETH

指定したトークンの数量を使用してトークンをETHにスワップする関数。

**引数**

- `amountOut`
	- 受け入れ可能な最小出力ETHの数量。
- `amountInMax`
	- 受け入れ可能な最大トークン数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたETHを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. `path`配列の最初の要素が**WETH**（ETHのラップトークン）であることを確認します。
これはトークンからETHにスワップするためのパスを指定する必要があることを示します。
2. `UniswapV2Library.getAmountsIn`関数を使用して、指定された出力ETH量とトークンパスに基づいて、必要な入力トークンの数量の配列`amounts`を計算します。
`amounts[0]`はスワップに必要な入力トークンの数量であり、`amounts[n]`はETHへのスワップによって得られるETHの数量です。
3. `amounts[0]`が`amountInMax`以下であることを確認します。
これにより、指定した最大入力トークン数量内でスワップを行うことを保証します。
4. `TransferHelper.safeTransferFrom`を使用して、指定された入力トークンをペアに送信します。
5. `_swap`関数を呼び出して、トークンのスワップを実行します。
これにより、指定されたトークンパスに基づいてトークンからETHにスワップが行われます。
6. `IWETH(WETH).withdraw`を使用して、**WETH**をETHに変換し、出力ETHの数量を取得します。
7. `TransferHelper.safeTransferETH`を使用して、指定されたアドレスに出力ETHの数量を送信します。

### swapExactTokensForETH

指定したトークンの数量を使用してトークンをETHにスワップする関数。

**引数**

- `amountIn`
	- 受け入れ可能な最小入力トークンの数量。
- `amountInMax`
	- 受け入れ可能な最小ETH数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたETHを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. `path`配列の最初の要素が**WETH**（ETHのラップトークン）であることを確認します。
これはトークンからETHにスワップするためのパスを指定する必要があることを示します。
2. `UniswapV2Library.getAmountsOut`関数を使用して、指定されたETH量とトークンパスに基づいて、スワップによって得られる出力ETHの数量の配列`amounts`を計算します。
`amounts[0]`は入力トークンからスワップに必要な出力ETHの数量であり、`amounts[n]`はETHへのスワップによって得られるETHの数量です。
3. `amounts[n]`が`amountOutMin`以上であることを確認します。
これにより、指定した最小出力ETH数量を満たすようにスワップが行われることを保証します。
4. `TransferHelper.safeTransferFrom`を使用して、指定された入力トークンをペアに送信します。
5. `_swap`関数を呼び出して、トークンのスワップを実行します。
これにより、指定されたトークンパスに基づいてトークンからETHにスワップが行われます。
6. `IWETH(WETH).withdraw`を使用して、**WETH**をETHに変換し、出力ETHの数量を取得します。
7. `TransferHelper.safeTransferETH`を使用して、指定されたアドレスに出力ETHの数量を送信します。

### swapETHForExactTokens

指定したETHの数量を使用してトークンを取得する関数。

**引数**

- `amountOut`
	- 受け入れ可能な最小出力トークン数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。
- `deadline`
	- 処理の有効期限。

**処理**

1. `path`配列の最初の要素が**WETH**（ETHのラップトークン）であることを確認します。
これはETHからトークンにスワップするためのパスを指定する必要があることを示します。
2. `UniswapV2Library.getAmountsIn`関数を使用して、指定された出力トークン数量とトークンパスに基づいて、必要な入力ETHの数量の配列`amounts`を計算します。
`amounts[0]`はスワップに必要な入力ETHの数量であり、`amounts[n]`は指定した出力トークン数量を得るために必要なETHの数量です。
3. `amounts[0]`が`msg.value`以下であることを確認します。
これにより、指定した入力ETH数量を超えないようにスワップが行われることを保証します。
4. `IWETH(WETH).deposit`を使用して、ETHを**WETH**に変換し、入力ETHの数量をWETHとして保持します。
5. `IWETH(WETH).transfer`を使用して、**WETH**を指定されたペアに送信します。
これにより、**WETH**からトークンへのスワップが行われます。
6. `_swap`関数を呼び出して、トークンのスワップを実行します。
これにより、指定されたトークンパスに基づいてETHからトークンにスワップが行われます。
7. もし実際に送信されたETHが必要な入力ETHの数量よりも多い場合、余剰のETHを送信元に返金します。

### _swapSupportingFeeOnTransferTokens

手数料を支払うトークンをサポートするスワップ処理を実行する関数。

**引数**

- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。

**処理**

1. `for`ループを使用して、パスに含まれる各トークンのスワップを順番に実行します。
2. ループ内で、`path`配列の現在の要素と次の要素を`input`と`output`に分割します。
3. `UniswapV2Library.sortTokens`関数を使用して、`input`と`output`のトークンの順序をソートし、それぞれ`token0`と`token1`に割り当てます。
4. **IUniswapV2Pair**インターフェースを使用して、スワップするペアのリザーブ（保有数量）を取得します。
5. `IERC20(input).balanceOf(address(pair))`を使用して、スワップペアに保有されている`input`トークンの残高を取得します。
この値からリザーブの差分を計算し、スワップするためのトークンの数量`amountInput`を決定します。
6. `UniswapV2Library.getAmountOut`関数を使用して、`amountInput`トークンをスワップした場合に得られる`output`トークンの数量を計算します。
7. `input`トークンと`output`トークンの関係に応じて、スワップに使用するトークンの数量を`amount0Out`と`amount1Out`に設定します。
8. `UniswapV2Library.pairFor`関数を使用して、スワップ先のペアのアドレスを計算し、スワップ結果を送信するアドレス`to`を決定します。
9. `pair.swap`関数を使用して、実際のトークンのスワップを実行します。
この関数により、スワップされたトークンが`to`に送信されます。

### swapExactTokensForTokensSupportingFeeOnTransferTokens

手数料を支払うトークンをサポートする状態で、一定数量のトークンをスワップして別のトークンに交換する関数。

**引数**

- `amountIn`
	- スワップ元のトークンの数量。
- `amountOutMin`
        - 受け入れ可能な最小出力トークン数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。
- `deadline`
        - 処理の有効期限。

**処理**

1. `TransferHelper.safeTransferFrom`関数を使用して、`msg.sender`からスワップ元のトークンを移動します。
移動するトークンは`amountIn`で指定された数量です。
2. `UniswapV2Library.pairFor`関数を使用して、スワップするトークンのペアのアドレスを計算します。
このアドレスにスワップ元のトークンを送信します。
3. スワップ前の`to`アドレスにおける`path[path.length - 1]`トークンの残高を取得し、`balanceBefore`として保持します。
4. `_swapSupportingFeeOnTransferTokens`関数を呼び出して、手数料を支払うトークンをサポートする状態でトークンをスワップします。
5. スワップ後、`to`アドレスにおける`path[path.length - 1]`トークンの残高を再度取得し、スワップ結果として得られたトークンの増加量を計算します。
6. `amountOutMin`と比較して、スワップ結果として得られたトークンの増加量が十分かを確認します。
十分でない場合、エラーを出力します。

### swapExactETHForTokensSupportingFeeOnTransferTokens

ETH（イーサリアム）を支払って、手数料を支払うトークンをサポートする状態で一定数量のETHをスワップして別のトークンに交換する関数。

**引数**

- `amountOutMin`
        - 受け入れ可能な最小出力トークン数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。
- `deadline`
        - 処理の有効期限。

**処理**

1. `path[0]`が**WETH**（Wrapped Ether）であることを確認します。
**WETH**は**ETH** をイーサリアムのトークンとしてラップしたものです。
2. `msg.value`から得られる**ETH**の数量を`amountIn`として保持します。
3. `IWETH(WETH).deposit`関数を使用して、**ETH**を**WETH**に変換し、**WETH**の数量を`amountIn`として保持します。
4. `IWETH(WETH).transfer`関数を使用して、**WETH**をスワップの最初のトークンである`path[0]`として指定されたペアに送信します。
5. スワップ前の`to`アドレスにおける`path[path.length - 1]`トークンの残高を取得し、`balanceBefore`として保持します。
6. `_swapSupportingFeeOnTransferTokens`関数を呼び出して、手数料を支払うトークンをサポートする状態でトークンをスワップします。
7. スワップ後、`to`アドレスにおける`path[path.length - 1]`トークンの残高を再度取得し、スワップ結果として得られたトークンの増加量を計算します。
8. `amountOutMin`と比較して、スワップ結果として得られたトークンの増加量が最低量`amountOutMin`を満たしているかを確認します。
満たしていない場合、エラーを出力します。

### swapExactTokensForETHSupportingFeeOnTransferTokens

手数料を支払うトークンをサポートする状態で一定数量のトークンをスワップしてETHに交換する関数。


**引数**

- `amountIn`
	- スワップするトークンの数量。
- `amountOutMin`
        - 受け入れ可能な最小出力ETH数量。
- `path`
        - スワップするトークンのパスを示す配列。
        - パスは入力トークンから出力トークンまでのトークンの順序を指定。
- `to`
        - スワップのしたトークンを受け取るアドレス。
- `deadline`
        - 処理の有効期限。

**処理**

1. `path[path.length - 1]`が**WETH**（Wrapped Ether）であることを確認します。
**ETH**を表す**WETH**トークンです。
2. `path[0]`からトークンの数量`amountIn`を、指定されたペアに送信します。
3. `_swapSupportingFeeOnTransferTokens`関数を呼び出して、手数料を支払うトークンをサポートする状態でトークンをスワップします。
4. スワップ後、アドレス`address(this)`における**WETH**トークンの残高を取得し、`amountOut`として保持します。
5. `amountOut`が最小数量`amountOutMin`を満たしているかを確認します。
満たしていない場合、エラーを出力します。
6. `IWETH(WETH).withdraw`関数を使用して**WETH**を**ETH**に戻し、その数量を引き出します。
7. `TransferHelper.safeTransferETH`関数を使用して、指定されたアドレス`to`に**ETH**を送信します。

# イベント

特になし。

# コード

記事の最大文字数の関係から、**UniswapV2Router01**と共通の関数は省きます。

https://zenn.dev/cryptogames/articles/75433e91eea8cc

## IUniswapV2Router02

:::details IUniswapV2Router02
```solidity
interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
```
:::

## UniswapV2Router02

:::details UniswapV2Router02
```solidity
contract UniswapV2Router02 is IUniswapV2Router02 {
    using SafeMath for uint;

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
    ) internal virtual returns (uint amountA, uint amountB) {
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
    ) external virtual override ensure(deadline) returns (uint amountA, uint amountB, uint liquidity) {
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
    ) external virtual override payable ensure(deadline) returns (uint amountToken, uint amountETH, uint liquidity) {
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
        // refund dust eth, if any
        if (msg.value > amountETH) TransferHelper.safeTransferETH(msg.sender, msg.value - amountETH);
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
    ) public virtual override ensure(deadline) returns (uint amountA, uint amountB) {
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
    ) public virtual override ensure(deadline) returns (uint amountToken, uint amountETH) {
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
    ) external virtual override returns (uint amountA, uint amountB) {
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
    ) external virtual override returns (uint amountToken, uint amountETH) {
        address pair = UniswapV2Library.pairFor(factory, token, WETH);
        uint value = approveMax ? uint(-1) : liquidity;
        IUniswapV2Pair(pair).permit(msg.sender, address(this), value, deadline, v, r, s);
        (amountToken, amountETH) = removeLiquidityETH(token, liquidity, amountTokenMin, amountETHMin, to, deadline);
    }

    // **** REMOVE LIQUIDITY (supporting fee-on-transfer tokens) ****
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) public virtual override ensure(deadline) returns (uint amountETH) {
        (, amountETH) = removeLiquidity(
            token,
            WETH,
            liquidity,
            amountTokenMin,
            amountETHMin,
            address(this),
            deadline
        );
        TransferHelper.safeTransfer(token, to, IERC20(token).balanceOf(address(this)));
        IWETH(WETH).withdraw(amountETH);
        TransferHelper.safeTransferETH(to, amountETH);
    }
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external virtual override returns (uint amountETH) {
        address pair = UniswapV2Library.pairFor(factory, token, WETH);
        uint value = approveMax ? uint(-1) : liquidity;
        IUniswapV2Pair(pair).permit(msg.sender, address(this), value, deadline, v, r, s);
        amountETH = removeLiquidityETHSupportingFeeOnTransferTokens(
            token, liquidity, amountTokenMin, amountETHMin, to, deadline
        );
    }

    // **** SWAP ****
    // requires the initial amount to have already been sent to the first pair
    function _swap(uint[] memory amounts, address[] memory path, address _to) internal virtual {
        for (uint i; i < path.length - 1; i++) {
            (address input, address output) = (path[i], path[i + 1]);
            (address token0,) = UniswapV2Library.sortTokens(input, output);
            uint amountOut = amounts[i + 1];
            (uint amount0Out, uint amount1Out) = input == token0 ? (uint(0), amountOut) : (amountOut, uint(0));
            address to = i < path.length - 2 ? UniswapV2Library.pairFor(factory, output, path[i + 2]) : _to;
            IUniswapV2Pair(UniswapV2Library.pairFor(factory, input, output)).swap(
                amount0Out, amount1Out, to, new bytes(0)
            );
        }
    }
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external virtual override ensure(deadline) returns (uint[] memory amounts) {
        amounts = UniswapV2Library.getAmountsOut(factory, amountIn, path);
        require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]
        );
        _swap(amounts, path, to);
    }
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external virtual override ensure(deadline) returns (uint[] memory amounts) {
        amounts = UniswapV2Library.getAmountsIn(factory, amountOut, path);
        require(amounts[0] <= amountInMax, 'UniswapV2Router: EXCESSIVE_INPUT_AMOUNT');
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]
        );
        _swap(amounts, path, to);
    }
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        virtual
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
        virtual
        override
        ensure(deadline)
        returns (uint[] memory amounts)
    {
        require(path[path.length - 1] == WETH, 'UniswapV2Router: INVALID_PATH');
        amounts = UniswapV2Library.getAmountsIn(factory, amountOut, path);
        require(amounts[0] <= amountInMax, 'UniswapV2Router: EXCESSIVE_INPUT_AMOUNT');
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]
        );
        _swap(amounts, path, address(this));
        IWETH(WETH).withdraw(amounts[amounts.length - 1]);
        TransferHelper.safeTransferETH(to, amounts[amounts.length - 1]);
    }
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        virtual
        override
        ensure(deadline)
        returns (uint[] memory amounts)
    {
        require(path[path.length - 1] == WETH, 'UniswapV2Router: INVALID_PATH');
        amounts = UniswapV2Library.getAmountsOut(factory, amountIn, path);
        require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]
        );
        _swap(amounts, path, address(this));
        IWETH(WETH).withdraw(amounts[amounts.length - 1]);
        TransferHelper.safeTransferETH(to, amounts[amounts.length - 1]);
    }
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        virtual
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
        // refund dust eth, if any
        if (msg.value > amounts[0]) TransferHelper.safeTransferETH(msg.sender, msg.value - amounts[0]);
    }

    // **** SWAP (supporting fee-on-transfer tokens) ****
    // requires the initial amount to have already been sent to the first pair
    function _swapSupportingFeeOnTransferTokens(address[] memory path, address _to) internal virtual {
        for (uint i; i < path.length - 1; i++) {
            (address input, address output) = (path[i], path[i + 1]);
            (address token0,) = UniswapV2Library.sortTokens(input, output);
            IUniswapV2Pair pair = IUniswapV2Pair(UniswapV2Library.pairFor(factory, input, output));
            uint amountInput;
            uint amountOutput;
            { // scope to avoid stack too deep errors
            (uint reserve0, uint reserve1,) = pair.getReserves();
            (uint reserveInput, uint reserveOutput) = input == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
            amountInput = IERC20(input).balanceOf(address(pair)).sub(reserveInput);
            amountOutput = UniswapV2Library.getAmountOut(amountInput, reserveInput, reserveOutput);
            }
            (uint amount0Out, uint amount1Out) = input == token0 ? (uint(0), amountOutput) : (amountOutput, uint(0));
            address to = i < path.length - 2 ? UniswapV2Library.pairFor(factory, output, path[i + 2]) : _to;
            pair.swap(amount0Out, amount1Out, to, new bytes(0));
        }
    }
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external virtual override ensure(deadline) {
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amountIn
        );
        uint balanceBefore = IERC20(path[path.length - 1]).balanceOf(to);
        _swapSupportingFeeOnTransferTokens(path, to);
        require(
            IERC20(path[path.length - 1]).balanceOf(to).sub(balanceBefore) >= amountOutMin,
            'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT'
        );
    }
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    )
        external
        virtual
        override
        payable
        ensure(deadline)
    {
        require(path[0] == WETH, 'UniswapV2Router: INVALID_PATH');
        uint amountIn = msg.value;
        IWETH(WETH).deposit{value: amountIn}();
        assert(IWETH(WETH).transfer(UniswapV2Library.pairFor(factory, path[0], path[1]), amountIn));
        uint balanceBefore = IERC20(path[path.length - 1]).balanceOf(to);
        _swapSupportingFeeOnTransferTokens(path, to);
        require(
            IERC20(path[path.length - 1]).balanceOf(to).sub(balanceBefore) >= amountOutMin,
            'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT'
        );
    }
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    )
        external
        virtual
        override
        ensure(deadline)
    {
        require(path[path.length - 1] == WETH, 'UniswapV2Router: INVALID_PATH');
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amountIn
        );
        _swapSupportingFeeOnTransferTokens(path, address(this));
        uint amountOut = IERC20(WETH).balanceOf(address(this));
        require(amountOut >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
        IWETH(WETH).withdraw(amountOut);
        TransferHelper.safeTransferETH(to, amountOut);
    }

    // **** LIBRARY FUNCTIONS ****
    function quote(uint amountA, uint reserveA, uint reserveB) public pure virtual override returns (uint amountB) {
        return UniswapV2Library.quote(amountA, reserveA, reserveB);
    }

    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut)
        public
        pure
        virtual
        override
        returns (uint amountOut)
    {
        return UniswapV2Library.getAmountOut(amountIn, reserveIn, reserveOut);
    }

    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut)
        public
        pure
        virtual
        override
        returns (uint amountIn)
    {
        return UniswapV2Library.getAmountIn(amountOut, reserveIn, reserveOut);
    }

    function getAmountsOut(uint amountIn, address[] memory path)
        public
        view
        virtual
        override
        returns (uint[] memory amounts)
    {
        return UniswapV2Library.getAmountsOut(factory, amountIn, path);
    }

    function getAmountsIn(uint amountOut, address[] memory path)
        public
        view
        virtual
        override
        returns (uint[] memory amounts)
    {
        return UniswapV2Library.getAmountsIn(factory, amountOut, path);
    }
}
```
:::

## SafeMath

:::details SafeMath
```solidity
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

# 最後に

今回の記事では、Bunzzの新機能『DeCipher』を使用して、UniswapV2の「**UniswapV2Router02**」のコントラクトを見てきました。
いかがだったでしょうか？
今後も特定のNFTやコントラクトをピックアップしてまとめて行きたいと思います。

普段はブログやQiitaでブロックチェーンやAIに関する記事を挙げているので、よければ見ていってください！

https://chaldene.net/

https://qiita.com/cardene
