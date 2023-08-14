---
title: "[Bunzz Decipher] UniswapのUNIトークンのコントラクトを理解しよう！"
emoji: "🦄"
type: "tech"
topics:
  - "blockchain"
  - "ブロックチェーン"
  - "solidity"
  - "ethereum"
  - "defi"
published: true
published_at: "2023-08-14 07:04"
publication_name: "cryptogames"
---

# はじめに

初めまして。
**CryptoGames**というブロックチェーンゲーム企業でエンジニアをしている **cardene（かるでね）** です！
スマートコントラクトを書いたり、フロントエンド・バックエンド・インフラと幅広く触れています。

https://cryptogames.co.jp/

代表的なゲームは**クリプトスペルズ**というブロックチェーンゲームです。

https://cryptospells.jp/

今回はBunzzの新機能『**DeCipher**』を使用して、Uniswapの「**Uni**」トークンのコントラクトを見てみようと思います。

『**DeCipher**』はAIを使用してコントラクトのドキュメントを自動生成してくれるサービスです。

https://www.bunzz.dev/decipher

詳しい使い方に関しては以下の記事を参考にしてください！

https://zenn.dev/heku/articles/33266f0c19d523

:::message
この記事はあくまで個人が「DeCipher」を使用してみての記事になります。
間違った部分などもあるかと思いますが、その際はコメントなどで教えてください。
:::

今回使用する『**DeCipher**』のリンクは以下になります。

https://app.bunzz.dev/decipher/chains/1/addresses/0x1f9840a85d5af5bf1d1762f925bdaddc4201f984

# 概要

**Uni**コントラクトはEthereumブロックチェーン上に展開されたスマートコントラクトです。
このコントラクトは**Uniswap**トークン（**UNI**）を管理するために設計されています。
**UNI**トークンは**ERC20**トークンであり、コントラクトは**UNI**トークンの発行、配布、管理を担当しています。

## 目的
**Uni**コントラクトの主な目的は**UNI**トークンを管理することです。
これには、新しいトークンの発行、アカウント間のトークンの転送、他のユーザーに対してトークンの使用を許可する機能が含まれています。

コントラクトには特定のアカウントの残高を確認する機能、Mintアドレスを変更する機能、転送、承認、Mintアドレスの変更などの重要なアクションに関するイベントを発行する機能も含まれています。

## コントラクトの責任範囲

### トークンの管理
**Uni**コントラクトは**UNI**トークンの総供給を管理する責任があります。
総供給は初めに10億**UNI**トークンに設定されます。
コントラクトはまた、**UNI**トークンの名前、シンボル、小数点以下桁数を指定します。
それぞれの値は「**Uniswap**」、「**UNI**」、および`18`です。

### 新しいトークンの発行
コントラクトには`minter`アドレスが含まれており、新しいトークンのMintを行う唯一のアドレスです。
コントラクトはまた、各`minter`ごとに発行できる総供給の最大割合を示すMintキャップを指定します。
Mint間隔の最小時間は365日です。

### トークンの転送
コントラクトは、送信者のアカウントから宛先アカウントへトークンを転送するための機能を提供しています。
転送するトークンの数は関数で指定されます。

### 他者へのトークン使用の承認
コントラクトは、トークンの所有者が他のアカウントに一定量のトークンの使用を許可することを可能にします。
これは承認関数を介して行われ、引数として対象となるアカウントのアドレスとトークンの量が指定されます。

### アカウント残高の確認
コントラクトは特定のアカウントの残高を確認するための機能を提供します。
この関数はアカウントが保持しているトークンの数量を返します。

### Mintアドレスの変更
コントラクトでは`minter`アドレスの変更も可能です。
これは新しいM`minter`アドレスを引数として受け取る`changeMinter`関数を介して行われます。

### イベントの発行
コントラクトは転送、承認、ミンターアドレスの変更などの重要なアクションに関するイベントを発行します。
これらのイベントは外部エンティティがコントラクトの活動を追跡するために使用できます。

まとめると、**Uni**コントラクトは**UNI**トークンを管理する包括的なスマートコントラクトです。
トークンの発行、配布、管理に必要なすべての機能を提供し、**Uniswap**エコシステムの重要な要素となっています。

# 使い方

**Uni**コントラクトは、Ethereumブロックチェーン上の**Uniswap**トークン（**UNI**）に関するスマートコントラクトです。
このコントラクトは**EIP20**トークンの標準に従いつつ、新しいトークンの発行などの追加機能を持っています。
以下は、テクニカルオペレーターや開発者の視点から**Uni**コントラクトを使用する手順案内です。

## 目標
**Uni**コントラクトの目標は、トークンの転送、承認、新しいトークンの発行、Mintアドレスの変更など、**Uniswap**トークン（**UNI**）の操作を管理することです。

### コントラクトのデプロイ

コントラクトはEthereumブロックチェーン上に展開されます。
初期のミンターおよび発行が可能になるタイムスタンプは展開時に設定されます。

### コントラクトとのやり取り

コントラクトが展開されると、その関数を呼び出すことでやり取りが可能です。
以下に主な関数とその使い方を示します。

#### 2.1. トークンの転送

`transfer`関数を呼び出して、メッセージ送信者のアカウントから別のアカウントに**UNI**トークンを転送します。

#### 2.2. トークンの使用承認

`approve`関数を呼び出して、別のアカウントに対してメッセージ送信者のアカウントから一定量の**UNI**トークンの使用を許可します。

#### 2.3. ミンターアドレスの変更

`setMinter`関数を呼び出して、Mintアドレスを変更します。
この関数は現在のMinterのみが呼び出せます。

#### 2.4. 新しいトークンの発行

`mint`関数を呼び出して新しい**UNI**トークンを作成します。
この関数はMinterのみが呼び出せ、特定のタイムスタンプの後にのみ呼び出すことができます。

## 使用シナリオ

### トークンの転送と承認

**Uni**コントラクトは、**UNI**トークンをアカウント間で転送したり、メッセージ送信者の代わりに他のアカウントに**UNI**トークンの使用を承認したりするために使用できます。

### 新しいトークンの発行

**Uni**コントラクトを使用して新しい**UNI**トークンを作成することができます。
これは`mint`関数を呼び出すことで行われ、Minterのみが特定のタイムスタンプの後に呼び出すことができます。

### Mintアドレスの変更

**Uni**コントラクトはMintアドレスを変更することができます。
これは`setMinter`関数を呼び出すことで行われ、現在のMinterのみが呼び出すことができます。
本番ネットに展開する前に、常にテストネット上でコントラクトとのやり取りをテストして、取り返しのつかないミスを避けるようにしましょう。

# パラメータ

## account

**Uni**コントラクトに関連付けられるアドレス。

## minter_
**Uni**コントラクトに関連付けられる`minter`のアドレス。

## mintingAllowedAfter

**Uni**コントラクトでMintが開始される時間のタイムスタンプ（`Unix`エポック形式）。

# 変数・定数・配列

## 変数

### totalSupply

```solidity
uint public totalSupply = 1_000_000_000e18; // 1 billion Uni
```

**UNI**トークンの総発行量。
10億枚が発行上限となっている。

### minter

```solidity
address public minter;
```

トークンをMintできるアドレス。

### mintingAllowedAfter

```solidity
uint public mintingAllowedAfter;
```

トークンをMintできるようになる時間をタイムスタンプで表している。

## 定数

### minimumTimeBetweenMints

```solidity
uint32 public constant minimumTimeBetweenMints = 1 days * 365;
```

前回のMintから最低どのくらいの時間経過している必要があるかの値。
**UNI**トークンでは、1年と定義している。

### mintCap

```solidity
uint8 public constant mintCap = 2;
```

1度のMintで`totalSupply`の何%発行できるかの値。
`2`と定義されているため、`totalSupply`の2%以下と制限されている。

### DOMAIN_TYPEHASH

```solidity
bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");
```

**EIP712**ドメインのタイプハッシュを定義しています。
**EIP712**は、イーサリアム上で標準的なメッセージ署名方法を提供するための規格であり、ドメイン分離（Domain Separation）というセキュリティの概念を導入しています。

:::message
ドメイン分離（**Domain Separation**）は、情報セキュリティの概念の一つで、異なるドメインやコンテキスト間でのデータや処理を分離することを指します。
このアプローチは、異なるセキュリティレベルや信頼性の要件を持つシステムやプロセスを保護し、データ漏洩や攻撃を防ぐために使用されます。

ブロックチェーンや暗号通貨のコンテキストでは、ドメイン分離は特に重要です。
例えば、**EIP712**のような署名メッセージのセキュリティを確保するために使われます。
ドメイン分離を実現するためには、異なるコンテキストでの処理やデータの混同を避けるために、特定の識別子（チェーンIDやコントラクトアドレスなど）を用いてデータや処理を区別する必要があります。

具体的には、ドメイン分離を導入することで、異なるチェーンやアプリケーション間でのデータの流用や、攻撃者がデータや署名を再利用してセキュリティを乱すことを防ぐことができます。**EIP712**のような署名機能において、ドメイン分離は署名のセキュリティを高め、コンテキストごとに異なるドメインハッシュを利用することで、データの混同や攻撃を防ぐ役割を果たします。
:::

- `name`
	- トークンの名前を表す文字列。
- `chainId`
	- チェーンの識別子を表す整数値（イーサリアムメインネットワークなどのネットワークごとに異なる値）。
- `verifyingContract`
	- 署名を検証するためのスマートコントラクトのアドレス。

### DELEGATION_TYPEHASH

```solidity
bytes32 public constant DELEGATION_TYPEHASH = keccak256("Delegation(address delegatee,uint256 nonce,uint256 expiry)");
```

**EIP712**メッセージの一部として、スマートコントラクト内でのデリゲーション（投票権委任）に関する情報をハッシュ化するために使用される。

- `delegatee`
	- 委任先のアドレス。
- `nonce`
	- 委任トランザクションのユニークな番号。
- `expiry`
	- 委任の有効期限。

### PERMIT_TYPEHASH

```solidity
bytes32 public constant PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
```

**EIP712**メッセージの一部として、トークン所有者が特定のアカウントに対してトークンの使用許可を与えるための情報をハッシュ化するために使用される。

- `owner`
	- トークンの所有者のアドレス。
- `spender`
	- 許可されるアドレス。
- `value`
	- 許可されるトークンの量。
- `nonce`
	- ユニークな番号。
- `deadline`
	- 署名の有効期限。

## 配列

### delegates

```solidity
mapping (address => address) public delegates;
```

アカウントごとにデリゲート（代理）を保持するためのマッピング配列。

各アカウントのアドレスをキーとし、そのアカウントがデリゲートしているアドレスを値として関連付けています。
つまり、各アカウントは別のアドレスに対して操作権限を委任（デリゲート）することができます。

:::message
ここで言う「**デリゲート**」とは、一つのアカウントが別のアカウントに対して、特定の操作（例えばトークンの移動や投票など）を行う権限を与えることを指します。
デリゲート関係は、トークンの所有権やスマートコントラクト内での操作権限を効果的に管理するための仕組みです。
アカウント所有者が一時的に別のアカウントに操作権限を委任することで、セキュリティや効率性を向上させることができます。
:::


### checkpoints

```solidity
struct Checkpoint {
uint32 fromBlock;
uint96 votes;
}

mapping (address => mapping (uint32 => Checkpoint)) public checkpoints;
```

アカウントごとに複数の投票数の履歴を保持するためのマッピング配列。

`Checkpoint`構造体では、特定のアカウントに対してブロックごとの投票数を記録するためのデータ構造を定義しています。

- `fromBlock`
	- 投票数がどのブロックから始まったかを示すブロック番号（uint32型）。
- `votes`
	- ブロックからの投票数を示す数値（uint96型）。

外側のマッピングのキーはアカウントのアドレスで、内側のマッピングのキーは特定のブロック番号（uint32型）です。
マッピングの値は、`Checkpoint`構造体を格納しており、各ブロックごとの投票数の履歴がそのアカウントに対して記録されています。
特定のアカウントがあるブロックから別のブロックにかけて行った投票の履歴を管理するために使用されます。
各アカウントの投票履歴がブロックごとに記録され、必要な場合に過去の投票数を確認することができます。

### numCheckpoints

```solidity
mapping (address => uint32) public numCheckpoints;
```

各アドレスごとにそのアカウントに関連する投票数の履歴（チェックポイント）の数を保持するためのマッピング配列。

各アカウントが保持する投票数の履歴の数を管理するために使用されます。
特定のアカウントの投票数の変更が行われるたびに、新しいチェックポイントが作成され、そのアカウントの`numCheckpoints`マッピングの値が更新されます。
これにより、特定のアカウントの投票数の履歴数を効率的に追跡することができます。

### nonces

```solidity
mapping (address => uint) public nonces;
```

各アドレスに対して署名に関連する状態情報を保持するためのマッピング配列。
特定のアカウントが署名を行う際に、一意の状態情報（`nonce`）を生成し、その後の署名操作において重複を避けるために使用されます。
具体的には、アカウントが署名を生成するたびに、関連する`nonces`マッピングの値がインクリメントされます。
これにより、アカウントごとに一意の状態情報が提供され、署名のセキュリティと整合性が確保されます。

:::message
署名操作において、一度使用された状態情報は次回以降の署名操作では使用されないため、セキュリティを向上させるのに役立ちます。
:::


# 関数

## constructor

```solidity
require(mintingAllowedAfter_ >= block.timestamp, "Uni::constructor: minting can only begin after deployment");
```
指定されたmintingAllowedAfter_のタイムスタンプがデプロイのタイムスタンプ以降であることを確認しています。

トークンの発行がデプロイ後にのみ許可されるように制限しています。
もし`mintingAllowedAfter_`がデプロイ時刻より前であれば、エラーメッセージが表示されてデプロイが中止されます。

```solidity
balances[account] = uint96(totalSupply);
```
コントラクト内のアカウントの残高を初期化しています。

指定されたアカウントの残高をトータル供給量（`totalSupply`）に設定しています。

```solidity
emit Transfer(address(0), account, totalSupply);
```
トランザクションがトークンの移動（転送）を示すTransferイベントを発行しています。

トークンがアドレス`0`から指定されたアカウントに送信されたことを示しています。

```solidity
minter = minter_;
```
トークンの発行者のアドレスを設定しています。

```solidity
emit MinterChanged(address(0), minter);
```
`minter`が変更されたことを示す`MinterChanged`イベントを発行しています。
`minter`がアドレス`0`から指定された`minter`のアドレスに変更されたことを示しています。

```solidity
mintingAllowedAfter = mintingAllowedAfter_;
```

トークンの発行が許可されるタイムスタンプを設定しています。

## READ

### getCurrentVotes

```solidity
function getCurrentVotes(address account) external view returns (uint96) {
        uint32 nCheckpoints = numCheckpoints[account];
        return nCheckpoints > 0 ? checkpoints[account][nCheckpoints - 1].votes : 0;
}
```

指定されたアドレスの現在の投票バランスを取得するための関数。

`nCheckpoints`は、指定されたアカウントに関連するチェックポイントの数を格納します。
アカウントの投票履歴の数を確認できます。
投票履歴が存在する場合は、最新のチェックポイントから得られる投票数 (`votes`) を返します。
存在しない場合は、`0`を返します。
これにより、指定されたアカウントの現在の投票バランスを取得することができます。

### getPriorVotes

```solidity
function getPriorVotes(address account, uint blockNumber) public view returns (uint96) {
        require(blockNumber < block.number, "Uni::getPriorVotes: not yet determined");

        uint32 nCheckpoints = numCheckpoints[account];
        if (nCheckpoints == 0) {
            return 0;
        }

        // First check most recent balance
        if (checkpoints[account][nCheckpoints - 1].fromBlock <= blockNumber) {
            return checkpoints[account][nCheckpoints - 1].votes;
        }

        // Next check implicit zero balance
        if (checkpoints[account][0].fromBlock > blockNumber) {
            return 0;
        }

        uint32 lower = 0;
        uint32 upper = nCheckpoints - 1;
        while (upper > lower) {
            uint32 center = upper - (upper - lower) / 2; // ceil, avoiding overflow
            Checkpoint memory cp = checkpoints[account][center];
            if (cp.fromBlock == blockNumber) {
                return cp.votes;
            } else if (cp.fromBlock < blockNumber) {
                lower = center;
            } else {
                upper = center - 1;
            }
        }
        return checkpoints[account][lower].votes;
}
```

特定のアドレスの特定のブロック番号(`blockNumber`)時点での事前の投票数を決定するための関数。

1. 指定されたブロック番号が現在のブロック番号よりも過去のものであることを確認しています。
未来のブロック番号に対して投票数を計算することを防ぐためです。
2. `nCheckpoints`は、指定されたアカウントに関連するチェックポイントの数を格納します。
これにより、アカウントの投票履歴の数を確認できます。
3. アカウントの投票履歴が存在しない場合、事前の投票数を`0`として返します。
4. 最新のチェックポイントが指定されたブロック番号よりも過去である場合、そのチェックポイントの投票数を返します。
指定されたブロック番号以前の最新の投票数が得られます。
5. 最も古いチェックポイントが指定されたブロック番号よりも未来の場合、事前の投票数を`0`として返します。
指定されたブロック番号よりも古い投票履歴が存在しないことを確認できます。
6. `lower`と`upper`は、二分探索アルゴリズムに使用される変数です。
初期値として、アカウントの投票履歴内での探索範囲が指定されています。
7. ループで二分探索アルゴリズムを使用して指定されたブロック番号に最も近いチェックポイントを見つけます。
8. `center`で示される位置のチェックポイントを一時的な変数`cp`に格納します。
9. チェックポイントのブロック番号が指定されたブロック番号と一致する場合、そのチェックポイントの投票数を返します。
10. 探索範囲を更新して次のステップに進むための条件分岐を行います。
もしチェックポイントのブロック番号が指定されたブロック番号よりも小さい場合は、探索範囲の下限を`center`に設定します。
逆に、大きい場合は探索範囲の上限を更新します。
11. 最終的に、最も近いチェックポイントを見つけた後、そのチェックポイントの投票数を返します。

### safe32

```solidity
function safe32(uint n, string memory errorMessage) internal pure returns (uint32) {
        require(n < 2**32, errorMessage);
        return uint32(n);
}
```

与えられた整数`n`が`32`ビット未満であることを確認し、条件に合致する場合は`n`を`uint32`型として返します。
もし`n`が`32`ビットを超える場合、エラーメッセージとともにエラーを発生させます。

### safe96

```solidity
function safe96(uint n, string memory errorMessage) internal pure returns (uint96) {
        require(n < 2**96, errorMessage);
        return uint96(n);
}
```

与えられた整数`n`が`96`ビット未満であることを確認し、条件に合致する場合は`n`を`uint96`型として返します。
もし`n`が`96`ビットを超える場合、エラーメッセージとともにエラーを発生させます。

### add96
```solidity
function add96(uint96 a, uint96 b, string memory errorMessage) internal pure returns (uint96) {
        uint96 c = a + b;
        require(c >= a, errorMessage);
        return c;
}
```

与えられた2つの`96`ビット整数`a`と`b`を加算し、結果を`uint96`型として返します。
加算時にオーバーフローが発生するかどうかを確認し、条件に合致しない場合はエラーメッセージとともにエラーを発生させます。


### sub96
```solidity
function sub96(uint96 a, uint96 b, string memory errorMessage) internal pure returns (uint96) {
        require(b <= a, errorMessage);
        return a - b;
}
```

与えられた2つの`96`ビット整数`a`から`b`を減算し、結果を`uint96`型として返します。
減算時にアンダーフローが発生するかどうかを確認し、条件に合致しない場合はエラーメッセージとともにエラーを発生させます。

### getChainId

```solidity
function getChainId() internal pure returns (uint) {
        uint256 chainId;
        assembly { chainId := chainid() }
        return chainId;
}
```

EthereumネットワークのチェーンIDを取得するために使用されます。
`assembly`ブロックを使用して`chainid()`関数を呼び出し、取得したチェーンIDを`uint`型で返します。

## WRITE

### setMinter

```solidity
function setMinter(address minter_) external {
        require(msg.sender == minter, "Uni::setMinter: only the minter can change the minter address");
        emit MinterChanged(minter, minter_);
        minter = minter_;
}
```
新しい`minter`アドレスを設定する関数。
トランザクションを発行したアドレスが現在の`minter`アドレスと一致するかどうかを確認しています。
変更操作は、現在の`minter`（トークンの発行者）によってのみ実行できます。
もし条件を満たさない場合、エラーメッセージが表示されて関数の実行が中止されます。

### mint

```solidity
function mint(address dst, uint rawAmount) external {
        require(msg.sender == minter, "Uni::mint: only the minter can mint");
        require(block.timestamp >= mintingAllowedAfter, "Uni::mint: minting not allowed yet");
        require(dst != address(0), "Uni::mint: cannot transfer to the zero address");

        // record the mint
        mintingAllowedAfter = SafeMath.add(block.timestamp, minimumTimeBetweenMints);

        // mint the amount
        uint96 amount = safe96(rawAmount, "Uni::mint: amount exceeds 96 bits");
        require(amount <= SafeMath.div(SafeMath.mul(totalSupply, mintCap), 100), "Uni::mint: exceeded mint cap");
        totalSupply = safe96(SafeMath.add(totalSupply, amount), "Uni::mint: totalSupply exceeds 96 bits");

        // transfer the amount to the recipient
        balances[dst] = add96(balances[dst], amount, "Uni::mint: transfer amount overflows");
        emit Transfer(address(0), dst, amount);

        // move delegates
        _moveDelegates(address(0), delegates[dst], amount);
}
```

新しいトークンを発行する関数。

1. トランザクションを発行したアドレスが現在の`minter`アドレスと一致するかどうかを確認しています。
トークンの発行は、`minter`アドレスによってのみ許可されています。

2. トークンの発行が許可されたタイムスタンプ以降の時点であるかを確認しています。
指定されたタイムスタンプ以前にトークンを発行することはできません。

3. トークンを送信する宛先アドレスがゼロアドレスでないことを確認しています。
ゼロアドレスへの送信は無効です。

4. トークンの発行後、次にトークンを発行できるタイムスタンプを更新しています。
これにより、トークンの発行間隔が制御されます。

5. 発行するトークンの量を計算して`amount`という変数に格納しています。

6. 発行するトークンの量が現在の総供給量の一定割合を超えないことを確認しています。
トークンの発行量に制限を設けることで、通貨のインフレーションをコントロールしています。

7. 総供給量を新しいトークンの発行に合わせて更新しています。

8. 宛先アドレスの残高を増やして、新しいトークンの発行を反映させています。

9. トークンの送信が行われたことを示す`Transfer`イベントを発行しています。

10. トークンの発行に伴い、デリゲート（投票権の委任先）を移動するための内部関数を呼び出しています。

### permit

```solidity
function permit(address owner, address spender, uint rawAmount, uint deadline, uint8 v, bytes32 r, bytes32 s) external {
        uint96 amount;
        if (rawAmount == uint(-1)) {
            amount = uint96(-1);
        } else {
            amount = safe96(rawAmount, "Uni::permit: amount exceeds 96 bits");
        }

        bytes32 domainSeparator = keccak256(abi.encode(DOMAIN_TYPEHASH, keccak256(bytes(name)), getChainId(), address(this)));
        bytes32 structHash = keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, rawAmount, nonces[owner]++, deadline));
        bytes32 digest = keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
        address signatory = ecrecover(digest, v, r, s);
        require(signatory != address(0), "Uni::permit: invalid signature");
        require(signatory == owner, "Uni::permit: unauthorized");
        require(now <= deadline, "Uni::permit: signature expired");

        allowances[owner][spender] = amount;

        emit Approval(owner, spender, amount);
}
```

特定のトークン所有者が別のアドレスに対して支出を承認するための関数。

1. 承認するトークンの量を計算しています。
`rawAmount`が`-1`であれば、無限に承認されたことを示すために`amount`を`-1`に設定します。
それ以外の場合、96ビットの範囲内に収めるために`safe96`関数を使用して`rawAmount`を変換します。

2. ドメイン分離のためのハッシュ値を計算しています。
このハッシュは、**EIP712**ドメインのパラメーターを組み合わせたものです。

3. 承認構造体のハッシュ値を計算しています。
このハッシュは、承認の各パラメーターを組み合わせたものです。
所有者のノンス（カウンター）もインクリメントしています。

4. 署名の対象となるダイジェストを計算しています。
**EIP191**署名のプレフィックス（`\x19\x01`）を含む、ドメイン分離と承認構造体のハッシュ値を組み合わせたものです。

5. 署名からアドレスを回復します。
誰がトークンの承認を行っているかを確認できます。

6. 回復されたアドレスがゼロアドレスでないことを確認しています。

7. 承認を行っているアドレスがトークン所有者のアドレスと一致することを確認しています。

8. 承認の署名が有効期限内であることを確認しています。

9. 承認リスト（トークン所有者が特定のアドレスに対してどれだけのトークンを承認しているか）を更新しています。

10. 承認が行われたことを示すApprovalイベントを発行しています。

### delegate

```solidity
function delegate(address delegatee) public {
        return _delegate(msg.sender, delegatee);
}
```

トークン所有者が投票権を他のアドレスに委任するための関数。
`_delegate`関数に処理を委譲しています。

### delegateBySig

```solidity
function delegateBySig(address delegatee, uint nonce, uint expiry, uint8 v, bytes32 r, bytes32 s) public {
        bytes32 domainSeparator = keccak256(abi.encode(DOMAIN_TYPEHASH, keccak256(bytes(name)), getChainId(), address(this)));
        bytes32 structHash = keccak256(abi.encode(DELEGATION_TYPEHASH, delegatee, nonce, expiry));
        bytes32 digest = keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
        address signatory = ecrecover(digest, v, r, s);
        require(signatory != address(0), "Uni::delegateBySig: invalid signature");
        require(nonce == nonces[signatory]++, "Uni::delegateBySig: invalid nonce");
        require(now <= expiry, "Uni::delegateBySig: signature expired");
        return _delegate(signatory, delegatee);
}
```

署名を使用してトークン所有者が投票権を他のアドレスに委任するための関数。
署名に関連する情報（`nonce`、`expiry`、`v`、`r`、`s`）が引数として渡されます。

1. `domainSeparator`は、署名の検証に使用されるドメインセパレーターを計算します。
コントラクトのドメイン情報をハッシュ化して生成されます。
2. `structHash`は、委任構造体のハッシュを計算します。
委任先のアドレス、`nonce`、有効期限を含む情報をハッシュ化して生成されます。
3. `digest`は、署名メッセージ全体のハッシュを計算します。
これは、署名メッセージのフォーマットと他のハッシュ情報を組み合わせて生成されます。
4. `ecrecover`関数を使用して、署名から復元したアドレスを計算します。
5. 署名が有効であることを確認します。
6. トランザクションの`nonce`が正しいことを確認します。
7. 現在の時間が有効期限を超えていないことを確認します。
8. `_delegate`関数に処理を委譲して、実際の委任処理を行います。

### _delegate

```solidity
function _delegate(address delegator, address delegatee) internal {
        address currentDelegate = delegates[delegator];
        uint96 delegatorBalance = balances[delegator];
        delegates[delegator] = delegatee;

        emit DelegateChanged(delegator, currentDelegate, delegatee);

        _moveDelegates(currentDelegate, delegatee, delegatorBalance);
}
```

投票の委任を行うための重要なステップを処理する関数。
`delegator`が委任する側のアドレスであり、`delegatee`が投票を委任される側のアドレスです。

1. `currentDelegate`は、現在の委任先アドレスを格納する変数です。
`delegator`アドレスの委任先を取得します。
2. `delegatorBalance`は、委任元アカウントの残高を格納する変数です。
これにより、委任元の残高が取得されます。
3. `delegator`アドレスの委任先を `delegatee`アドレスに設定します。
`delegator`アカウントの投票権を `delegatee`アカウントに委任する操作です。
4. `DelegateChanged`イベントを発行します。
このイベントは、アカウント間の投票権の委任が変更されたときに通知を提供するために使用されます。
`delegator`アドレス、以前の委任先アドレス (`currentDelegate`)、新しい委任先アドレス (`delegatee`) がイベントに含まれます。
5. `_moveDelegates`関数を呼び出して、委任先の変更に伴う投票数の移動を処理します。
以前の委任先 (`currentDelegate`)、新しい委任先 (`delegatee`)、および委任元アカウントの残高 (`delegatorBalance`) を引数として渡します。

### _moveDelegates

```solidity
function _moveDelegates(address srcRep, address dstRep, uint96 amount) internal {
        if (srcRep != dstRep && amount > 0) {
            if (srcRep != address(0)) {
                uint32 srcRepNum = numCheckpoints[srcRep];
                uint96 srcRepOld = srcRepNum > 0 ? checkpoints[srcRep][srcRepNum - 1].votes : 0;
                uint96 srcRepNew = sub96(srcRepOld, amount, "Uni::_moveVotes: vote amount underflows");
                _writeCheckpoint(srcRep, srcRepNum, srcRepOld, srcRepNew);
            }

            if (dstRep != address(0)) {
                uint32 dstRepNum = numCheckpoints[dstRep];
                uint96 dstRepOld = dstRepNum > 0 ? checkpoints[dstRep][dstRepNum - 1].votes : 0;
                uint96 dstRepNew = add96(dstRepOld, amount, "Uni::_moveVotes: vote amount overflows");
                _writeCheckpoint(dstRep, dstRepNum, dstRepOld, dstRepNew);
            }
        }
}
```

投票権の移動を処理し、新しい委任先アカウントに投票数を追加し、元の委任先アカウントから投票数を減少させる関数。
`srcRep`は元の委任先アドレス、`dstRep`は新しい委任先アドレス、`amount`は移動する投票数を示します。

1. 元の委任先アドレスと新しい委任先アドレスが異なる場合かつ移動する投票数が`0`より大きい場合にのみ、次の処理に移ります。
2. 元の委任先アドレスがゼロアドレスでない場合に、元の委任先アドレスの投票数を処理します。
3. `srcRepNum`は、元の委任先アドレスのチェックポイントの数を示す変数です。
4. `srcRepOld`は、元の委任先アドレスの最新のチェックポイントでの投票数を示します。
チェックポイントが存在しない場合、デフォルトで`0`が使用されます。
5. `srcRepNew`は、元の委任先アドレスの新しいチェックポイントでの投票数を示します。
`amount`を減算することで、投票数が更新されます。
減算の際にアンダーフローが発生する場合はエラーとして処理されます。
6. `_writeCheckpoint`関数を呼び出して、元の委任先アドレスのチェックポイントを更新します。
これにより、元の委任先の投票数の変更が記録されます。
7. 3~6の手順で、新しい委任先アドレスに対しても、投票数の増加を処理するセクションがあります。

### _writeCheckpoint

```solidity
function _writeCheckpoint(address delegatee, uint32 nCheckpoints, uint96 oldVotes, uint96 newVotes) internal {
      uint32 blockNumber = safe32(block.number, "Uni::_writeCheckpoint: block number exceeds 32 bits");

      if (nCheckpoints > 0 && checkpoints[delegatee][nCheckpoints - 1].fromBlock == blockNumber) {
          checkpoints[delegatee][nCheckpoints - 1].votes = newVotes;
      } else {
          checkpoints[delegatee][nCheckpoints] = Checkpoint(blockNumber, newVotes);
          numCheckpoints[delegatee] = nCheckpoints + 1;
      }

      emit DelegateVotesChanged(delegatee, oldVotes, newVotes);
}
```

委任先アドレスの投票数の変更をチェックポイントとして記録する関数。
`delegatee`は委任先アドレス、`nCheckpoints`は委任先アドレスのチェックポイント数、`oldVotes`は変更前の投票数、`newVotes`は変更後の投票数です。

1. `blockNumber`は、現在のブロック番号を表す変数です。
32ビットを超える場合にエラーを回避するために `safe32`関数を使用してブロック番号を制約しています。
2. すでに委任先アドレスの最新のチェックポイントが現在のブロック番号と一致している場合をチェックしています。
もし一致している場合、最新のチェックポイントの投票数を`newVotes`に更新します。
3. 最新のチェックポイントの投票数を`newVotes`に更新します。
4. 条件に当てはまらない場合、新しいチェックポイントを作成し、新しい投票数とブロック番号を記録します。
5. 新しいチェックポイントを`checkpoints`マッピングに追加します。
この新しいチェックポイントは、新しい投票数と現在のブロック番号から構成されます。
6. `numCheckpoints`マッピング内の委任先アドレスのチェックポイント数を更新します。
7. 投票数が変更されたことを示す DelegateVotesChanged イベントを発行します。

# Event

## MinterChanged

```solidity
event MinterChanged(address minter, address newMinter);
```

Mint権限のアドレスが変更されたときに発行されるイベント。

- `minter`
	- 変更前のMint権限のアドレス。
- `newMinter`
	- 変更後のMint権限のアドレス。

## DelegateChanged

```solidity
event DelegateChanged(address indexed delegator, address indexed fromDelegate, address indexed toDelegate);
```

アカウントがそのデリゲートを変更したときに発行されるイベント。

- `delegator`
	- デリゲートを変更したアカウントのアドレス。
- `fromDelegate`
	- 変更前のデリゲートのアドレス。
- `toDelegate`
	- 新しいデリゲートのアドレス。

## DelegateVotesChanged

```solidity
event DelegateVotesChanged(address indexed delegate, uint previousBalance, uint newBalance);
```

デリゲートアカウントの投票バランスが変更されたとき発行されるイベント。

- `delegate`
	- 投票バランスが変更されたデリゲートのアドレス。
- `previousBalance`
	- 変更前の投票バランス。
- `newBalance`
	- 新しい投票バランス。

# コード

## SafeMath

:::details SafeMath
```solidity
/**
 *Submitted for verification at Etherscan.io on 2020-09-15
*/

pragma solidity ^0.5.16;
pragma experimental ABIEncoderV2;

// From https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/math/Math.sol
// Subject to the MIT license.

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting with custom message on overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, errorMessage);

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on underflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot underflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction underflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on underflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot underflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, errorMessage);

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers.
     * Reverts on division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers.
     * Reverts with custom message on division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}
```
:::

## Uni

:::details Uni
```solidity
/**
 *Submitted for verification at Etherscan.io on 2020-09-15
*/

pragma solidity ^0.5.16;
pragma experimental ABIEncoderV2;

// From https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/math/Math.sol
// Subject to the MIT license.

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting with custom message on overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, errorMessage);

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on underflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot underflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction underflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on underflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot underflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, errorMessage);

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers.
     * Reverts on division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers.
     * Reverts with custom message on division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract Uni {
    /// @notice EIP-20 token name for this token
    string public constant name = "Uniswap";

    /// @notice EIP-20 token symbol for this token
    string public constant symbol = "UNI";

    /// @notice EIP-20 token decimals for this token
    uint8 public constant decimals = 18;

    /// @notice Total number of tokens in circulation
    uint public totalSupply = 1_000_000_000e18; // 1 billion Uni

    /// @notice Address which may mint new tokens
    address public minter;

    /// @notice The timestamp after which minting may occur
    uint public mintingAllowedAfter;

    /// @notice Minimum time between mints
    uint32 public constant minimumTimeBetweenMints = 1 days * 365;

    /// @notice Cap on the percentage of totalSupply that can be minted at each mint
    uint8 public constant mintCap = 2;

    /// @notice Allowance amounts on behalf of others
    mapping (address => mapping (address => uint96)) internal allowances;

    /// @notice Official record of token balances for each account
    mapping (address => uint96) internal balances;

    /// @notice A record of each accounts delegate
    mapping (address => address) public delegates;

    /// @notice A checkpoint for marking number of votes from a given block
    struct Checkpoint {
        uint32 fromBlock;
        uint96 votes;
    }

    /// @notice A record of votes checkpoints for each account, by index
    mapping (address => mapping (uint32 => Checkpoint)) public checkpoints;

    /// @notice The number of checkpoints for each account
    mapping (address => uint32) public numCheckpoints;

    /// @notice The EIP-712 typehash for the contract's domain
    bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");

    /// @notice The EIP-712 typehash for the delegation struct used by the contract
    bytes32 public constant DELEGATION_TYPEHASH = keccak256("Delegation(address delegatee,uint256 nonce,uint256 expiry)");

    /// @notice The EIP-712 typehash for the permit struct used by the contract
    bytes32 public constant PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");

    /// @notice A record of states for signing / validating signatures
    mapping (address => uint) public nonces;

    /// @notice An event thats emitted when the minter address is changed
    event MinterChanged(address minter, address newMinter);

    /// @notice An event thats emitted when an account changes its delegate
    event DelegateChanged(address indexed delegator, address indexed fromDelegate, address indexed toDelegate);

    /// @notice An event thats emitted when a delegate account's vote balance changes
    event DelegateVotesChanged(address indexed delegate, uint previousBalance, uint newBalance);

    /// @notice The standard EIP-20 transfer event
    event Transfer(address indexed from, address indexed to, uint256 amount);

    /// @notice The standard EIP-20 approval event
    event Approval(address indexed owner, address indexed spender, uint256 amount);

    /**
     * @notice Construct a new Uni token
     * @param account The initial account to grant all the tokens
     * @param minter_ The account with minting ability
     * @param mintingAllowedAfter_ The timestamp after which minting may occur
     */
    constructor(address account, address minter_, uint mintingAllowedAfter_) public {
        require(mintingAllowedAfter_ >= block.timestamp, "Uni::constructor: minting can only begin after deployment");

        balances[account] = uint96(totalSupply);
        emit Transfer(address(0), account, totalSupply);
        minter = minter_;
        emit MinterChanged(address(0), minter);
        mintingAllowedAfter = mintingAllowedAfter_;
    }

    /**
     * @notice Change the minter address
     * @param minter_ The address of the new minter
     */
    function setMinter(address minter_) external {
        require(msg.sender == minter, "Uni::setMinter: only the minter can change the minter address");
        emit MinterChanged(minter, minter_);
        minter = minter_;
    }

    /**
     * @notice Mint new tokens
     * @param dst The address of the destination account
     * @param rawAmount The number of tokens to be minted
     */
    function mint(address dst, uint rawAmount) external {
        require(msg.sender == minter, "Uni::mint: only the minter can mint");
        require(block.timestamp >= mintingAllowedAfter, "Uni::mint: minting not allowed yet");
        require(dst != address(0), "Uni::mint: cannot transfer to the zero address");

        // record the mint
        mintingAllowedAfter = SafeMath.add(block.timestamp, minimumTimeBetweenMints);

        // mint the amount
        uint96 amount = safe96(rawAmount, "Uni::mint: amount exceeds 96 bits");
        require(amount <= SafeMath.div(SafeMath.mul(totalSupply, mintCap), 100), "Uni::mint: exceeded mint cap");
        totalSupply = safe96(SafeMath.add(totalSupply, amount), "Uni::mint: totalSupply exceeds 96 bits");

        // transfer the amount to the recipient
        balances[dst] = add96(balances[dst], amount, "Uni::mint: transfer amount overflows");
        emit Transfer(address(0), dst, amount);

        // move delegates
        _moveDelegates(address(0), delegates[dst], amount);
    }

    /**
     * @notice Get the number of tokens `spender` is approved to spend on behalf of `account`
     * @param account The address of the account holding the funds
     * @param spender The address of the account spending the funds
     * @return The number of tokens approved
     */
    function allowance(address account, address spender) external view returns (uint) {
        return allowances[account][spender];
    }

    /**
     * @notice Approve `spender` to transfer up to `amount` from `src`
     * @dev This will overwrite the approval amount for `spender`
     *  and is subject to issues noted [here](https://eips.ethereum.org/EIPS/eip-20#approve)
     * @param spender The address of the account which may transfer tokens
     * @param rawAmount The number of tokens that are approved (2^256-1 means infinite)
     * @return Whether or not the approval succeeded
     */
    function approve(address spender, uint rawAmount) external returns (bool) {
        uint96 amount;
        if (rawAmount == uint(-1)) {
            amount = uint96(-1);
        } else {
            amount = safe96(rawAmount, "Uni::approve: amount exceeds 96 bits");
        }

        allowances[msg.sender][spender] = amount;

        emit Approval(msg.sender, spender, amount);
        return true;
    }

    /**
     * @notice Triggers an approval from owner to spends
     * @param owner The address to approve from
     * @param spender The address to be approved
     * @param rawAmount The number of tokens that are approved (2^256-1 means infinite)
     * @param deadline The time at which to expire the signature
     * @param v The recovery byte of the signature
     * @param r Half of the ECDSA signature pair
     * @param s Half of the ECDSA signature pair
     */
    function permit(address owner, address spender, uint rawAmount, uint deadline, uint8 v, bytes32 r, bytes32 s) external {
        uint96 amount;
        if (rawAmount == uint(-1)) {
            amount = uint96(-1);
        } else {
            amount = safe96(rawAmount, "Uni::permit: amount exceeds 96 bits");
        }

        bytes32 domainSeparator = keccak256(abi.encode(DOMAIN_TYPEHASH, keccak256(bytes(name)), getChainId(), address(this)));
        bytes32 structHash = keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, rawAmount, nonces[owner]++, deadline));
        bytes32 digest = keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
        address signatory = ecrecover(digest, v, r, s);
        require(signatory != address(0), "Uni::permit: invalid signature");
        require(signatory == owner, "Uni::permit: unauthorized");
        require(now <= deadline, "Uni::permit: signature expired");

        allowances[owner][spender] = amount;

        emit Approval(owner, spender, amount);
    }

    /**
     * @notice Get the number of tokens held by the `account`
     * @param account The address of the account to get the balance of
     * @return The number of tokens held
     */
    function balanceOf(address account) external view returns (uint) {
        return balances[account];
    }

    /**
     * @notice Transfer `amount` tokens from `msg.sender` to `dst`
     * @param dst The address of the destination account
     * @param rawAmount The number of tokens to transfer
     * @return Whether or not the transfer succeeded
     */
    function transfer(address dst, uint rawAmount) external returns (bool) {
        uint96 amount = safe96(rawAmount, "Uni::transfer: amount exceeds 96 bits");
        _transferTokens(msg.sender, dst, amount);
        return true;
    }

    /**
     * @notice Transfer `amount` tokens from `src` to `dst`
     * @param src The address of the source account
     * @param dst The address of the destination account
     * @param rawAmount The number of tokens to transfer
     * @return Whether or not the transfer succeeded
     */
    function transferFrom(address src, address dst, uint rawAmount) external returns (bool) {
        address spender = msg.sender;
        uint96 spenderAllowance = allowances[src][spender];
        uint96 amount = safe96(rawAmount, "Uni::approve: amount exceeds 96 bits");

        if (spender != src && spenderAllowance != uint96(-1)) {
            uint96 newAllowance = sub96(spenderAllowance, amount, "Uni::transferFrom: transfer amount exceeds spender allowance");
            allowances[src][spender] = newAllowance;

            emit Approval(src, spender, newAllowance);
        }

        _transferTokens(src, dst, amount);
        return true;
    }

    /**
     * @notice Delegate votes from `msg.sender` to `delegatee`
     * @param delegatee The address to delegate votes to
     */
    function delegate(address delegatee) public {
        return _delegate(msg.sender, delegatee);
    }

    /**
     * @notice Delegates votes from signatory to `delegatee`
     * @param delegatee The address to delegate votes to
     * @param nonce The contract state required to match the signature
     * @param expiry The time at which to expire the signature
     * @param v The recovery byte of the signature
     * @param r Half of the ECDSA signature pair
     * @param s Half of the ECDSA signature pair
     */
    function delegateBySig(address delegatee, uint nonce, uint expiry, uint8 v, bytes32 r, bytes32 s) public {
        bytes32 domainSeparator = keccak256(abi.encode(DOMAIN_TYPEHASH, keccak256(bytes(name)), getChainId(), address(this)));
        bytes32 structHash = keccak256(abi.encode(DELEGATION_TYPEHASH, delegatee, nonce, expiry));
        bytes32 digest = keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
        address signatory = ecrecover(digest, v, r, s);
        require(signatory != address(0), "Uni::delegateBySig: invalid signature");
        require(nonce == nonces[signatory]++, "Uni::delegateBySig: invalid nonce");
        require(now <= expiry, "Uni::delegateBySig: signature expired");
        return _delegate(signatory, delegatee);
    }

    /**
     * @notice Gets the current votes balance for `account`
     * @param account The address to get votes balance
     * @return The number of current votes for `account`
     */
    function getCurrentVotes(address account) external view returns (uint96) {
        uint32 nCheckpoints = numCheckpoints[account];
        return nCheckpoints > 0 ? checkpoints[account][nCheckpoints - 1].votes : 0;
    }

    /**
     * @notice Determine the prior number of votes for an account as of a block number
     * @dev Block number must be a finalized block or else this function will revert to prevent misinformation.
     * @param account The address of the account to check
     * @param blockNumber The block number to get the vote balance at
     * @return The number of votes the account had as of the given block
     */
    function getPriorVotes(address account, uint blockNumber) public view returns (uint96) {
        require(blockNumber < block.number, "Uni::getPriorVotes: not yet determined");

        uint32 nCheckpoints = numCheckpoints[account];
        if (nCheckpoints == 0) {
            return 0;
        }

        // First check most recent balance
        if (checkpoints[account][nCheckpoints - 1].fromBlock <= blockNumber) {
            return checkpoints[account][nCheckpoints - 1].votes;
        }

        // Next check implicit zero balance
        if (checkpoints[account][0].fromBlock > blockNumber) {
            return 0;
        }

        uint32 lower = 0;
        uint32 upper = nCheckpoints - 1;
        while (upper > lower) {
            uint32 center = upper - (upper - lower) / 2; // ceil, avoiding overflow
            Checkpoint memory cp = checkpoints[account][center];
            if (cp.fromBlock == blockNumber) {
                return cp.votes;
            } else if (cp.fromBlock < blockNumber) {
                lower = center;
            } else {
                upper = center - 1;
            }
        }
        return checkpoints[account][lower].votes;
    }

    function _delegate(address delegator, address delegatee) internal {
        address currentDelegate = delegates[delegator];
        uint96 delegatorBalance = balances[delegator];
        delegates[delegator] = delegatee;

        emit DelegateChanged(delegator, currentDelegate, delegatee);

        _moveDelegates(currentDelegate, delegatee, delegatorBalance);
    }

    function _transferTokens(address src, address dst, uint96 amount) internal {
        require(src != address(0), "Uni::_transferTokens: cannot transfer from the zero address");
        require(dst != address(0), "Uni::_transferTokens: cannot transfer to the zero address");

        balances[src] = sub96(balances[src], amount, "Uni::_transferTokens: transfer amount exceeds balance");
        balances[dst] = add96(balances[dst], amount, "Uni::_transferTokens: transfer amount overflows");
        emit Transfer(src, dst, amount);

        _moveDelegates(delegates[src], delegates[dst], amount);
    }

    function _moveDelegates(address srcRep, address dstRep, uint96 amount) internal {
        if (srcRep != dstRep && amount > 0) {
            if (srcRep != address(0)) {
                uint32 srcRepNum = numCheckpoints[srcRep];
                uint96 srcRepOld = srcRepNum > 0 ? checkpoints[srcRep][srcRepNum - 1].votes : 0;
                uint96 srcRepNew = sub96(srcRepOld, amount, "Uni::_moveVotes: vote amount underflows");
                _writeCheckpoint(srcRep, srcRepNum, srcRepOld, srcRepNew);
            }

            if (dstRep != address(0)) {
                uint32 dstRepNum = numCheckpoints[dstRep];
                uint96 dstRepOld = dstRepNum > 0 ? checkpoints[dstRep][dstRepNum - 1].votes : 0;
                uint96 dstRepNew = add96(dstRepOld, amount, "Uni::_moveVotes: vote amount overflows");
                _writeCheckpoint(dstRep, dstRepNum, dstRepOld, dstRepNew);
            }
        }
    }

    function _writeCheckpoint(address delegatee, uint32 nCheckpoints, uint96 oldVotes, uint96 newVotes) internal {
      uint32 blockNumber = safe32(block.number, "Uni::_writeCheckpoint: block number exceeds 32 bits");

      if (nCheckpoints > 0 && checkpoints[delegatee][nCheckpoints - 1].fromBlock == blockNumber) {
          checkpoints[delegatee][nCheckpoints - 1].votes = newVotes;
      } else {
          checkpoints[delegatee][nCheckpoints] = Checkpoint(blockNumber, newVotes);
          numCheckpoints[delegatee] = nCheckpoints + 1;
      }

      emit DelegateVotesChanged(delegatee, oldVotes, newVotes);
    }

    function safe32(uint n, string memory errorMessage) internal pure returns (uint32) {
        require(n < 2**32, errorMessage);
        return uint32(n);
    }

    function safe96(uint n, string memory errorMessage) internal pure returns (uint96) {
        require(n < 2**96, errorMessage);
        return uint96(n);
    }

    function add96(uint96 a, uint96 b, string memory errorMessage) internal pure returns (uint96) {
        uint96 c = a + b;
        require(c >= a, errorMessage);
        return c;
    }

    function sub96(uint96 a, uint96 b, string memory errorMessage) internal pure returns (uint96) {
        require(b <= a, errorMessage);
        return a - b;
    }

    function getChainId() internal pure returns (uint) {
        uint256 chainId;
        assembly { chainId := chainid() }
        return chainId;
    }
}
```
:::


# 最後に

今回の記事では、Bunzzの新機能『DeCipher』を使用して、Uniswapの「**Uni**」トークンのコントラクトを見てきました。
いかがだったでしょうか？
今後も特定のNFTやコントラクトをピックアップしてまとめて行きたいと思います。

普段はブログやQiitaでブロックチェーンやAIに関する記事を挙げているので、よければ見ていってください！

https://chaldene.net/

https://qiita.com/cardene
