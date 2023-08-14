---
title: "Bunzz 『DeCipher』の使い方紹介"
emoji: "🤖"
type: "tech"
topics:
  - "blockchain"
  - "ブロックチェーン"
  - "solid"
  - "ethereum"
  - "smartcontract"
published: true
published_at: "2023-07-26 18:43"
---

# はじめに

スマートコントラクト開発を支援するプラットフォームである「**Bunzz**」から、AIを使用してスマートコントラクトのドキュメントを自動生成するサービスである「**DeCipher**」が2023年7月26日にローンチされました🎉

https://twitter.com/kenta_web3/status/1684015070089723904

https://www.bunzz.dev/decipher

クローズドβ版の時から触らせていただき、ドキュメント生成の精度の高さに驚いていました。
以下の記事などで生成されたドキュメントを日本語に翻訳したりしていました。

https://zenn.dev/cryptogames/articles/dab1107e0f9e0f

https://zenn.dev/cryptogames/articles/e4e90738a97d9f

この記事では、「**DeCipher**」の基本的な使い方や機能と生成されたドキュメントの中身を紹介していきます。
また、最後にクローズドβ版よりも精度が上がっているらしいので、実際に生成されたドキュメントを比較してみようと思います！

:::message
この記事はあくまで個人が「**DeCipher**」を使用してみての記事になります。
間違った部分などもあるかと思いますが、その際はコメントなどで教えてください。
:::

それでは早速見ていきましょう！

# 使用方法

## 基本的な使い方

まずは基本的な使い方から紹介していきます。

基本的な使い方としては、ブロックチェーンエクスプローラーというブロックチェーンに関する情報を調査できるツールのリンクを以下に貼り付けます。

https://www.bunzz.dev/decipher

まずは、以下のEtherscan上のBAYCのURLをコピーしてください。

https://etherscan.io/token/0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d

![](https://storage.googleapis.com/zenn-user-upload/934f26e8a9af-20230726.png)

上の画像の赤枠部分に先ほどコピーしたURLを貼り付けて、「**Generate Documentation**」をクリックしてください。

https://twitter.com/cardene777/status/1684124031547772929

そうすると上の動画のように一瞬でドキュメントが生成されます。

## ドキュメントの項目

次に生成されたドキュメントの項目についてみていきましょう！

![](https://storage.googleapis.com/zenn-user-upload/0703b9a43242-20230726.png)

上の画像の赤枠のように、大きく6つに分かれます。
1つずつ簡単に紹介していきます。

### Overview

名前の通り、この項目ではコントラクトの概要について説明しています。
コントラクトによっては、この概要部分が空っぽのこともあります。

### How To Use

コントラクトの中身の説明をしてくれている項目です。
どのような機能があって、どのようなて手順で使用すれば良いのかがわかるので便利です。

### Module Parameters

ここではコントラクトをデプロイするときに、一度だけ実行される`constructor`に渡す引数の説明がされています。
1つずつ引数の名前・型・簡単な説明が書かれています。

### Functions

この項目では、コントラクト内の関数についてまとめられています。
左側のサイドメニューに、読み込み系（**READ**）と書き込み系（**WRITE**）の2つに大きく分けられた状態で一覧になって表示されています。
各関数名をクリックすると、関数の説明と引数が表示されます。
コントラクトを読んでいるとき、それぞれの関数が何をしているのかわからなくなることが多いのでめちゃくちゃ役立ちそうです。

### Events

この項目では、コントラクト内に定義されているイベントについてまとめられています。
イベントを発行することで、ブロックチェーン上にログを残すことができます。

### Code

この項目では、コードについてまとめらています。
ディレクトリ構造になっていて、読み込んでいるファイルのコードまで見れるようになっています。
通常コントラクトを読んでいるときは、読み込んでいるファイルがあればわざわざ該当のGithubまで行ってコードを確認する必要がありました。
しかし、「**DeCipher**」ではコード一式を「**DeCipher**」上で確認できるのでものすごく便利です。
コードを読む時のあっち行って、こっち行ってがなくなるのは本当に助かります...💫

## 対応Explorer

対応しているExploerは以下になります。
:::message
ページ内に記載されていたExplorerを抜き出してみました。
もしかしたらもっと対応しているExplorerはあるかもしれません。
:::

- **Etherscan**
	- https://etherscan.io/
- **ARBISCAN**
	- https://arbiscan.io/
- **BscScan**
	- https://bscscan.com/
- **FTMScan**
	- https://ftmscan.com/
- **Optimism**
	- https://optimistic.etherscan.io/
- **SNOWTRACE**
	- https://snowtrace.io/
- **polygonscan**
	- https://polygonscan.com/
- **Moonbeam**
	- https://moonscan.io/

# Chrome拡張

「**DeCipher**」はChrome拡張も提供してくれています。

https://chrome.google.com/webstore/detail/bunzz-decipher/ekkgjdbkmlcaepohenggjpfadklklpaj?hl=en&auth

この拡張機能がめちゃくちゃ便利です。

![](https://storage.googleapis.com/zenn-user-upload/2c3359d8e1dc-20230726.png)

上の画像のようにExplorerで特定のコントラクトを開くと、右下に「**Click Here**」というボタンが出てきます。
このボタンを押してみましょう。

そうすると前章で紹介したように、コントラクトのドキュメントを自動で生成してくれます！

https://app.bunzz.dev/decipher/chains/1/addresses/0x23581767a106ae21c074b2276d25e5c3e136a68b

便利すぎます...✨


# DeCipher GPT

生成されたドキュメントに対して、ChatGPTを使用して不明点などを質問することができる機能も備わっています。

![](https://storage.googleapis.com/zenn-user-upload/fdbe3a381d9a-20230726.png)

上の画像のように、生成されたドキュメントの右下に「**DeCipher GPT**」というボタンがあるので、「**Ask**」を押してみます。

![](https://storage.googleapis.com/zenn-user-upload/657db5473758-20230726.png)

上の画像のようにテキストを入力できるようになります。
では、実際に以下の質問をしてみます。

:::message
What are some of the unique features of this contraption?
:::

:::message
このコントラクトの特徴的な機能について教えてください。
:::

そうすると以下のような回答をしてもらえました。

:::message
Based on the context, some of the unique features of the BoredApeYachtClub contract are: 1. Unique NFT Attributes and Traits: The contract allows for each NFT to have a unique set of attributes and traits. This adds to the uniqueness and value of each NFT. 2. ERC721 Standard Implementation: The contract extends the ERC721 Non-Fungible Token Standard, which provides a standardized interface for NFTs on the Ethereum blockchain. 3. Ownership Management: The contract includes functions like renouncing ownership and transferring ownership, which allow for the management of contract ownership. 4. Provenance Tracking: The contract includes a variable called BAYC_PROVENANCE, which is used for tracking the provenance or origin of the NFTs. 5. Sale Management: The contract includes variables like apePrice, maxApePurchase, and saleIsActive, which are used to manage the sale of the NFTs. Please note that this answer is based on the provided context and may not include all the unique features of the BoredApeYachtClub contract.
:::

:::message
このような背景から、BoredApeYachtClub契約のユニークな特徴をいくつか挙げる： 1. ユニークなNFTの属性と特徴： この契約では、各NFTが独自の属性と特徴を持つことができます。これにより、各NFTの独自性と価値が高まります。2. ERC721標準の実装： この契約はERC721 Non-Fungible Token Standardを拡張したもので、イーサリアム・ブロックチェーン上のNFTに標準化されたインターフェースを提供します。3. 所有権の管理： コントラクトには、所有権の放棄や所有権の移転といった機能が含まれており、コントラクトの所有権を管理することができます。4. 証明追跡： 契約にはBAYC_PROVENANCEと呼ばれる変数が含まれ、NFTの出所を追跡するために使用される。5. 販売管理： 契約にはapePrice、maxApePurchase、saleIsActiveなどの変数が含まれ、NFTの販売管理に使用されます。この回答は、提供されたコンテキストに基づくものであり、BoredApeYachtClub契約に固有の機能をすべて含んでいるわけではないことにご注意ください。
:::

ちゃんと回答してもらえています！
これは便利ですね。

# Bunzz CLI

以下のBunzz CLIを使用することで、生成したドキュメントのコントラクトコードをローカルにダウンロードしたり、コードのビルド、デプロイができてしまいます。

https://www.npmjs.com/package/bunzz?activeTab=readme

![](https://storage.googleapis.com/zenn-user-upload/639df9e235b8-20230726.png)

手順としては上の画像の赤枠部分の「**Edit this code on IDE**」をクリックすると、以下のようなモーダルが開くので、任意のコマンドを実行することができます。

![](https://storage.googleapis.com/zenn-user-upload/8194e5370f44-20230726.png)

エンジニアからしたら手間が省けて省けて仕方ないですね...🚀

# その他補足

## 生成時間

基本的にドキュメントの生成時間は表示されている通り1分から5分ほどで、コントラクトの長さによって異なります。
ただ、一度ドキュメントが生成されたことがあるコントラクトについては、生成時間が一気に短くなっている気がします。
使う人が増えれば増えるほど生成されたドキュメントが増えるので、ドキュメントの生成時間が早まる確率が上がりそうですね！

## フィードバック

生成されたドキュメント内には、フィードバックできるボタンが用意されています。

![](https://storage.googleapis.com/zenn-user-upload/964d4da5e606-20230726.png)

上の画像の赤枠部分をクリックするとメッセージ付きのフィードバックを送れるようになっています。

- 「ここを〇〇したらもっと便利！」
- 「ここの部分で不具合がある...」
- 「この機能めちゃくちゃ便利！」

上記のような改善点や良い点などがあればぜひフィードバックしましょう！

# 最後に

この記事では、スマートコントラクトのドキュメントを自動生成するサービスである「**DeCipher**」についてまとめてきました。

いかがだったでしょうか？
この記事を読んで、「**DeCipher**」の魅力や可能性を感んじて使用する人が増えたら嬉しいです！
ぜひみんなで「**DeCipher**」を使い倒しながらスマートコントラクトの開発効率を上げていきましょう！

また、以下のように生成されたドキュメントを日本語に翻訳しながら、補足・要約しているので、生成されたドキュメントでわからないことがあるときは、ぜひ参考にしてください！
（参考になるように記事どんどん増やしていきます🔥 ）

https://zenn.dev/cryptogames/articles/dab1107e0f9e0f

https://zenn.dev/cryptogames/articles/e4e90738a97d9f
