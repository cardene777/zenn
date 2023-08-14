---
title: "[Bunzz Decipher] UniswapV2Pairのコントラクトを理解しよう！"
emoji: "🦄"
type: "tech"
topics:
  - "blockchain"
  - "ブロックチェーン"
  - "solidity"
  - "ethereum"
  - "defi"
published: true
published_at: "2023-08-10 07:43"
publication_name: "cryptogames"
---

# はじめに

初めまして。
**CryptoGames**というブロックチェーンゲーム企業でエンジニアをしている **cardene（かるでね）** です！
スマートコントラクトを書いたり、フロントエンド・バックエンド・インフラと幅広く触れています。

https://cryptogames.co.jp/

代表的なゲームは**クリプトスペルズ**というブロックチェーンゲームです。

https://cryptospells.jp/

今回はBunzzの新機能『**DeCipher**』を使用して、UniswapV2の「**UniswapV2Pair**」のコントラクトを見てみようと思います。

『**DeCipher**』はAIを使用してコントラクトのドキュメントを自動生成してくれるサービスです。

https://www.bunzz.dev/decipher

詳しい使い方に関しては以下の記事を参考にしてください！

https://zenn.dev/heku/articles/33266f0c19d523

:::message
この記事はあくまで個人が「**DeCipher**」を使用してみての記事になります。
間違った部分などもあるかと思いますが、その際はコメントなどで教えてください。
:::

今回使用する『**DeCipher**』のリンクは以下になります。

https://app.bunzz.dev/decipher/chains/1/addresses/0xA478c2975Ab1Ea89e8196811F51A7B7Ade33eB11

# 概要

## 要約

**UniswapV2Pair**コントラクトは、Ethereumブロックチェーン上に構築された分散型取引所 **Uniswap**プロトコルの重要な一部です。
このコントラクトは、一対の**ERC20**トークンの取引を管理および実行する役割を担っています。
**Uniswap V2**プロトコルの中核を成す部分であり、共通の基軸通貨を必要とせずに任意の二つの **ERC20**トークンを取引できるようにしています。

## 目的
**UniswapV2Pair**コントラクトの主な目的は、特定のトークンペアの流動性プールを管理することです。
このコントラクトは流動性の追加や削除、トークンのスワップを処理します。
また、トークンスワップの価格を計算するために、各トークンのリザーブ（保有量）を追跡します。

## コントラクトの責任
**UniswapV2Pair**コントラクトにはいくつかの主要な責任があります。

### トークン管理
このコントラクトは、`token0`および`token1`と呼ばれる一対の**ERC20**トークンを管理します。
これらのトークンは、Ethereumブロックチェーン上に存在する任意の**ERC20**トークンです。

### リザーブ管理
コントラクトは、トークンペアの両方のトークンのリザーブ（保有量）を記録します。
これらのリザーブは取引を実行し、価格を計算するために使用されます。
リザーブは、`reserve0`と`reserve1`というプライベート変数に保存されます。

### 価格の計算
コントラクトは、各トークンのリザーブに基づいてトークンスワップの価格を計算します。
また、累積価格変数（`price0CumulativeLast`および`price1CumulativeLast`）を維持し、トークンペアの時間加重平均価格（**TWAP**）の計算に使用されます。

### 流動性イベント
コントラクトは`kLast`変数を追跡します。
これは、最新の流動性イベント時点でのトークンペアのリザーブの積を表します。
これは流動性プロバイダーの手数料を計算するために使用されます。

### ロック機構
コントラクトには再入攻撃を防ぐためのロック機構が含まれています。
これは、`unlocked`変数と`lock`モディファイアを通じて実装されています。

### 承認と転送
コントラクトには、トークンの承認と転送を行うための関数が含まれています。
これらの関数（`approve`、`transfer`、`transferFrom`）は、**ERC20**トークンの標準的な機能であり、アドレス間でトークンを移動させることができます。

### 同期
`sync`関数は、取引または流動性イベントの後にリザーブと価格を更新するために使用されます。

### 初期化
`initialize`関数は、コントラクトが最初に作成されたときにトークンペアのアドレスを設定するために使用されます。

まとめると、**UniswapV2Pair**コントラクトは**Uniswap**プロトコルの基本的な部分です。
トークンペアのリザーブと価格を管理し、トークンの取引と移動を容易にします。
これは分散型取引所の動作において重要な役割を果たす、複雑なスマートコントラクトの一部です。

# 使い方

**UniswapV2Pair**コントラクトは、Ethereumブロックチェーン上の分散型取引所**Uniswap**プロトコルの中核をなす部分です。
このコントラクトは、二つの異なるトークンの流動性プールを管理し、それらのトークン間でのスワップを容易にするために使用されます。

以下に UniswapV2Pair コントラクトの使用手順をステップバイステップで説明します。

### 1. トークンペアの作成

最初のステップは、流動性を提供したりスワップしたいトークンのペアを作成することです。
これは、**IUniswapV2Factory**コントラクトの`createPair`関数を呼び出すことで行います。
この関数には二つの引数が必要で、トークンペアを作成したい二つのトークンのアドレスを渡します。

### 2. 流動性の追加

ペアを作成した後、各トークンの一定量をペアコントラクトに送信することで流動性を追加できます。トークンをコントラクトに送信するためには、`transfer`関数を使用します。

### 3. ペアコントラクトの承認

ペアコントラクトとの対話を開始する前に、トークンを使うためにペアコントラクトに対して承認が必要になります。
これは、各トークンコントラクトの`approve`関数を呼び出すことで行います。
ペアコントラクトのアドレスと承認したい金額を渡します。

### 4. トークンのスワップ

ペアコントラクトが承認されると、`swap`関数を呼び出すことでトークンをスワップできます。
この関数には以下の四つの引数が必要です。
- スワップしたい`token0`の量。
- スワップしたい`token1`の量。
- スワップしたトークンを送信するアドレス。
- 追加のデータ。

### 5. 流動性の削除

流動性をプールから削除したい場合は、`transfer`関数を呼び出して流動性トークンをペアコントラクトに送信します。
コントラクトはトークンを焼却し、元の資産の一部を返します。

### 6. 残高と承認額の確認

特定のアドレスの残高を確認するには、`balanceOf`関数を呼び出します。
同様に、特定のアドレスの承認額を確認するには、`allowance`関数を呼び出します。

スマートコントラクトとの対話には EthereumブロックチェーンとSolidityプログラミング言語の理解が必要です。
本番環境にデプロイする前に必ずテストネットでテストするようにしましょう。

# パラメータ

- `factory`
	- **UniswapV2Pair**コントラクトをデプロイした**UniswapV2Factory**コントラクトのアドレス。
- `token0`
	- **UniswapV2Pair**コントラクトが表すトークンペアの片方のアドレス。
- `token1`
	- **UniswapV2Pair**コントラクトが表すトークンペアのもう片方のアドレス。


# 関数

:::message
一般的な**ERC20**トークンの関数については以下の記事を参考にしてください。
:::

https://chaldene.net/erc20

## READ

### DOMAIN_SEPARATOR

```solidity
function DOMAIN_SEPARATOR() external view returns (bytes32);
```

`permit`関数の署名検証に使用されるドメインセパレータの値を取得する関数。
ドメインセパレータは、リプレイ攻撃を防ぐために使用されるコントラクト固有の識別子です。
コントラクトの名前、バージョン、およびチェーンIDを**keccak256**ハッシュ関数を使用してハッシュ化して計算されます。
この関数は、`bytes32`値であるドメインセパレータの値を返します。
ユーザーはこの関数を使用して、期待される値と返されたドメインセパレータの値を比較することで、`permit`関数呼び出しの署名を検証できます。
この関数は、`permit`関数呼び出しの整合性とセキュリティを確保するために役立ち、関数署名の正当性を検証する手段を提供します。

### MINIMUM_LIQUIDITY

```solidity
function MINIMUM_LIQUIDITY() external pure returns (uint);
```

`MINIMUM_LIQUIDITY`定数の値を取得する関数。
`MINIMUM_LIQUIDITY`定数は、流動性プールに対して生成される最小の流動性トークンの量を表します。
ユーザーが流動性を提供する際には、`token0`と`token1`の両方の等価な価値をデポジットすることで、流動性トークンが生成されます。
`MINIMUM_LIQUIDITY`の値は、流動性プールの初期化時に事前に設定される予め定義された値です。
`MINIMUM_LIQUIDITY`の値は通常、ゼロでの割り算エラーを防ぎ、流動性プールの正常な動作を確保するために小さな非ゼロの値となります。
ユーザーはこの関数を使用して、流動性をプールに提供する際に生成される最小の流動性トークンの量を取得することができます。

### PERMIT_TYPEHASH

```solidity
function PERMIT_TYPEHASH() external pure returns (bytes32);
```

`permit`関数のための型ハッシュを提供する関数。
型ハッシュは、`permit`関数の署名に対する固有の識別子であり、`EIP712`署名検証プロセスで使用されます。
この関数は`bytes32`型の`PERMIT_TYPEHASH`変数の値を返します。
この関数は、`permit`関数自体や、`permit`署名の検証や処理を行う関数など、`permit`関数の型ハッシュが必要なコントラクト内の他の関数によって使用されます。

### factory

```solidity
function factory() external view returns (address);
```

**Factory**コントラクトのアドレスを取得する関数。
**Factory**コントラクトは、このコントラクトの新しいインスタンスを作成します。
ユーザーはこの関数を呼び出してファクトリーコントラクトのアドレスを取得できます。
これは、コントラクトの正当性を確認したり、ファクトリーコントラクトと直接対話するためなど、さまざまな目的で役立ちます。

:::message
Factoryコントラクトについては以下を参照してください。
:::

https://zenn.dev/cryptogames/articles/89744d2e9629f4

### getReserves

```solidity
function getReserves() public view returns (uint112 _reserve0, uint112 _reserve1, uint32 _blockTimestampLast) {
        _reserve0 = reserve0;
        _reserve1 = reserve1;
        _blockTimestampLast = blockTimestampLast;
}
```

コントラクトの現在のリザーブとリザーブが最後に更新された時のブロックタイムスタンプを取得する関数。
関数は以下の3つの値を返します。

- `_reserve0`
	- `token0`の現在のリザーブ。
- `_reserve1`
	- `token1`の現在のリザーブ。
- `_blockTimestampLast`
	- リザーブが更新された最後のブロックのタイムスタンプ。

### kLast

```solidity
function kLast() external view returns (uint);
```

`kLast`変数の値を取得する関数。
`kLast`変数は、最後に流動性がプールに追加されたときの、`token0`と`token1`のリザーブの積の値を表します。
ユーザーはこの関数を使用して、`kLast`の値を取得できます。

### nonces

```solidity
function nonces(address owner) external view returns (uint);
```

特定のアドレスに関連する`nonce`値を取得する関数。
`nonce`値は、アドレスから送信されるごとに増加する固有の番号です。
これはリプレイ攻撃を防ぎ、トランザクションの整合性を確保するために使用されます。
関数は、指定されたアドレスをキーとして`nonces`マッピングにアクセスして`nonce`値を取得します。
このマッピングは、各アドレスの`nonce`値を保持します。
ユーザーはこの関数を使用して、自分自身のアドレスの`nonce`値やアクセス許可を持つ他のアドレスの`nonce`値を取得することができます。

### price0CumulativeLast

```solidity
function price0CumulativeLast() external view returns (uint);
```

`price0CumulativeLast`変数の値を取得する関数。
`price0CumulativeLast`変数は、`token0`と`token1`の間の累積価格を示すものです。
累積価格とは、`token0`の価格を一定の期間内で合計したものを指します。
この累積価格は、過去の時間にわたる価格の変動を示す重要な指標です。

:::message
累積価格は、特定の期間内での価格の変動を反映します。
具体的には、現在の時点での`token0`の価格を前回価格が更新されてからの時間経過と乗算して計算されます。
この計算により、過去の価格の変動が合計され、累積価格が得られます。
:::

この情報は、トークンの過去の価格変動を追跡し、投資判断を行う際に役立つ情報となります。

### price1CumulativeLast

```solidity
function price1CumulativeLast() external view returns (uint);
```

`price1CumulativeLast`変数の値を取得する関数。
`price1CumulativeLast`変数は、`token0`と`token1`の間の累積価格を示すものです。
累積価格とは、`token1`の価格を一定の期間内で合計したものを指します。
この累積価格は、過去の時間にわたる価格の変動を示す重要な指標です。

### token0

```solidity
function token0() external view returns (address);
```

`token0`変数の値を取得する関数。
ユーザーはこの関数を呼び出して、ペア内の最初のトークンのアドレスを取得できます。

### token1

```solidity
function token1() external view returns (address);
```

`token1`変数の値を取得する関数。
ユーザーはこの関数を呼び出して、ペア内の2番目のトークンのアドレスを取得できます。

## WRITE

### initialize

```solidity
function initialize(address _token0, address _token1) external {
        require(msg.sender == factory, 'UniswapV2: FORBIDDEN'); // sufficient check
        token0 = _token0;
        token1 = _token1;
}
```

流動性プールで使用される2つのトークンのアドレスを使用してコントラクトを初期化する関数。
`_token0`と`_token1`という2つのアドレスパラメータを受け取ります。

通常、ユーザーは新しいコントラクトのインスタンスをデプロイする際や新しい流動性プールをセットアップする際に、この関数を呼び出します。

### permit

```solidity
function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external {
        require(deadline >= block.timestamp, 'UniswapV2: EXPIRED');
        bytes32 digest = keccak256(
            abi.encodePacked(
                '\x19\x01',
                DOMAIN_SEPARATOR,
                keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, value, nonces[owner]++, deadline))
            )
        );
        address recoveredAddress = ecrecover(digest, v, r, s);
        require(recoveredAddress != address(0) && recoveredAddress == owner, 'UniswapV2: INVALID_SIGNATURE');
        _approve(owner, spender, value);
}
```

ユーザーが複数のトランザクションを必要とせずに、別のアドレスによるトークンの支出を承認するための関数。

支出を承認する前に、関数は現在のブロックのタイムスタンプと`deadline`を比較して`permit`が期限切れかどうかをチェックします。
もし`deadline`を過ぎている場合、関数は**'UniswapV2: EXPIRED'** エラーを返します。
その後、ドメインセパレータ、`permit`タイプハッシュ、`owner`、`spender`、`value`、`nonce`、`deadline`を含むパックされたデータをハッシュ化してダイジェストを計算します。
このダイジェストは、ECDSAの`recover`関数を使用して`permit`に署名したアドレスを復元するために使用されます。
復元されたアドレスがゼロではないことを確認し、オーナーのアドレスと一致することを検証します。
もし検証に失敗した場合、関数は**'UniswapV2: INVALID_SIGNATURE'** エラーを返します。
検証が成功した場合、関数は内部の`_approve`関数を呼び出して、`spender`の`allowance`を所有者のトークンに対して指定された値に設定します。

**引数**
- `owner`
	- トークンの所有者のアドレス。
- `spender`
	- トークンの支出を許可を与えるアカウントのアドレス。
- `value`
	- `spender`が使用できるトークンの量。
- `deadline`
	- `permit`の有効期限までのタイムスタンプ。
- `v`・`r`・`s`
	- ECDSA署名の構成要素で、`permit`を検証するために使用されます。

### skim

```solidity
function skim(address to) external lock {
        address _token0 = token0; // gas savings
        address _token1 = token1; // gas savings
        _safeTransfer(_token0, to, IERC20(_token0).balanceOf(address(this)).sub(reserve0));
        _safeTransfer(_token1, to, IERC20(_token1).balanceOf(address(this)).sub(reserve1));
}
```

コントラクトに蓄積した余剰のトークンを指定されたアドレスに送信する関数。

`to`アドレスがゼロアドレスでないことをチェックします。
もしゼロアドレスである場合、関数はエラーメッセージとともにリバート（取り消し）します。

各トークンに対して`balanceOf`関数を呼び出すことで、コントラクトの`token0`および`token1`トークンの現在の残高を取得します。

:::message
`balanceOf`関数はコントラクトが保持しているトークンの量を返します。
:::

もし`token0`または`token1`の残高がゼロより大きい場合、関数は`transfer`関数を使用して余剰のトークンを`to`アドレスに送信します。

この関数は、コントラクトから余剰のトークンを取り除き、指定されたアドレスに送信するために役立ちます。
ユーザーはこの関数を呼び出して、コントラクトに誤って送信された可能性のあるトークンを取り戻したり、特定のアドレスにトークンを配布したりすることができます。

この関数は、ユーザーが有効な`to`アドレスを提供すれば、どのユーザーでもいつでも呼び出すことができます。
この関数を呼び出すための特別な前提条件や制限はありません。

**引数**

- `to`
	- トークンを送付したいアドレス。


### swap

```solidity
function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external lock {
        require(amount0Out > 0 || amount1Out > 0, 'UniswapV2: INSUFFICIENT_OUTPUT_AMOUNT');
        (uint112 _reserve0, uint112 _reserve1,) = getReserves(); // gas savings
        require(amount0Out < _reserve0 && amount1Out < _reserve1, 'UniswapV2: INSUFFICIENT_LIQUIDITY');

        uint balance0;
        uint balance1;
        { // scope for _token{0,1}, avoids stack too deep errors
        address _token0 = token0;
        address _token1 = token1;
        require(to != _token0 && to != _token1, 'UniswapV2: INVALID_TO');
        if (amount0Out > 0) _safeTransfer(_token0, to, amount0Out); // optimistically transfer tokens
        if (amount1Out > 0) _safeTransfer(_token1, to, amount1Out); // optimistically transfer tokens
        if (data.length > 0) IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);
        balance0 = IERC20(_token0).balanceOf(address(this));
        balance1 = IERC20(_token1).balanceOf(address(this));
        }
        uint amount0In = balance0 > _reserve0 - amount0Out ? balance0 - (_reserve0 - amount0Out) : 0;
        uint amount1In = balance1 > _reserve1 - amount1Out ? balance1 - (_reserve1 - amount1Out) : 0;
        require(amount0In > 0 || amount1In > 0, 'UniswapV2: INSUFFICIENT_INPUT_AMOUNT');
        { // scope for reserve{0,1}Adjusted, avoids stack too deep errors
        uint balance0Adjusted = balance0.mul(1000).sub(amount0In.mul(3));
        uint balance1Adjusted = balance1.mul(1000).sub(amount1In.mul(3));
        require(balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(1000**2), 'UniswapV2: K');
        }

        _update(balance0, balance1, _reserve0, _reserve1);
        emit Swap(msg.sender, amount0In, amount1In, amount0Out, amount1Out, to);
}
```

ユーザーが**UniswapV2**コントラクト内でトークンを交換するための関数。

ユーザーは、この関数を使用して**UniswapV2**コントラクト内でトークンを交換することができます。
具体的には、トークン0とトークン1の交換する数量を指定し、2つのトークンを交換して指定された受取人のアドレスにトークンを送ります。

**引数**

- `amount0Out`
	- トークン0を交換する数量。
- `amount1Out`
	- トークン1を交換する数量。
- `to`
	- トークンの受取アドレス。
- `data`
	- 追加データを表すバイト配列。

### sync

```solidity
function sync() external lock {
        _update(IERC20(token0).balanceOf(address(this)), IERC20(token1).balanceOf(address(this)), reserve0, reserve1);
}
```

**UniswapV2**のペアコントラクト内で、トークンのリザーブと累積価格値を最新の情報に更新する関数。

関数が呼び出されると、まず**UniswapV2**のペアコントラクト自体が呼び出し元であるかどうかを確認します。
もし呼び出し元がペアコントラクトでない場合、関数は操作を取り消して元の状態に戻ります。

その後、ペアコントラクト内で保持されているトークン0とトークン1の現在のリザーブを取得します。
次に、関数は**Uniswap Oracle**コントラクトから得られた最新の累積価格値と、ペアコントラクト内で保存されている前回の累積価格値を用いて、トークン0とトークン1の最新の累積価格値を計算します。

計算された最新の累積価格値で、ペアコントラクト内で保存されている累積価格値を更新します。

これにより、正確な価格計算と**Uniswap**プラットフォーム上での取引が可能になります。

# イベント

## Swap

分散型取引所で2つのトークン間でスワップが行われるたびにトリガーされるイベント。
このイベントには6つのパラメータがあります。

- `sender`
	- スワップを開始する送信者のアドレス。
- `amount0In`
	- スワップに入力されるトークン0の数量。
- `amount1In`
	- スワップに入力されるトークン1の数量。
- `amount0Out`
	- スワップから出力されるトークン0の数量。
- `amount1Out`
	- スワップから出力されるトークン1の数量。
- `to`
	- 出力トークンを受け取るアドレス。

このイベントは`sender`と`to`のパラメータに対してインデックスが付けられており、これによってこれらのパラメータを使用して特定のスワップをイベントログからクエリする際にフィルタとして使用できます。
要約すると、`Swap`イベントは分散型取引所で発生するトークンのスワップに関する重要な情報を提供し、送信者、入力および出力トークンの数量、出力トークンの受取人などが含まれます。

## Sync

**Uniswap**のトークンペアの準備が調整される度に発生するイベント。
ユーザーが流動性や取引活動の変化を追跡できるように情報を提供します。
このイベントには2つのパラメータがあります。

- `reserve0`
	- 最初のトークンのリザーブの更新された値。
- `reserve1`
	- 2番目のトークンのリザーブの更新された値。


# コード

## IUniswapV2Pair

:::details IUniswapV2Pair

```solidity
// File: contracts/interfaces/IUniswapV2Pair.sol

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

## IUniswapV2ERC20

:::details IUniswapV2ERC20

```solidity
// File: contracts/interfaces/IUniswapV2ERC20.sol

pragma solidity >=0.5.0;

interface IUniswapV2ERC20 {
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
}
```
:::

## SafeMath

:::details SafeMath
```solidity
// File: contracts/libraries/SafeMath.sol

pragma solidity =0.5.16;

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

## UniswapV2ERC20

:::details UniswapV2ERC20
```solidity
// File: contracts/UniswapV2ERC20.sol

pragma solidity =0.5.16;



contract UniswapV2ERC20 is IUniswapV2ERC20 {
    using SafeMath for uint;

    string public constant name = 'Uniswap V2';
    string public constant symbol = 'UNI-V2';
    uint8 public constant decimals = 18;
    uint  public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    bytes32 public DOMAIN_SEPARATOR;
    // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant PERMIT_TYPEHASH = 0x6e71edae12b1b97f4d1f60370fef10105fa2faae0126114a169c64845d6126c9;
    mapping(address => uint) public nonces;

    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    constructor() public {
        uint chainId;
        assembly {
            chainId := chainid
        }
        DOMAIN_SEPARATOR = keccak256(
            abi.encode(
                keccak256('EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)'),
                keccak256(bytes(name)),
                keccak256(bytes('1')),
                chainId,
                address(this)
            )
        );
    }

    function _mint(address to, uint value) internal {
        totalSupply = totalSupply.add(value);
        balanceOf[to] = balanceOf[to].add(value);
        emit Transfer(address(0), to, value);
    }

    function _burn(address from, uint value) internal {
        balanceOf[from] = balanceOf[from].sub(value);
        totalSupply = totalSupply.sub(value);
        emit Transfer(from, address(0), value);
    }

    function _approve(address owner, address spender, uint value) private {
        allowance[owner][spender] = value;
        emit Approval(owner, spender, value);
    }

    function _transfer(address from, address to, uint value) private {
        balanceOf[from] = balanceOf[from].sub(value);
        balanceOf[to] = balanceOf[to].add(value);
        emit Transfer(from, to, value);
    }

    function approve(address spender, uint value) external returns (bool) {
        _approve(msg.sender, spender, value);
        return true;
    }

    function transfer(address to, uint value) external returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint value) external returns (bool) {
        if (allowance[from][msg.sender] != uint(-1)) {
            allowance[from][msg.sender] = allowance[from][msg.sender].sub(value);
        }
        _transfer(from, to, value);
        return true;
    }

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external {
        require(deadline >= block.timestamp, 'UniswapV2: EXPIRED');
        bytes32 digest = keccak256(
            abi.encodePacked(
                '\x19\x01',
                DOMAIN_SEPARATOR,
                keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, value, nonces[owner]++, deadline))
            )
        );
        address recoveredAddress = ecrecover(digest, v, r, s);
        require(recoveredAddress != address(0) && recoveredAddress == owner, 'UniswapV2: INVALID_SIGNATURE');
        _approve(owner, spender, value);
    }
}
```
:::

## Math・UQ112x112

:::details Math・UQ112x112
```solidity
// File: contracts/libraries/Math.sol

pragma solidity =0.5.16;

// a library for performing various math operations

library Math {
    function min(uint x, uint y) internal pure returns (uint z) {
        z = x < y ? x : y;
    }

    // babylonian method (https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method)
    function sqrt(uint y) internal pure returns (uint z) {
        if (y > 3) {
            z = y;
            uint x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

// File: contracts/libraries/UQ112x112.sol

pragma solidity =0.5.16;

// a library for handling binary fixed point numbers (https://en.wikipedia.org/wiki/Q_(number_format))

// range: [0, 2**112 - 1]
// resolution: 1 / 2**112

library UQ112x112 {
    uint224 constant Q112 = 2**112;

    // encode a uint112 as a UQ112x112
    function encode(uint112 y) internal pure returns (uint224 z) {
        z = uint224(y) * Q112; // never overflows
    }

    // divide a UQ112x112 by a uint112, returning a UQ112x112
    function uqdiv(uint224 x, uint112 y) internal pure returns (uint224 z) {
        z = x / uint224(y);
    }
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

## IUniswapV2Factory

:::details IUniswapV2Factory
```solidity
// File: contracts/interfaces/IUniswapV2Factory.sol

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

## IUniswapV2Callee

:::details IUniswapV2Callee
```solidity
// File: contracts/interfaces/IUniswapV2Callee.sol

pragma solidity >=0.5.0;

interface IUniswapV2Callee {
    function uniswapV2Call(address sender, uint amount0, uint amount1, bytes calldata data) external;
}
```
:::

## UniswapV2Pair

:::details UniswapV2Pair
```solidity
// File: contracts/interfaces/IUniswapV2Pair.sol

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

// File: contracts/interfaces/IUniswapV2ERC20.sol

pragma solidity >=0.5.0;

interface IUniswapV2ERC20 {
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
}

// File: contracts/libraries/SafeMath.sol

pragma solidity =0.5.16;

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

// File: contracts/UniswapV2ERC20.sol

pragma solidity =0.5.16;



contract UniswapV2ERC20 is IUniswapV2ERC20 {
    using SafeMath for uint;

    string public constant name = 'Uniswap V2';
    string public constant symbol = 'UNI-V2';
    uint8 public constant decimals = 18;
    uint  public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    bytes32 public DOMAIN_SEPARATOR;
    // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant PERMIT_TYPEHASH = 0x6e71edae12b1b97f4d1f60370fef10105fa2faae0126114a169c64845d6126c9;
    mapping(address => uint) public nonces;

    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    constructor() public {
        uint chainId;
        assembly {
            chainId := chainid
        }
        DOMAIN_SEPARATOR = keccak256(
            abi.encode(
                keccak256('EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)'),
                keccak256(bytes(name)),
                keccak256(bytes('1')),
                chainId,
                address(this)
            )
        );
    }

    function _mint(address to, uint value) internal {
        totalSupply = totalSupply.add(value);
        balanceOf[to] = balanceOf[to].add(value);
        emit Transfer(address(0), to, value);
    }

    function _burn(address from, uint value) internal {
        balanceOf[from] = balanceOf[from].sub(value);
        totalSupply = totalSupply.sub(value);
        emit Transfer(from, address(0), value);
    }

    function _approve(address owner, address spender, uint value) private {
        allowance[owner][spender] = value;
        emit Approval(owner, spender, value);
    }

    function _transfer(address from, address to, uint value) private {
        balanceOf[from] = balanceOf[from].sub(value);
        balanceOf[to] = balanceOf[to].add(value);
        emit Transfer(from, to, value);
    }

    function approve(address spender, uint value) external returns (bool) {
        _approve(msg.sender, spender, value);
        return true;
    }

    function transfer(address to, uint value) external returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint value) external returns (bool) {
        if (allowance[from][msg.sender] != uint(-1)) {
            allowance[from][msg.sender] = allowance[from][msg.sender].sub(value);
        }
        _transfer(from, to, value);
        return true;
    }

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external {
        require(deadline >= block.timestamp, 'UniswapV2: EXPIRED');
        bytes32 digest = keccak256(
            abi.encodePacked(
                '\x19\x01',
                DOMAIN_SEPARATOR,
                keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, value, nonces[owner]++, deadline))
            )
        );
        address recoveredAddress = ecrecover(digest, v, r, s);
        require(recoveredAddress != address(0) && recoveredAddress == owner, 'UniswapV2: INVALID_SIGNATURE');
        _approve(owner, spender, value);
    }
}

// File: contracts/libraries/Math.sol

pragma solidity =0.5.16;

// a library for performing various math operations

library Math {
    function min(uint x, uint y) internal pure returns (uint z) {
        z = x < y ? x : y;
    }

    // babylonian method (https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method)
    function sqrt(uint y) internal pure returns (uint z) {
        if (y > 3) {
            z = y;
            uint x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

// File: contracts/libraries/UQ112x112.sol

pragma solidity =0.5.16;

// a library for handling binary fixed point numbers (https://en.wikipedia.org/wiki/Q_(number_format))

// range: [0, 2**112 - 1]
// resolution: 1 / 2**112

library UQ112x112 {
    uint224 constant Q112 = 2**112;

    // encode a uint112 as a UQ112x112
    function encode(uint112 y) internal pure returns (uint224 z) {
        z = uint224(y) * Q112; // never overflows
    }

    // divide a UQ112x112 by a uint112, returning a UQ112x112
    function uqdiv(uint224 x, uint112 y) internal pure returns (uint224 z) {
        z = x / uint224(y);
    }
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

// File: contracts/interfaces/IUniswapV2Factory.sol

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

// File: contracts/interfaces/IUniswapV2Callee.sol

pragma solidity >=0.5.0;

interface IUniswapV2Callee {
    function uniswapV2Call(address sender, uint amount0, uint amount1, bytes calldata data) external;
}

// File: contracts/UniswapV2Pair.sol

pragma solidity =0.5.16;

contract UniswapV2Pair is IUniswapV2Pair, UniswapV2ERC20 {
    using SafeMath  for uint;
    using UQ112x112 for uint224;

    uint public constant MINIMUM_LIQUIDITY = 10**3;
    bytes4 private constant SELECTOR = bytes4(keccak256(bytes('transfer(address,uint256)')));

    address public factory;
    address public token0;
    address public token1;

    uint112 private reserve0;           // uses single storage slot, accessible via getReserves
    uint112 private reserve1;           // uses single storage slot, accessible via getReserves
    uint32  private blockTimestampLast; // uses single storage slot, accessible via getReserves

    uint public price0CumulativeLast;
    uint public price1CumulativeLast;
    uint public kLast; // reserve0 * reserve1, as of immediately after the most recent liquidity event

    uint private unlocked = 1;
    modifier lock() {
        require(unlocked == 1, 'UniswapV2: LOCKED');
        unlocked = 0;
        _;
        unlocked = 1;
    }

    function getReserves() public view returns (uint112 _reserve0, uint112 _reserve1, uint32 _blockTimestampLast) {
        _reserve0 = reserve0;
        _reserve1 = reserve1;
        _blockTimestampLast = blockTimestampLast;
    }

    function _safeTransfer(address token, address to, uint value) private {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(SELECTOR, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'UniswapV2: TRANSFER_FAILED');
    }

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

    constructor() public {
        factory = msg.sender;
    }

    // called once by the factory at time of deployment
    function initialize(address _token0, address _token1) external {
        require(msg.sender == factory, 'UniswapV2: FORBIDDEN'); // sufficient check
        token0 = _token0;
        token1 = _token1;
    }

    // update reserves and, on the first call per block, price accumulators
    function _update(uint balance0, uint balance1, uint112 _reserve0, uint112 _reserve1) private {
        require(balance0 <= uint112(-1) && balance1 <= uint112(-1), 'UniswapV2: OVERFLOW');
        uint32 blockTimestamp = uint32(block.timestamp % 2**32);
        uint32 timeElapsed = blockTimestamp - blockTimestampLast; // overflow is desired
        if (timeElapsed > 0 && _reserve0 != 0 && _reserve1 != 0) {
            // * never overflows, and + overflow is desired
            price0CumulativeLast += uint(UQ112x112.encode(_reserve1).uqdiv(_reserve0)) * timeElapsed;
            price1CumulativeLast += uint(UQ112x112.encode(_reserve0).uqdiv(_reserve1)) * timeElapsed;
        }
        reserve0 = uint112(balance0);
        reserve1 = uint112(balance1);
        blockTimestampLast = blockTimestamp;
        emit Sync(reserve0, reserve1);
    }

    // if fee is on, mint liquidity equivalent to 1/6th of the growth in sqrt(k)
    function _mintFee(uint112 _reserve0, uint112 _reserve1) private returns (bool feeOn) {
        address feeTo = IUniswapV2Factory(factory).feeTo();
        feeOn = feeTo != address(0);
        uint _kLast = kLast; // gas savings
        if (feeOn) {
            if (_kLast != 0) {
                uint rootK = Math.sqrt(uint(_reserve0).mul(_reserve1));
                uint rootKLast = Math.sqrt(_kLast);
                if (rootK > rootKLast) {
                    uint numerator = totalSupply.mul(rootK.sub(rootKLast));
                    uint denominator = rootK.mul(5).add(rootKLast);
                    uint liquidity = numerator / denominator;
                    if (liquidity > 0) _mint(feeTo, liquidity);
                }
            }
        } else if (_kLast != 0) {
            kLast = 0;
        }
    }

    // this low-level function should be called from a contract which performs important safety checks
    function mint(address to) external lock returns (uint liquidity) {
        (uint112 _reserve0, uint112 _reserve1,) = getReserves(); // gas savings
        uint balance0 = IERC20(token0).balanceOf(address(this));
        uint balance1 = IERC20(token1).balanceOf(address(this));
        uint amount0 = balance0.sub(_reserve0);
        uint amount1 = balance1.sub(_reserve1);

        bool feeOn = _mintFee(_reserve0, _reserve1);
        uint _totalSupply = totalSupply; // gas savings, must be defined here since totalSupply can update in _mintFee
        if (_totalSupply == 0) {
            liquidity = Math.sqrt(amount0.mul(amount1)).sub(MINIMUM_LIQUIDITY);
           _mint(address(0), MINIMUM_LIQUIDITY); // permanently lock the first MINIMUM_LIQUIDITY tokens
        } else {
            liquidity = Math.min(amount0.mul(_totalSupply) / _reserve0, amount1.mul(_totalSupply) / _reserve1);
        }
        require(liquidity > 0, 'UniswapV2: INSUFFICIENT_LIQUIDITY_MINTED');
        _mint(to, liquidity);

        _update(balance0, balance1, _reserve0, _reserve1);
        if (feeOn) kLast = uint(reserve0).mul(reserve1); // reserve0 and reserve1 are up-to-date
        emit Mint(msg.sender, amount0, amount1);
    }

    // this low-level function should be called from a contract which performs important safety checks
    function burn(address to) external lock returns (uint amount0, uint amount1) {
        (uint112 _reserve0, uint112 _reserve1,) = getReserves(); // gas savings
        address _token0 = token0;                                // gas savings
        address _token1 = token1;                                // gas savings
        uint balance0 = IERC20(_token0).balanceOf(address(this));
        uint balance1 = IERC20(_token1).balanceOf(address(this));
        uint liquidity = balanceOf[address(this)];

        bool feeOn = _mintFee(_reserve0, _reserve1);
        uint _totalSupply = totalSupply; // gas savings, must be defined here since totalSupply can update in _mintFee
        amount0 = liquidity.mul(balance0) / _totalSupply; // using balances ensures pro-rata distribution
        amount1 = liquidity.mul(balance1) / _totalSupply; // using balances ensures pro-rata distribution
        require(amount0 > 0 && amount1 > 0, 'UniswapV2: INSUFFICIENT_LIQUIDITY_BURNED');
        _burn(address(this), liquidity);
        _safeTransfer(_token0, to, amount0);
        _safeTransfer(_token1, to, amount1);
        balance0 = IERC20(_token0).balanceOf(address(this));
        balance1 = IERC20(_token1).balanceOf(address(this));

        _update(balance0, balance1, _reserve0, _reserve1);
        if (feeOn) kLast = uint(reserve0).mul(reserve1); // reserve0 and reserve1 are up-to-date
        emit Burn(msg.sender, amount0, amount1, to);
    }

    // this low-level function should be called from a contract which performs important safety checks
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external lock {
        require(amount0Out > 0 || amount1Out > 0, 'UniswapV2: INSUFFICIENT_OUTPUT_AMOUNT');
        (uint112 _reserve0, uint112 _reserve1,) = getReserves(); // gas savings
        require(amount0Out < _reserve0 && amount1Out < _reserve1, 'UniswapV2: INSUFFICIENT_LIQUIDITY');

        uint balance0;
        uint balance1;
        { // scope for _token{0,1}, avoids stack too deep errors
        address _token0 = token0;
        address _token1 = token1;
        require(to != _token0 && to != _token1, 'UniswapV2: INVALID_TO');
        if (amount0Out > 0) _safeTransfer(_token0, to, amount0Out); // optimistically transfer tokens
        if (amount1Out > 0) _safeTransfer(_token1, to, amount1Out); // optimistically transfer tokens
        if (data.length > 0) IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);
        balance0 = IERC20(_token0).balanceOf(address(this));
        balance1 = IERC20(_token1).balanceOf(address(this));
        }
        uint amount0In = balance0 > _reserve0 - amount0Out ? balance0 - (_reserve0 - amount0Out) : 0;
        uint amount1In = balance1 > _reserve1 - amount1Out ? balance1 - (_reserve1 - amount1Out) : 0;
        require(amount0In > 0 || amount1In > 0, 'UniswapV2: INSUFFICIENT_INPUT_AMOUNT');
        { // scope for reserve{0,1}Adjusted, avoids stack too deep errors
        uint balance0Adjusted = balance0.mul(1000).sub(amount0In.mul(3));
        uint balance1Adjusted = balance1.mul(1000).sub(amount1In.mul(3));
        require(balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(1000**2), 'UniswapV2: K');
        }

        _update(balance0, balance1, _reserve0, _reserve1);
        emit Swap(msg.sender, amount0In, amount1In, amount0Out, amount1Out, to);
    }

    // force balances to match reserves
    function skim(address to) external lock {
        address _token0 = token0; // gas savings
        address _token1 = token1; // gas savings
        _safeTransfer(_token0, to, IERC20(_token0).balanceOf(address(this)).sub(reserve0));
        _safeTransfer(_token1, to, IERC20(_token1).balanceOf(address(this)).sub(reserve1));
    }

    // force reserves to match balances
    function sync() external lock {
        _update(IERC20(token0).balanceOf(address(this)), IERC20(token1).balanceOf(address(this)), reserve0, reserve1);
    }
}
```
:::

# 最後に

今回の記事では、Bunzzの新機能『**DeCipher**』を使用して、**UniswapV2**の「**UniswapV2Pair**」のコントラクトを見てきました。
いかがだったでしょうか？
今後も特定のNFTやコントラクトをピックアップしてまとめて行きたいと思います。

普段はブログやQiitaでブロックチェーンやAIに関する記事を挙げているので、よければ見ていってください！

https://chaldene.net/

https://qiita.com/cardene
