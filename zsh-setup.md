# zsh環境構築手順
Ubuntu Zsh環境構築手順（Oh My Zsh + 各種プラグイン）Ubuntuインストール直後の状態から、指定されたプラグイン（Oh My Zsh組み込み ＋ 外部プラグイン）をすべて利用可能にするための全手順です。1. 必須パッケージのインストールとZshへの切り替えまずは Git と Zsh をインストールし、デフォルトシェルを Zsh に変更します。
# パッケージリストの更新
`sudo apt update`

# zsh と git（プラグインのクローンに必要）のインストール
`sudo apt install -y zsh git`

# デフォルトシェルをzshに変更

`chsh -s $(which zsh)`

注意: chsh を実行した後は、一度システムからログアウトして再ログインするか、Ubuntuを再起動することでシェルが完全に切り替わります。

# Oh-My-Zshのインストール
Zshのフレームワークである Oh My Zsh をインストールします。

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

インストール中に「デフォルトシェルをZshに変更するか？」と聞かれた場合は、すでに変更していても Y を押して進めて問題ありません。<br>
<br>外部プラグイン・依存ツールのインストール提示されたプラグインのうち、GitHubから別途ダウンロード（クローン）が必要なもの、およびシステム側に依存ツールが必要なもの（fzf など）をインストールします。<br>
<br>外部プラグインのダウンロード（Oh My Zsh用）以下のコマンドを順番に実行し、Oh My Zshのカスタムプラグインディレクトリにクローンします。

## zsh-completions
`git clone [https://github.com/zsh-users/zsh-completions](https://github.com/zsh-users/zsh-completions) ${ZSH_CUSTOM:-${HOME}/.oh-my-zsh/custom}/plugins/zsh-completions`

## zsh-autosuggestions
`git clone [https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`

## zsh-syntax-highlighting
`git clone [https://github.com/zsh-users/zsh-syntax-highlighting.git](https://github.com/zsh-users/zsh-syntax-highlighting.git) ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

## you-should-use
`git clone [https://github.com/MichaelAquilina/zsh-you-should-use.git](https://github.com/MichaelAquilina/zsh-you-should-use.git) ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/you-should-use`

## fzf（コマンドラインファジーファインダー）のインストール
fzfプラグインを動作させるには、システム本体に fzf コマンドがインストールされている必要があります。<br>
`sudo apt install -y fzf`

## zshrc の設定（プラグインの有効化）
ホームディレクトリにある `.zshrc` ファイルを編集し、プラグインを読み込ませます。（`nano ~/.zshrc` や `vim ~/.zshrc` などのエディタで開いてください）<br>
<br>plugins=(...) の書き換え設定ファイル内にある初期設定の `plugins=(git)` という記述を探し、以下のように書き換えます。
```
plugins=(
    git aliases copypath history docker github composer 
    zsh-completions zsh-autosuggestions zsh-syntax-highlighting
    you-should-use
    z sudo fzf
)
```

## 補完（zsh-completions）用の設定追加
zsh-completions を正常に動作させるため、`.zshrc` 内の `source $ZSH/oh-my-zsh.sh` よりも前の行に以下の記述を追加してください。<br>
zsh-completions の読み込み設定 (`source $ZSH/oh-my-zsh.sh` より前に書く)

`fpath+=${ZSH_CUSTOM:-${HOME}/.oh-my-zsh/custom}/plugins/zsh-completions/src`

設定の反映すべての編集が終わったらファイルを保存して閉じ、以下のコマンドで設定を現在の端末に反映させます。
`source ~/.zshrc`

これで指定されたすべてのプラグインが有効になります。
