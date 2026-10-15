# KomenoTV Auto 引き継ぎ

更新: 2026-10-15

- 正本: C:\Users\rikou\Workspace\Fermata、ブランチ codex/komenotv-auto。
- フォーク: https://github.com/KxOxUxMxExI/Fermata、upstream: AndreyPavlenko/Fermata。
- ベースはFermata 2.0.2 / 65a3c163。認識試験用のアプリ名 `KomenoTV Auto`、ID `me.aap.fermata.auto.komenotv`。起動先固定・自動全画面は未実装。
- Pixel 6a / Android 17 / Android Auto 17.8へ別アプリとして追加済み。元の `me.aap.fermata.auto.dear.google.why` は版・初回導入日時・更新日時が試験前後で一致。アンインストール・データ消去なし。

## 今回の実機・DHU結果

- ユーザーの認証解除後、スマホで試作版のフォルダー画面・Webブラウザを操作できた。7100番のKomenoTVを開き、480p HEVC表示と実際の放送映像の進行を短時間確認。音声の実聴は未実施。
- 停車状態の800x480 / タッチなし / rotaryのDHU接続は成功。既存Fermata Autoの起動はサービスのアプリIDで照合できた。
- 通常ランチャーには試作版の `Fermata Media Service` だけ表示。動画用の `KomenoTV Auto` と試作版Mirrorは、通常一覧とカスタマイズの表示・非表示一覧には出なかった。追加検証でアプリモードを「デベロッパー」にすると「すべての車両アプリ」が出現し、その診断一覧に `KomenoTV Auto` があった。そこから起動し、試作版CarServiceへのAndroid Auto接続をdumpsysで照合できた。
- 診断一覧から起動した試作版で、回転・方向・決定操作だけでWebブラウザへ移動。スマホで指定済みの7100番が開き、実際の映像の進行を確認。既存メニューの `Full screen` を選ぶとURL欄とFermataバーが消え、アプリ領域全画面で映像が継続した。Android Autoのドックは残る。自動起動・自動全画面の成功を示すものではない。
- 初回はADB導入（installer=null / initiating=com.android.shell）。同一署名・同一ハッシュのAPKを既存Shizuku/KingInstallerでデータを保持して更新した後は、installer / initiatingとも `com.google.android.packageinstaller`。Playストア扱いにはならなかった。既存6a版Fermataと同じ導入元になっても動画用項目は出ず、Android Autoプロセス再起動・DHU再接続後も同じ結果。
- ログの `Uses for TEMPLATE not defined` は既存Fermataにも出るため、試作版だけが出ない原因の証明にはならない。通常一覧に出ない理由と実車での認識条件は未切り分け。導入元だけを原因と断定しない。
- 短時間再生を終了し、試験アプリのプロセスなしを確認。DHU・所有するTCP 54010/65515転送・ヘッドユニットサーバー・一時起動したShizukuを終了。画面消灯設定は元の値15へ復旧。KingInstallerのShizukuスイッチも元のオフへ復旧。Android Auto開発者モードと元からオンだった提供元不明アプリは保持し、後者のchecked=trueを再確認した。アプリモードは追加試験で「デベロッパー」に設定し、その選択状態をUIで確認して実車チェック用に保持。変更前のダイアログは4項目すべて未選択だった。
- 証跡はGit対象外の `dist/dhu-smoke/`。スマホ映像は `phone-video-{1,2}.png`、診断一覧は `dhu-all-car-apps-fermata.png`、試作CarServiceは `allcar-launch-services.txt`、DHU映像は `dhu-video-{1,2}.png`、全画面は `dhu-komenotv-fullscreen.png` / `dhu-fullscreen-video-2.png`、最終モードは `aa-final-mode.xml`、後片付けは `cleanup-final.json`。

## ビルド

- Universal APKビルドとlintVital成功（2m44s）。ARM64のみ、アプリID・ラベル・CarService登録をAPKから確認。今回は動作コードを変更しておらず、再ビルドしていない。
- 専用RSA 3072署名鍵を使用。apksigner検証成功、自作鍵と一致し公式Fermata APKとは異なる。鍵・パスワードはGit対象外。
- APK: `dist/komenotv-auto-2.0.2-probe.apk`（154454349 bytes）。SHA-256: `071EF5B2E505BB3ED0D441B2A8851A8C94B1724A992AE98A32D6E30434558F51`。KingInstaller用に6aのDownloadへ同名ファイルをコピーし、ハッシュ一致を確認。
- Windowsビルド用変更はWhisperの2ファイルのみ: NDKホストOS参照、短くCPU別に分離した生成先、本体と共通のABI指定。詳細はKOMENOTV_AUTO.md。
- Whisper依存の浮動参照は維持。今回取得commit: whisper.cpp=0c7be696371069e0baa2515ed7aa08d5a5473aa6、Vulkan-Hpp=1a24b015830c116632a0723f3ccfd1f06009ce12、SPIRV-Headers=86f980c731e62ae4eaf383d320449d71687936bf。将来のバイナリ同一再現は未保証。

## 次の段階

ユーザーが停車中のCX-5で、6aの「すべての車両アプリ」→「KomenoTV Auto」から起動できるか確認する。DHUの診断一覧経由の成功は、通常一覧や実車での認識を保証しない。通常一覧へ出ない条件も未解決。既存Fermataの置換・削除、共通公開鍵への変更、Android Autoのデータ消去は行っていない。
実車での起動が成立してから7100番の固定起動と、アプリ起動/復帰時の全画面化を実装する。家庭用7000・車載7100のWeb版・配信サーバーは変更不要。Android Autoのドックはアプリ領域の全画面とは別。
