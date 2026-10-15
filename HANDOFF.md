# KomenoTV Auto 引き継ぎ

更新: 2026-10-15

- 正本: C:\Users\rikou\Workspace\Fermata、ブランチ codex/komenotv-auto。
- フォーク: https://github.com/KxOxUxMxExI/Fermata、upstream: AndreyPavlenko/Fermata。
- ベースはFermata 2.0.2 / 65a3c163。認識試験用のアプリ名 `KomenoTV Auto`、ID `me.aap.fermata.auto.komenotv`。起動先固定・自動全画面は未実装。
- Pixel 6a / Android 17 / Android Auto 17.8へ別アプリとして追加済み。元の `me.aap.fermata.auto.dear.google.why` は版・初回導入日時・更新日時が試験前後で一致。アンインストール・データ消去なし。

## 今回の実機・DHU結果

- ユーザーの認証解除後、スマホで試作版のフォルダー画面・Webブラウザを操作できた。7100番のKomenoTVを開き、480p HEVC表示と実際の放送映像の進行を短時間確認。音声の実聴は未実施。
- 停車状態の800x480 / タッチなし / rotaryのDHU接続は成功。既存Fermata Autoの起動はサービスのアプリIDで照合できた。
- 試作版は `Fermata Media Service` の音声用項目だけ表示・起動できた。動画用の `KomenoTV Auto` と試作版Mirrorは、DHUにもスマホのランチャーカスタマイズの表示・非表示一覧にも出なかった。車載Web動画・全画面の試験には到達していない。
- 初回はADB導入（installer=null / initiating=com.android.shell）。同一署名・同一ハッシュのAPKを既存Shizuku/KingInstallerでデータを保持して更新した後は、installer / initiatingとも `com.google.android.packageinstaller`。Playストア扱いにはならなかった。既存6a版Fermataと同じ導入元になっても動画用項目は出ず、Android Autoプロセス再起動・DHU再接続後も同じ結果。
- ログの `Uses for TEMPLATE not defined` は既存Fermataにも出るため、試作版だけが出ない原因の証明にはならない。アプリID・署名・Android Autoの認識条件は未切り分け。導入元だけを原因と断定しない。
- 短時間再生を終了し、試験アプリのプロセスなしを確認。DHU・所有するTCP 54010転送・ヘッドユニットサーバー・一時起動したShizukuを終了。画面消灯設定は元の値15へ復旧。KingInstallerのShizukuスイッチも元のオフへ復旧。Android Auto開発者モードと元からオンだった提供元不明アプリは保持し、後者のchecked=trueを再確認した。
- 証跡はGit対象外の `dist/dhu-smoke/`。スマホ映像は `phone-video-{1,2}.png`、DHU音声UIは `dhu-prototype-media-open.png`、更新後一覧は `aa-restart-launcher-middle.xml`、導入元は `prototype-after-king-package.txt`、後片付けは `cleanup.json`。

## ビルド

- Universal APKビルドとlintVital成功（2m44s）。ARM64のみ、アプリID・ラベル・CarService登録をAPKから確認。今回は動作コードを変更しておらず、再ビルドしていない。
- 専用RSA 3072署名鍵を使用。apksigner検証成功、自作鍵と一致し公式Fermata APKとは異なる。鍵・パスワードはGit対象外。
- APK: `dist/komenotv-auto-2.0.2-probe.apk`（154454349 bytes）。SHA-256: `071EF5B2E505BB3ED0D441B2A8851A8C94B1724A992AE98A32D6E30434558F51`。KingInstaller用に6aのDownloadへ同名ファイルをコピーし、ハッシュ一致を確認。
- Windowsビルド用変更はWhisperの2ファイルのみ: NDKホストOS参照、短くCPU別に分離した生成先、本体と共通のABI指定。詳細はKOMENOTV_AUTO.md。
- Whisper依存の浮動参照は維持。今回取得commit: whisper.cpp=0c7be696371069e0baa2515ed7aa08d5a5473aa6、Vulkan-Hpp=1a24b015830c116632a0723f3ccfd1f06009ce12、SPIRV-Headers=86f980c731e62ae4eaf383d320449d71687936bf。将来のバイナリ同一再現は未保証。

## 次の段階

独立アプリの動画用CarServiceがAndroid Autoに認識される条件を先に切り分ける。現在の試作は実車チェックへ進める状態とは判定しない。既存Fermataの置換・削除、共通公開鍵への変更、Android Autoのデータ消去は行っていない。
認識が成立してから、ユーザーが停車中のCX-5で一覧表示・起動を確認する。その判定後に7100番の固定起動と、アプリ起動/復帰時の全画面化を実装する。家庭用7000・車載7100のWeb版・配信サーバーは変更不要。Android Autoのドックはアプリ領域の全画面とは別。
