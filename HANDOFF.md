# KomenoTV Auto 引き継ぎ

更新: 2026-10-15

- 正本: C:\Users\rikou\Workspace\Fermata、ブランチ codex/komenotv-auto。
- フォーク: https://github.com/KxOxUxMxExI/Fermata、upstream: AndreyPavlenko/Fermata。
- ベースは導入済みと同じFermata 2.0.2 / 65a3c163。今回は認識試験用で、アプリ名 `KomenoTV Auto`、ID `me.aap.fermata.auto.komenotv`。起動先固定と自動全画面は未実装。
- Pixel 6a / Android 17 / Android Auto 17.8へ別アプリとして追加済み。元の `me.aap.fermata.auto.dear.google.why` の版・初回導入日時・更新日時が前後一致。アンインストール・データ消去・既存アプリの設定変更なし。
- 今回はADB導入。新アプリのinstallerPackageName=null、initiatingPackageName=com.android.shell。KingInstallerでの導入と車載一覧への認識は未確認。提供元不明アプリ設定は今回変更していない。
- am start -WはStatus: ok、プロセス起動あり。直後の同プロセスの200行にFATAL EXCEPTIONなし。ただしkeyguard showing=true / inputRestricted=true / occluded=falseでロック画面が前面。スマホ上のアプリ表示・操作成功とは扱わない。
- 通常のwm dismiss-keyguardでロックは外れなかった。認証迂回は行わない。画面表示とDHU検証はロック解除後の段階。ssに5277の表示はなかったが、netlinkのPermission deniedもあり、サーバー停止の確証とは扱わない。

## ビルドと検証

- 最終Universal APKのビルドとlintVital成功（2m44s）。ARM64のみ、アプリID・ラベル・CarService登録をAPKから確認。
- 専用のRSA 3072署名鍵を使用。apksigner検証成功、証明書が自作鍵に一致し、公式Fermata APKとは異なることを確認。
- ローカルAPK: dist/komenotv-auto-2.0.2-probe.apk（154454349 bytes）。SHA-256: 071EF5B2E505BB3ED0D441B2A8851A8C94B1724A992AE98A32D6E30434558F51。
- Windowsビルド用変更はWhisperの2ファイルのみ: NDKホストOS参照、短くCPU別に分離した生成先、本体と共通のABI指定。車載/Web Javaの動作コードは未変更。詳細はKOMENOTV_AUTO.md。
- Whisper依存の上流の浮動参照は維持。今回の取得commit: whisper.cpp=0c7be696371069e0baa2515ed7aa08d5a5473aa6、Vulkan-Hpp=1a24b015830c116632a0723f3ccfd1f06009ce12、SPIRV-Headers=86f980c731e62ae4eaf383d320449d71687936bf。将来のバイナリ同一再現は未保証。
- dist/に最終ビルドログ、prototype-validation.json、prototype-device.json、署名/manifest/起動の証跡を保存。鍵・パスワード・APK・ログはGit対象外。

## 次の段階

帰宅後などに6aを認証解除し、スマホ上の起動と停車状態の800x480 DHUを検証する。実車チェックはユーザー担当。元Fermataが動作しているPixel 8で、導入元と開発者設定を確認したうえで別アプリの一覧表示・起動を先に判定する。
その判定後に車載用7100番の固定起動と、アプリ起動/復帰時の全画面化を実装する。既存KomenoTV（家庭用7000・車載7100）は変更不要。Android Autoのドックはアプリ領域の全画面とは別。
