# Autonomous_Agent_Spoken_Dialogue_Plugin_for_Minecraft

本プラグインは、マインクラフトに自律性を持つAIエージェントを追加し、音声対話を実現するものである。
 
## 事前準備
　本プラグインは、以下の手順に従って事前準備を行ってください。
 動作確認は Windows 11 上で行っています。
 また、CUDAを用いた処理(音声合成)を活用しているため、NVIDIAgpuが搭載されたPCが望ましい。

### Minecraft のインストール
[Minecraft Launcher](https://www.minecraft.net/)をインストールし，Minecraft: Java Edition（バージョン1.19）をプレイできるようにしてください．ユーザとして使用します．Java Edition のライセンスが必要です．

Minecraft Launcherの「起動構成」タブの「新規作成」から，バージョン1.19 を選択してください．

### Docker のインストール
  本プラグインでは、Minecraftのマルチワールドサーバーの作成、データ送受信用のrabbitoMQにおいて、Dockerを使用しております。
  [Docker Docs](https://docs.docker.com/desktop/setup/install/windows-install/) からインストーラをダウンロードして実行してください。
  
### Node.js のインストール
　Mineflayer(Minecraftボット制御用javascript api)の制御において、javascript実行のためNode.jsを使用します。
　公式HP:[Node.js](https://nodejs.org/en)からインストーラをダウンロードしてインストールを実行してください。
 以下のコマンドが実行できればダウンロード完了です。
```
node --version
```
実行例:v22.20.0

### VOICEVOX のインストール
音声合成moduleに VOICEVOX engineを使用しています
下記のURLからダウンロードをして、7-zip等を活用し、二つのzipファイルをまとめて解凍してください。 
解凍後、フォルダ名を以下のように変更していただくと同封されているbatファイルから実行することができます。
`windows-nvidia -> voicevox_server`

Windows版 GPU利用 CUDA
[part1](https://github.com/VOICEVOX/voicevox_engine/releases/download/0.25.0/voicevox_engine-windows-nvidia-0.25.0.7z.001)
[part2](https://github.com/VOICEVOX/voicevox_engine/releases/download/0.25.0/voicevox_engine-windows-nvidia-0.25.0.7z.002)

また、[Dockerを利用したVOICEVOXの活用](https://hub.docker.com/r/voicevox/voicevox_engine)も可能となっていますが、開発環境において実行時間が400msを超えており、リアルタイム性の面から非推奨とさせていただきます。

### openai api key の取得
multi-modal-generator として LLM を活用しており、本プラグインでは openai が提供する gpt-40 での利用を想定しております。
[公式HP](https://openai.com/ja-JP/api/)より openai のアカウント作成を行い、apikeyの取得を行ってください

### Google cloud Speech To Text apikey の取得
音声認識moduleにGoogle cloud Speech To Text を使用しています。
[公式HP](https://console.cloud.google.com/) Google Cloud Platform のアカウント作成を行い、apikey(jsonファイル) の取得を行ってください

## 環境構築

### 1.本プラグインのダウンロード
githubから本プラグインおよび依存パッケージのダウンロードを行います。
以下のコマンドをコマンドプロンプトで実行してください
```
git clone https://github.com/Koichiro-terao/Autonomous_Agent_Spoken_Dialogue_Plugin_for_Minecraft.git Spoken_Dialogue_Plugin
cd Spoken_Dialogue_Plugin
git clone https://github.com/sagara-r/BeliefNest BeliefNest
```

### 2.Node.js 環境の作成
Minecraftエージェント実行のため、javascript実行に関するパッケージをインストールする
```
cd BeliefNest\belief_nest\env\mineflayer
npm install
```

## 各 apikey の設定
Minecraft_Plugin_debug/plugin のディレクトリに conifg.yaml があります。
以下のように、取得した各apikeyおよび、マインクラフトのアカウント名を記述してください
```
openai_apikey: sk-xxx (openai の apikey)
json_key_path: google_speech_to_text の apikey の path

player_minecraft_id: マインクラフトのアカウント名

call_yourname_by_aiagent: 呼ばれ方
```
## 使用方法

### サンプルプログラム実行までの手順
手順は以下のようになります。
1.Docker Desktopの起動
2.各サーバの起動
3.main.py実行

#### 1.Docker Desktop の起動
サーバの起動にdockerを使用するため、Docker Desktop を起動してください。

#### 2.各サーバの起動
以下のサーバを起動します。
- RabbitMQサーバ：Beliefnest\rabbitmq.bat を起動
- Minecraftサーバ：BeliefNest\mc_server\flat\mc_server.bat を起動
- VOICEVOXサーバ：windows-nvidia\run.exe --use_gpu を起動(GPU利用のため、コマンドプロンプトからの起動を推奨)

windows環境の場合、plugin_setup.batを実行すると上記の３つのサーバが起動します。（batファイル内のpathをお手元の環境に合わせて修正する必要があります。）

#### 3.main.py の実行
Spoken_Dialogue_Plugin\plugin にある`main.exe`を実行すると起動します。

ターミナルに以下の記号が出力されれば、実行完了です。
```
start [y]/n:
```
ターミナルでEnterをクリックすると対話が可能となります。
