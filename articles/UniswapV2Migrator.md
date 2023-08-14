---
title: "[Bunzz Decipher] UniswapV2Migratorのコントラクトを理解しよう！"
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

https://app.bunzz.dev/decipher/chains/1/addresses/0x16d4f26c15f3658ec65b1126ff27dd3df2a2996b

# 概要

**UniswapV2Migrator**コントラクトは、Ethereumブロックチェーン上に構築された分散型取引所（DEX）である**Uniswap**プロトコルの重要なコンポーネントです。
このコントラクトは、**UniswapV1**から**UniswapV2**への流動性の移行をスムーズかつ効率的に行うために設計されています。

## コントラクトの目的
**UniswapV2Migrator**コントラクトの主な目的は、流動性プロバイダーが**UniswapV1**から**UniswapV2**への流動性をシームレスかつ効率的に移行する手段を提供することです。
この移行は、**Uniswap**プロトコルのバージョン1からバージョン2へのアップグレードに伴い、いくつかの新機能と改善が導入されたため必要とされています。

移行プロセスは、V1取引所から流動性を取り出し、それをV2取引所に追加することを含みます。
これにより、流動性プロバイダーは流動性プールの同じ割合を保持し、プロセス中に資金を失うことはありません。

## コントラクトの責任
**UniswapV2Migrator**コントラクトにはいくつかの重要な責任があります。

### Uniswap V1およびV2との連携
このコントラクトは、**Uniswap**プロトコルのV1およびV2の両方と連携します。
特定のトークンの取引所をV1ファクトリーから取得し、V2ルーターを使用してV2取引所に流動性を追加します。

### 流動性の移動
コントラクトは、V1取引所からV2取引所への流動性の移動を処理します。
これには、V1取引所から流動性を取り出し、それをV2取引所に追加する作業が含まれます。

### 安全な移行の確保
コントラクトは、移行プロセスが安全でセキュアであることを確認します。
V1取引所からの流動性の移動が成功したかどうかをチェックし、`safeApprove`関数を使用してV2ルーターにトークンの使用許可をセキュアに付与します。

### ETHの処理
コントラクトには、**ETH**の受け取りを許可する`receive`関数があります。
これは、移行プロセスがトークンを**ETH**にスワップし、それをV2取引所に流動性を追加するために使用するため必要です。

まとめると、**UniswapV2Migrator**コントラクトは、**Uniswap**プロトコルにおいてV1からV2への流動性の移行を円滑にサポートする重要な役割を果たしています。
これにより、移行プロセスが安全でセキュアであり、**Uniswap**エコシステムのインテグリティが維持されています。

# 使い方

**UniswapV2Migrator**コントラクトは、**UniswapV1**から**UniswapV2**への流動性の移行をサポートするために設計されました。
このコントラクトは、更新されたプロトコルに流動性を移動させたい流動性プロバイダーによって主に使用されます。

以下は、**UniswapV2Migrator**コントラクトを使用する手順です。

### 1. コントラクトのデプロイ
最初のステップは、**UniswapV2Migrator**コントラクトをEthereumネットワークにデプロイすることです。
これには、**UniswapV1**ファクトリーと**UniswapV2**ルーターのアドレスがコンストラクタ引数として必要です。

### 2. 移行の準備
`migrate`関数を呼び出す前に、流動性プロバイダーは**UniswapV1**取引所からの流動性トークンの移動を**UniswapV2Migrator**コントラクトに承認する必要があります。

### 3. Migrate関数の呼び出し
流動性プロバイダーは、`migrate`関数を呼び出すことができます。
この関数には、以下のパラメータが必要です。

- `token`
	- 流動性ペアのトークンのアドレス。
- `amountTokenMin`
	- 流動性プールに追加されるべきトークンの最小量。
- `amountETHMin`
	- 流動性プールに追加されるべきETHの最小量。
- `to`
	- 新しい流動性トークンが送られるアドレス。
- `deadline`
	- 移行が完了する必要がある時刻。

### 4. 移行プロセス
`migrate`関数は、以下の手順を実行します。

- 呼び出し元が**UniswapV1**取引所に持っている流動性の量を取得します。
- この流動性を自身に送ります。
- **UniswapV1**取引所から流動性を取り出し、その代わりに基礎となるトークンを受け取ります。
- **UniswapV2**ルーターがこれらのトークンを使用することを承認します。
- トークンと**ETH**を**UniswapV2**取引所に追加し、代わりに流動性トークンを受け取ります。

### 5. 流動性トークンの受け取り
移行プロセスが完了すると、新しい流動性トークンは`migrate`関数の呼び出しで指定した`to`アドレスに送られます。

注意点として、移行プロセスは複数のトランザクションを必要とし、ガス料金をカバーするために十分なETHが必要です。
また、移行プロセスは取り消すことができません。
一旦流動性が**UniswapV2**に移行されると、**UniswapV1**に戻すことはできません。

# パラメータ

## _factoryV1

**UniswapV1**ファクトリーコントラクトのアドレスです。
このアドレスは、**UniswapV1**の取引所を作成し管理するために使用されます。
**UniswapV1**ファクトリーコントラクトは、異なるトークン間の流動性プールを作成し、取引所をセットアップする際に使用される重要な役割を果たします。

## _router

**UniswapV2**ルーターコントラクトのアドレスです。
このアドレスは、**UniswapV2**プラットフォーム上でのトークンのスワップや流動性の提供を実行するために使用されます。
**UniswapV2**ルーターコントラクトは、トークン間のスワップ取引を効率的に処理し、流動性を提供するためのトランザクションを実行する際に重要な機能を果たします。

# 関数

## WRITE

### migrate

**UniswapV1**から**UniswapV2**に流動性を移行する関数。

**引数**

- `token`
	- 移行するトークンのアドレス。
- `amountTokenMin`
	- 移行時にユーザーが受け取りたい最小量のトークン。
- `amountETHMin`
	- 移行時にユーザーが受け取りたい最小量のETH。
- `to`
	- 移行された流動性が送られるアドレス。
- `deadline`
	- 移行操作が行われる期限。

この関数は、最初に指定されたトークンに対する**UniswapV1**取引所コントラクトを`factoryV1.getExchange`関数を使用して取得します。
次に、関数は呼び出し元の**UniswapV1**取引所コントラクトの残高をチェックします。

その後、**UniswapV1**取引所コントラクトから呼び出し元の流動性を現在のコントラクトに`transferFrom`関数を使用して移動します。

その後、`removeLiquidity`関数を使用して**UniswapV1**取引所コントラクトから流動性を取り出します。

`safeApprove`関数を使用して`amountTokenV1`トークンの**UniswapV2**ルーターコントラクトへの送金を承認します。

次に、関数はルーターコントラクトの`addLiquidityETH`関数を使用して**UniswapV2**に流動性を追加します。
`amountETHV1`**ETH**が流動性の追加に使用されます。

もし`amountTokenV1`が`amountTokenV2`よりも大きい場合、関数は`safeApprove`関数を使用してトークンの承認を`0`にリセットし、`safeTransfer`関数を使用して余剰のトークンを呼び出し元に送金します。

もし`amountETHV1`が`amountETHV2`よりも大きい場合、関数は`safeTransferETH`関数を使用して余剰の**ETH**を呼び出し元に送金します。

このように、ユーザーが**UniswapV1**から**UniswapV2**に流動性を移行し、**UniswapV2**が提供する機能と利点を活用するために使用されます。

:::message
`deadline`は、流動性の移行操作が行われる期限を指定するためのパラメータです。
この期限は、取引の実行がある一定の期間内に完了することを保証するために使用されます。
指定された期限（Unixタイムスタンプ）までに処理が完了しない場合、関数は実行されずにエラーが返されます。この仕組みにより、流動性の移行操作がある程度の時間内に確実に実行されることが保証されます。

たとえば、ユーザーが「`migrate`」関数を呼び出す際に、期限を1時間後のUnixタイムスタンプとして指定した場合、1時間以内に移行操作が完了しない場合はエラーが発生します。
これにより、移行操作が適切な時間内に実行されることが確認され、トランザクションの確定性が向上します。
:::

# イベント

特になし。

# コード

## IUniswapV2Migrator

:::details IUniswapV2Migrator
```solidity
pragma solidity =0.6.6;

interface IUniswapV2Migrator {
    function migrate(address token, uint amountTokenMin, uint amountETHMin, address to, uint deadline) external;
}
```
:::

## IUniswapV1Factory

:::details IUniswapV1Factory
```solidity
interface IUniswapV1Factory {
    function getExchange(address) external view returns (address);
}
```
:::

## IUniswapV1Exchange

:::details IUniswapV1Exchange
```solidity
interface IUniswapV1Exchange {
    function balanceOf(address owner) external view returns (uint);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function removeLiquidity(uint, uint, uint, uint) external returns (uint, uint);
    function tokenToEthSwapInput(uint, uint, uint) external returns (uint);
    function ethToTokenSwapInput(uint, uint) external payable returns (uint);
}
```
:::

## IUniswapV2Router01

:::details IUniswapV2Router01
```solidity
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

## UniswapV2Migrator

:::details UniswapV2Migrator
```solidity
contract UniswapV2Migrator is IUniswapV2Migrator {
    IUniswapV1Factory immutable factoryV1;
    IUniswapV2Router01 immutable router;

    constructor(address _factoryV1, address _router) public {
        factoryV1 = IUniswapV1Factory(_factoryV1);
        router = IUniswapV2Router01(_router);
    }

    // needs to accept ETH from any v1 exchange and the router. ideally this could be enforced, as in the router,
    // but it's not possible because it requires a call to the v1 factory, which takes too much gas
    receive() external payable {}

    function migrate(address token, uint amountTokenMin, uint amountETHMin, address to, uint deadline)
        external
        override
    {
        IUniswapV1Exchange exchangeV1 = IUniswapV1Exchange(factoryV1.getExchange(token));
        uint liquidityV1 = exchangeV1.balanceOf(msg.sender);
        require(exchangeV1.transferFrom(msg.sender, address(this), liquidityV1), 'TRANSFER_FROM_FAILED');
        (uint amountETHV1, uint amountTokenV1) = exchangeV1.removeLiquidity(liquidityV1, 1, 1, uint(-1));
        TransferHelper.safeApprove(token, address(router), amountTokenV1);
        (uint amountTokenV2, uint amountETHV2,) = router.addLiquidityETH{value: amountETHV1}(
            token,
            amountTokenV1,
            amountTokenMin,
            amountETHMin,
            to,
            deadline
        );
        if (amountTokenV1 > amountTokenV2) {
            TransferHelper.safeApprove(token, address(router), 0); // be a good blockchain citizen, reset allowance to 0
            TransferHelper.safeTransfer(token, msg.sender, amountTokenV1 - amountTokenV2);
        } else if (amountETHV1 > amountETHV2) {
            // addLiquidityETH guarantees that all of amountETHV1 or amountTokenV1 will be used, hence this else is safe
            TransferHelper.safeTransferETH(msg.sender, amountETHV1 - amountETHV2);
        }
    }
}
```
:::

## TransferHelper

:::details TransferHelper
```solidity
pragma solidity =0.6.6;

interface IUniswapV2Migrator {
    function migrate(address token, uint amountTokenMin, uint amountETHMin, address to, uint deadline) external;
}

interface IUniswapV1Factory {
    function getExchange(address) external view returns (address);
}

interface IUniswapV1Exchange {
    function balanceOf(address owner) external view returns (uint);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function removeLiquidity(uint, uint, uint, uint) external returns (uint, uint);
    function tokenToEthSwapInput(uint, uint, uint) external returns (uint);
    function ethToTokenSwapInput(uint, uint) external payable returns (uint);
}

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

contract UniswapV2Migrator is IUniswapV2Migrator {
    IUniswapV1Factory immutable factoryV1;
    IUniswapV2Router01 immutable router;

    constructor(address _factoryV1, address _router) public {
        factoryV1 = IUniswapV1Factory(_factoryV1);
        router = IUniswapV2Router01(_router);
    }

    // needs to accept ETH from any v1 exchange and the router. ideally this could be enforced, as in the router,
    // but it's not possible because it requires a call to the v1 factory, which takes too much gas
    receive() external payable {}

    function migrate(address token, uint amountTokenMin, uint amountETHMin, address to, uint deadline)
        external
        override
    {
        IUniswapV1Exchange exchangeV1 = IUniswapV1Exchange(factoryV1.getExchange(token));
        uint liquidityV1 = exchangeV1.balanceOf(msg.sender);
        require(exchangeV1.transferFrom(msg.sender, address(this), liquidityV1), 'TRANSFER_FROM_FAILED');
        (uint amountETHV1, uint amountTokenV1) = exchangeV1.removeLiquidity(liquidityV1, 1, 1, uint(-1));
        TransferHelper.safeApprove(token, address(router), amountTokenV1);
        (uint amountTokenV2, uint amountETHV2,) = router.addLiquidityETH{value: amountETHV1}(
            token,
            amountTokenV1,
            amountTokenMin,
            amountETHMin,
            to,
            deadline
        );
        if (amountTokenV1 > amountTokenV2) {
            TransferHelper.safeApprove(token, address(router), 0); // be a good blockchain citizen, reset allowance to 0
            TransferHelper.safeTransfer(token, msg.sender, amountTokenV1 - amountTokenV2);
        } else if (amountETHV1 > amountETHV2) {
            // addLiquidityETH guarantees that all of amountETHV1 or amountTokenV1 will be used, hence this else is safe
            TransferHelper.safeTransferETH(msg.sender, amountETHV1 - amountETHV2);
        }
    }
}

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

# 最後に

今回の記事では、Bunzzの新機能『**DeCipher**』を使用して、**UniswapV2**の「**UniswapV2Migrator**」のコントラクトを見てきました。
いかがだったでしょうか？
今後も特定のNFTやコントラクトをピックアップしてまとめて行きたいと思います。

普段はブログやQiitaでブロックチェーンやAIに関する記事を挙げているので、よければ見ていってください！

https://chaldene.net/

https://qiita.com/cardene

