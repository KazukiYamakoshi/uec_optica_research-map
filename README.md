# uec_optica_research-map

電気通信大学 UEC-Optica Student Chapter 向けの **Photo Rally 研究室マップ** です。  
GitHub Pages 上で公開され、東・西キャンパスのマップから研究室情報を確認できます。

**公開 URL:** https://kazukiyamakoshi.github.io/uec_optica_research-map/

---

## リポジトリ構成

```
uec_optica_research-map/
├── index.html              … サイト本体（HTML / CSS / JavaScript がすべてここ）
├── images/
│   ├── east-map.png        … 東キャンパス背景マップ
│   ├── west-map.png        … 西キャンパス背景マップ
│   └── PhotoRally_2026_v2/ … 参考用スライド画像（サイトでは未使用）
├── README.md               … このファイル
└── LICENSE
```

> このサイトは **1 ファイル（`index.html`）＋ 画像** だけで動きます。  
> Node.js や npm install は不要です。

---

## 1. 環境構築（初回のみ）

### 必要なもの

| ソフトウェア | 用途 |
|-------------|------|
| [Visual Studio Code](https://code.visualstudio.com/) | ファイル編集 |
| [Git](https://git-scm.com/) | 変更の保存・GitHub への反映 |
| [GitHub アカウント](https://github.com/) | リポジトリへのアクセス |

Python 3 はローカルプレビュー用（後述）に使います。Windows なら Microsoft Store 版でも構いません。

### VS Code 拡張機能（推奨）

VS Code を開き、拡張機能タブから以下をインストールします。

- **Live Server**（作者: Ritwick Dey）… 保存のたびにブラウザが自動更新される（任意）
- **Japanese Language Pack** … 日本語 UI（任意）

### リポジトリを PC に取得

ターミナル（PowerShell など）で作業用フォルダに移動し、クローンします。

```powershell
git clone https://github.com/KazukiYamakoshi/uec_optica_research-map.git
cd uec_optica_research-map
```

すでにフォルダがある場合は、最新版を取得します。

```powershell
cd uec_optica_research-map
git pull
```

VS Code でフォルダを開きます。

```powershell
code .
```

---

## 2. ローカルでプレビューする

GitHub Pages への反映には数分かかるため、**編集中はローカルで確認**してください。

### 方法 A: Live Server（おすすめ）

1. VS Code で `index.html` を開く
2. エディタ上で右クリック → **Open with Live Server**
3. ブラウザが開く。ファイルを保存するたびに自動で更新される

### 方法 B: Python 簡易サーバー

リポジトリのルートフォルダで実行します。

```powershell
python -m http.server 8080
```

ブラウザで http://localhost:8080 を開きます。  
終了するときはターミナルで `Ctrl + C`。

### 方法 C: ブラウザで直接開く

`index.html` をダブルクリックしても動きますが、一部の環境では画像パスがうまく読めないことがあります。**A か B を推奨**します。

---

## 3. `index.html` の編集

`index.html` 内にコメントが詳しく書いてあります。主な編集箇所は次のとおりです。

### ページ上部の説明文

`<header>` タグ内の HTML を直接書き換えます。

### 研究室の追加・変更・削除

`<script>` 内の **`eastLabData`**（東）または **`westLabData`**（西）を編集します。

1 研究室 = 1 行です。

```javascript
{
  building: "bld6",           // 所属建物（eastBuildings / westBuildings のキー）
  name: "宮本 研",            // 表示名
  room: "東6号館 617",        // モーダルに表示する場所
  desc: "研究内容の説明...",  // モーダルに表示する説明文
  color: "#E07B39",           // ラベルの色
  roomLabel: "617",           // 丸の中の部屋番号
  labelX: 2,                  // ラベルの横位置（0=左端, 100=右端）
  labelY: 10,                 // ラベルの縦位置（上から下へ増える）
}
```

景品抽選所のように特別な見た目にしたい場合は `isPrize: true` を追加します。

**新しい建物**に研究室を置く場合は、先に **`eastBuildings` / `westBuildings`** に建物アンカーを追加してください。  
`name` フィールドの文字列が、**その建物の最上段ラベルの上**に自動表示されます。

```javascript
// 例: マップ画像内の位置（左上 0,0 ～ 右下 100,100 の百分率）
bld6: { name: "東6号館", mapX: 54, mapY: 36 },
// 建物名の位置を微調整する場合（PC 表示。単位は %）
bld6: { name: "東6号館", mapX: 54, mapY: 36, titleOffsetX: 2, titleOffsetY: -1 },
```

**東6号館**の研究室は部屋番号の**降順**で並べ、`labelY` は **8 % 刻み**（22, 30, 38, …）で等間隔にしてください。

**西キャンパス**の建物と研究室の対応は次のとおりです。

| 建物 | 研究室 |
|------|--------|
| 西1号館 | 一色 研 |
| 西2号館 | 張・庄司・中村・坂野 研 |
| 西7号館 | 上記以外（白川・Nayak・中川・丹治・戸倉川・岩國・武者 研） |

西4・西3・西8・西9号館には研究室はありません。

**西7号館**など左側の一覧は部屋番号**降順**、`labelY` は **10 % 刻み**（24, 34, 44, …）で等間隔にしてください（西マップは横長のため、東6号館の 8 % より広め）。

### 配色・フォント・レイアウト

`<style>` 内を編集します。

| 場所 | 内容 |
|------|------|
| `:root` | サイト全体のテーマカラー |
| `.east-map-inner` の `margin-left` | 東キャンパス：左側ラベル用の余白（PC 表示） |
| `.west-map-inner` の `margin-left/right` | 西キャンパス：左右ラベル用の余白（PC 表示） |
| `.marker-room` / `.marker-name` | 部屋番号・研究室名のサイズ |
| `.building-title` / `:root` の `--building-title-size` | 建物名（東6号館など）のフォントサイズ（PC / スマホ） |
| `@media (max-width: 640px)` | **スマートフォン向け**レイアウト（マップ下に研究室一覧） |

#### スマートフォン表示について

画面幅 **640px 以下**では、PC 向けの「マップ上にラベルを重ねる」レイアウトをやめ、次の構成に自動切り替えします。

1. **マップ画像**（画面幅いっぱい）
2. **建物名ごとの研究室一覧**（タップで詳細モーダル）

折れ線は非表示になります。スマホでの見た目を変えたい場合は `@media (max-width: 640px)` ブロックを編集してください。

### 背景マップ画像の差し替え

`images/east-map.png` または `images/west-map.png` を同じファイル名で上書きします。  
ファイル名を変えた場合は `index.html` 内の `<img src="...">` も合わせて変更してください。

---

## 4. 変更を GitHub に反映する

ローカルで問題なければ、Git でコミットして push します。

```powershell
# 変更内容を確認
git status
git diff

# 変更をステージング
git add index.html
# 画像も変えた場合
git add images/

# コミット（メッセージは変更内容が分かるように）
git commit -m "東キャンパスの研究室情報を更新"

# GitHub へ送信
git push origin main
```

> push するにはリポジトリへの **書き込み権限** が必要です。  
> 権限がない場合は、リポジトリ管理者に Collaborator として追加してもらうか、Fork → Pull Request の流れを使います。

### GitHub Pages への反映

このリポジトリは **GitHub Pages** で公開されています。  
`main` ブランチへの push が完了すると、通常 **1〜3 分程度** で公開 URL に反映されます。

- 公開 URL: https://kazukiyamakoshi.github.io/uec_optica_research-map/
- 反映設定の確認: GitHub リポジトリ → **Settings** → **Pages**
  - Source: **Deploy from a branch**
  - Branch: **main** / **/ (root)**

反映されないときは、Settings → Pages の **Latest deployment** の状態を確認してください。

---

## 5. 作業の流れ（まとめ）

```
① git pull                          … 最新版を取得
② VS Code で index.html を編集
③ Live Server または python -m http.server でローカル確認
④ git add → git commit → git push   … GitHub へ反映
⑤ 1〜3 分待って公開 URL をブラウザで確認（キャッシュが残る場合は Ctrl+F5）
```

---

## 6. よくあるトラブル

| 症状 | 対処 |
|------|------|
| ローカルでは表示されるが URL に反映されない | push が成功しているか確認。Pages のデプロイ完了を待つ（5 分程度） |
| 古い内容が表示される | ブラウザのスーパーリロード（`Ctrl + F5`） |
| 折れ線の位置がずれる | ウィンドウサイズ変更後は自動再描画される。`labelX` / `labelY` または `mapX` / `mapY` を調整 |
| 新しい建物にラベルを付けられない | `eastBuildings` / `westBuildings` に建物を追加し、`building` キーを一致させる |
| `git push` で認証エラー | GitHub にログイン（Personal Access Token または GitHub Desktop の利用） |

---

## 7. 問い合わせ先

- リポジトリ: https://github.com/KazukiYamakoshi/uec_optica_research-map
- 詳細なコードの説明は `index.html` 内のコメントも参照してください
