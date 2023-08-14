---
title: "[Bunzz Decipher] UniswapV2Factoryのコントラクトを理解しよう！"
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

今回はBunzzの新機能『**DeCipher**』を使用して、UniswapV2の「**UniswapV2Factory**」のコントラクトを見てみようと思います。

『**DeCipher**』はAIを使用してコントラクトのドキュメントを自動生成してくれるサービスです。

https://www.bunzz.dev/decipher

詳しい使い方に関しては以下の記事を参考にしてください！

https://zenn.dev/heku/articles/33266f0c19d523

:::message
この記事はあくまで個人が「DeCipher」を使用してみての記事になります。
間違った部分などもあるかと思いますが、その際はコメントなどで教えてください。
:::

今回使用する『**DeCipher**』のリンクは以下になります。

https://app.bunzz.dev/decipher/chains/1/addresses/0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f

# 概要

## 要約

**UniswapV2Factory**コントラクトは、**Uniswap**プロトコルの重要な構成要素であり、すべての展開済み**Pair**コントラクトのレジストリとして機能しています。
このコントラクトは、**Uniswap**プラットフォーム上での流動性ペアの作成と管理を担当しています。

:::message
**Uniswap**プラットフォーム上で使用されるトークンのペア（例えば、`ETH`と`DAI`のペアなど）の情報を管理するための役割を果たしています。
これは、トークン同士の交換や流動性の提供に関連して重要な機能です。
:::

## コントラクトの目的
**UniswapV2Factory**コントラクトの主な目的は、同一のトークンのペアの作成を防ぐことです。
これは重要なことであり、同一のペアが許可されると、流動性が複数の同一のペアに分割されて効率が悪くなり、**Uniswap**プラットフォームのスムーズな運用が乱れる可能性があります。

**UniswapV2Factory**コントラクトは、すべての展開済みペアコントラクトのレジストリを維持することで、この目的を達成しています。このレジストリは、公式なUniswapペアの記録として機能し、各ペアがユニークであり、流動性が不必要に分割されないことを確保します。

## コントラクトの責務
**UniswapV2Factory**コントラクトにはいくつかの主要な責務があります：

### ペアの作成
コントラクトは、新しいトークンのペアを作成するためのメソッドを提供します。
これは、`createPair`関数を介して行われます。
この関数は、2つのトークンアドレスを引数として受け取り、新しく作成されたペアのアドレスを返します。

### ペアの取得
コントラクトは既存のペアに関する情報を取得することができます。
これは、`getPair`関数を介して行われます。
この関数は、与えられたトークンのペアに対するペアのアドレスを返し、`allPairs`関数は、指定されたインデックスのペアのアドレスを返します。

### ペアの数のカウント
コントラクトは、作成された合計のペア数を追跡します
これは、`allPairsLength`関数を介してアクセスできます。

### 手数料の管理
コントラクトは、プロトコル手数料が送信されるアドレス（`feeTo`）と、`feeTo`アドレスを変更する権限を持つアドレス（`feeToSetter`）を管理します。
これらは、`setFeeTo`、`feeTo`、`setFeeToSetter`、および`feeToSetter`関数を介して設定および取得できます。

### イベントの記録
新しいペアが作成されるたびに、コントラクトはイベント（`PairCreated`）を発行します。
このイベントには、ペア内の2つのトークンのアドレスとペア自体のアドレスが含まれます。

まとめると、**UniswapV2Factory**コントラクトは、流動性ペアの作成と取得を管理し、各ペアのユニークさを確保する**Uniswap**プロトコルにおいて重要な役割を果たしています。
このコントラクトは、流動性の不必要な分割を防ぎ、トークンのスムーズな交換を支援するために、**Uniswap**プラットフォームの効率的な運用に不可欠です。

# 使い方

**UniswapV2Factory**コントラクトは、**Uniswap**プロトコルの中核的な部分であり、展開されたすべてのペアコントラクトのレジストリとして機能します。
このガイドでは、技術的なオペレーターや開発者の視点から **UniswapV2Factory**コントラクトを使用する手順をステップバイステップで説明します。

## コントラクトの目標
**UniswapV2Factory**コントラクトの主な目標は、**Uniswap**プラットフォーム上のすべてのトークンペアを管理し、追跡することです。
これによって同一のトークンペアの作成が防止され、**Uniswap**ペアの公式レジストリとしての役割を果たします。

### 1. UniswapV2Factoryコントラクトを展開する

最初のステップは、**UniswapV2Factory**コントラクトをEthereumネットワークに展開することです。
これはコントラクトのコンストラクタ関数を呼び出すことによって行われます。

### 2. 新しいトークンペアを作成する

新しいトークンペアを作成するには、`createPair`関数を呼び出して2つのトークンのアドレスを引数として渡します。
この関数は新しく作成されたペアのアドレスを返します。

### 3. トークンペアに関する情報を取得する

特定のトークンペアに関する情報を取得するには、`getPair`関数を使用します。
この関数は2つのトークンのアドレスを引数として受け取り、そのペアのアドレスを返します。

### 4. ペアの総数を取得する

ペアの総数を取得するには、`allPairsLength`関数を使用します。
この関数はレジストリ内のペアの総数を返します。

### 5. インデックスに基づいて特定のペアのアドレスを取得する

特定のインデックスに基づいてペアのアドレスを取得するには、`allPairs`関数を使用します。
この関数はインデックスを引数として受け取り、そのインデックスに対応するペアのアドレスを返します。

### 6. 手数料の受け取り先を設定する

取引手数料を受信するアドレスを設定するには、`setFeeTo`関数を使用します。
この関数は受け取り先のアドレスを引数として受け取ります。

### 7. 手数料設定者を設定する

手数料の設定者を変更する権限を持つアドレスを設定するには、`setFeeToSetter`関数を使用します。
この関数は新しい手数料設定者のアドレスを引数として受け取ります。

## 使用シナリオ

### 新しいトークンペアの作成

`createPair`関数を呼び出して2つのトークンのアドレスを渡すことで、新しいペアを作成し、レジストリに追加します。

### 情報のクエリ

`getPair`、`allPairsLength`、`allPairs`関数を使用して、トークンペアに関する情報を取得します。

### 手数料の管理

`setFeeTo`および`setFeeToSetter`関数を使用して、取引手数料の受け取り先と手数料設定者のアドレスを管理します。

覚えておくべきことは、各関数呼び出しにはガスが必要であり、必要なガス量は関数の複雑さによって異なります。
常にウォレットに十分なETHがあることを確認して、ガスのコストをカバーできるようにしてください。


# パラメータ

## _feeToSetter

`_feeToSetter`は、**UniswapV2Factory**コントラクトにおいて、手数料の受け取りアドレスおよび手数料設定者アドレスを設定する権限を持つアドレスです。
このアドレスはコントラクトのデプロイ時に設定され、その後変更することはできません。

具体的には、**UniswapV2Factory**コントラクト内では、取引手数料の受け取り先アドレス（`feeTo`）や手数料設定者アドレス（`feeToSetter`）を管理します。
取引手数料の受け取りアドレスは実際に取引手数料が送信されるアドレスを指し、手数料設定者アドレスはこの受け取り先アドレスを変更する権限を持つアドレスを指します。

しかし、この手数料設定者アドレスである`_feeToSetter`は、コントラクトが展開される際に1度だけ設定され、その後の変更ができません。
つまり、コントラクトが運用される間、取引手数料の受け取り先や手数料設定者を変更することはできないということです。

この制約は、セキュリティと運用の側面から重要です。
特定のアドレスに制限されることで、意図しない変更や不正な操作からコントラクトを守り、信頼性の高い運用を確保することができます。

# 関数

## READ

### allPairs

```solidity
function allPairs(uint) external view returns (address pair);
```

`allPairs`配列の特定のインデックスにあるペアコントラクトのアドレスを取得する関数。
Factoryを通じて作成されたトークンペアの中から、指定された順番（`0`から始まるインデックス）のペアのアドレスを返します。
最初に作成されたペアのアドレスを知りたい場合、`allPairs(0)`として関数を呼び出します。
2番目に作成されたペアのアドレスを知りたい場合、`allPairs(1)`として関数を呼び出します。
このように、作成順のインデックスを指定することで、対応するペアのアドレスを取得できます。
ただし、まだ作成されたペアの数が指定したインデックスに達していない場合、アドレス(0)が返されることに注意してください。

### allPairsLength

```solidity
function allPairsLength() external view returns (uint);
```

`allPairs`配列の長さを返す関数。
Factoryを通じて現在までに作成されたトークンペアの総数を返します。

現在までにFactoryが作成したトークンペアの数を知ることができます。
これによって、作成されたトークンペアの総数を確認できるため、DeFi（分散型金融）プロトコルなどで使用されるUniswapの活動の状態を把握するのに役立ちます。

### feeTo

```solidity
function feeTo() external view returns (address);
```

手数料の受け取りアドレスを返す関数。

ユーザーはこの関数を呼び出すことで、**UniswapV2Factory**コントラクトによって集められた手数料を受け取るアカウントである、手数料の受け取りアドレスを取得することができます。

### feeToSetter

```solidity
function feeToSetter() external view returns (address);
```

**UniswapV2Factory** コントラクト内の`feeToSetter`変数の値を取得する関数。
`feeToSetter`は、**UniswapV2Factory**コントラクト内で`feeTo`アドレスの設定やその他の管理機能を担当しています。
この関数は単に`feeToSetter`変数の値、つまりアドレスを返すだけです。
ユーザーはこの関数を呼び出すことで、現在の`feeToSetter`アドレスを取得し、現在のコントラクトの管理者を確認したり、`feeToSetter`アドレスが必要な他の関数とのやり取りを行ったりすることができます。

### getPair

```solidity
function getPair(address tokenA, address tokenB) external view returns (address pair);
```

引数で渡された**tokenA**と**tokenB**に対応する**Uniswap V2**ペアを表すペアコントラクトのアドレスを取得する関数。
与えられた**tokenA**と**tokenB**に対応するペアコントラクトが既に存在するかどうかを、**UniswapV2Factory**コントラクト内の`getPair`関数を呼び出すことで確認します。
もし既にペアコントラクトが存在する場合、関数はそのペアコントラクトのアドレスを返します。
一方、ペアコントラクトが存在しない場合、関数はアドレス`0x0`を返し、指定された**tokenA**と**tokenB**に対するペアコントラクトがまだ作成されていないことを示します。
ユーザーはこの関数を使用して、特定のトークンペアに対応するペアコントラクトのアドレスを取得できます。
これにより、ユーザーはペアコントラクトとやり取りして、トークンのスワップや流動性の提供などさまざまな操作を行うことができます。

## WRITE

### createPair

```solidity
function createPair(address tokenA, address tokenB) external returns (address pair) {
        require(tokenA != tokenB, 'UniswapV2: IDENTICAL_ADDRESSES');
        (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'UniswapV2: ZERO_ADDRESS');
        require(getPair[token0][token1] == address(0), 'UniswapV2: PAIR_EXISTS'); // single check is sufficient
        bytes memory bytecode = type(UniswapV2Pair).creationCode;
        bytes32 salt = keccak256(abi.encodePacked(token0, token1));
        assembly {
            pair := create2(0, add(bytecode, 32), mload(bytecode), salt)
        }
        IUniswapV2Pair(pair).initialize(token0, token1);
        getPair[token0][token1] = pair;
        getPair[token1][token0] = pair; // populate mapping in the reverse direction
        allPairs.push(pair);
        emit PairCreated(token0, token1, pair, allPairs.length);
}
```

**Uniswap V2**分散型取引所上で新しいペアを作成する関数。
2つの**ERC20**トークン、**tokenA**と**tokenB**のアドレスを受け取り、新しいペアを作成します。

**引数**
- `tokenA`
	- 新しいペアで使用される最初のERC20トークンのアドレス。
- `tokenB`
	- 新しいペアで使用される2番目のERC20トークンのアドレス。

### setFeeTo

```solidity
function setFeeTo(address _feeTo) external {
        require(msg.sender == feeToSetter, 'UniswapV2: FORBIDDEN');
        feeTo = _feeTo;
}
```

`feeTo`アドレスを設定する関数。
`_feeTo`という名前のアドレスパラメータを受け取り、`feeTo`に設定します。

この関数の呼び出し元はコントラクトデプロイ時に設定されたコントラクトの管理者かつ、`feeToSetter`に設定されたアドレスである必要があります。
もし呼び出し元が`feeToSetter`でない場合、関数はエラーメッセージ**'UniswapV2: FORBIDDEN'** と共にリバート（処理を取り消し）します。

**引数**

- `_feeTo`
	- 新しい`feeTo`アドレス。
	- コントラクトによって収集された料金の新しい受け取りアドレスとして設定されます。

### setFeeToSetter

```solidity
function setFeeToSetter(address _feeToSetter) external {
        require(msg.sender == feeToSetter, 'UniswapV2: FORBIDDEN');
        feeToSetter = _feeToSetter;
}
```

コントラクトのオーナーが`feeToSetter`アドレスを設定する関数。

コントラクトのオーナーまたは許可されたアドレスによってのみ呼び出すことができます。
取引から収集された手数料の送信先が変更できる権限が付与されます。

**引数**

- `_feeToSetter`
        - 新しい`feeToSetter`アドレス。

# イベント

## PairCreated

**Uniswap V2 Factory**コントラクト内で新しいペアが作成されるたびに発行されます。
このイベントは、以下の4つのパラメータを持ちます。

- `token0`
	- ペアの最初のトークンのアドレス。
- `token1`
	- ペアの2番目のトークンのアドレス。
- `pair`
	- 新しく作成されたペアのアドレス。

`token0`と`token1`は、トークンのアドレスを示しています。
この二つのトークンは、ソートされた順序に基づいて、常に`token0`が`token1`よりも小さな値を持つようになっています。
つまり、ペアを作成する際には、その順序が確定されているということです。

また、`log`という整数（uint）の値も関連しています。
最初に作成されたペアの場合、`log`の値は1になります。2番目に作成されたペアの場合、`log` の値は2になります。
これに続いて、順に増加していきます。`allPairs` と `getPair` という関数が関わっており、これらの関数を用いてペアの作成順に基づいて`log`の値が割り当てられていることが理解できます。

# コード

## IUniswapV2Factory

:::details IUniswapV2Factory

```solidity
pragma solidity =0.5.16;

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

## IUniswapV2Pair

:::details IUniswapV2Pair
```solidity
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

## IUniswapV2Callee

:::details IUniswapV2Callee
```solidity
interface IUniswapV2Callee {
    function uniswapV2Call(address sender, uint amount0, uint amount1, bytes calldata data) external;
}
```
:::

## UniswapV2ERC20

:::details UniswapV2ERC20
```solidity
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

## UniswapV2Pair

:::details UniswapV2Pair
```solidity
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

## UniswapV2Factory

:::details UniswapV2Factory
```solidity
contract UniswapV2Factory is IUniswapV2Factory {
    address public feeTo;
    address public feeToSetter;

    mapping(address => mapping(address => address)) public getPair;
    address[] public allPairs;

    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    constructor(address _feeToSetter) public {
        feeToSetter = _feeToSetter;
    }

    function allPairsLength() external view returns (uint) {
        return allPairs.length;
    }

    function createPair(address tokenA, address tokenB) external returns (address pair) {
        require(tokenA != tokenB, 'UniswapV2: IDENTICAL_ADDRESSES');
        (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'UniswapV2: ZERO_ADDRESS');
        require(getPair[token0][token1] == address(0), 'UniswapV2: PAIR_EXISTS'); // single check is sufficient
        bytes memory bytecode = type(UniswapV2Pair).creationCode;
        bytes32 salt = keccak256(abi.encodePacked(token0, token1));
        assembly {
            pair := create2(0, add(bytecode, 32), mload(bytecode), salt)
        }
        IUniswapV2Pair(pair).initialize(token0, token1);
        getPair[token0][token1] = pair;
        getPair[token1][token0] = pair; // populate mapping in the reverse direction
        allPairs.push(pair);
        emit PairCreated(token0, token1, pair, allPairs.length);
    }

    function setFeeTo(address _feeTo) external {
        require(msg.sender == feeToSetter, 'UniswapV2: FORBIDDEN');
        feeTo = _feeTo;
    }

    function setFeeToSetter(address _feeToSetter) external {
        require(msg.sender == feeToSetter, 'UniswapV2: FORBIDDEN');
        feeToSetter = _feeToSetter;
    }
}
```
:::

## SafeMath, Math, UQ112x112

:::details SafeMath, Math, UQ112x112
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

# 最後に

今回の記事では、Bunzzの新機能『DeCipher』を使用して、UniswapV2の「**UniswapV2Pair**」のコントラクトを見てきました。
いかがだったでしょうか？
今後も特定のNFTやコントラクトをピックアップしてまとめて行きたいと思います。

普段はブログやQiitaでブロックチェーンやAIに関する記事を挙げているので、よければ見ていってください！

https://chaldene.net/

https://qiita.com/cardene


