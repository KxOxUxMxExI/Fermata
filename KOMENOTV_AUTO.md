# KomenoTV Auto 試作

Fermata 2.0.2 (65a3c163) の派生版。上流のGPL-3.0と著作権表示を保持する。

現在は車載ランチャー認識を調べるための版。名前は `KomenoTV Auto`、アプリIDは `me.aap.fermata.auto.komenotv`。起動先固定・自動全画面は、実車での別アプリ認識を確認した後の段階。
既存のFermataアプリ、家庭用7000番、車載Web版7100番の設定・データは変更しない。

## ビルド

- JDK 21、既存のGradle Wrapper 9.5.1、Android SDK 36、NDK 29.0.14206865。
- Windowsでは既存のVisual Studio 2022 C++ Developer PowerShellを有効にする。Vulkan用PC側プログラムにはMSVC、Android側にはNDKを使う。CMake 4.1.2とNinja 1.13.1を作業フォルダー内に配置。公式GitHubリリースのSHA-256を照合した。`local.properties` の `cmake.dir` で参照する。
- WhisperのCPU種類を本体のABI指定に合わせ、短い依存生成先をCPUごとに分離する。上流のWhisper用NDK参照がLinux固定だったため、NDKの `ANDROID_HOST_TAG` とホストの `glslc` 検索に変更。字幕生成モジュールを削除せず、車載/WebのJavaコードも変更しない。
- アプリ固有の署名鍵を `res/komenotv.jks` に生成し、署名設定はGit対象外の `local.properties` に保存する。上流の既定値から生成される共通鍵は試作の署名に使用しない。鍵とパスワードをコミットしない。

PowerShellで `JAVA_HOME` と `ANDROID_SDK_ROOT` を設定して実行:

```powershell
$env:NO_GS = 'true'
.\gradlew.bat --no-daemon :fermata:packageAutoReleaseUniversalApk '-PAPP_ID_SFX=.komenotv' '-PAPP_NAME=KomenoTV Auto' '-PABI=arm64-v8a' --console plain
```

## 判定

APKのアプリID・ラベル・署名を検証してから別アプリとして追加する。既存Fermataのアンインストールやデータ消去は不要。
ADBでの追加インストールだけでは、実車のAndroid Autoに表示されることは証明できない。導入元と開発者設定は別途確認し、スマホの認証が必要な操作をロック中に迂回しない。
PC/DHUの表示と実車の表示・入力・音声を区別する。実車での確認はユーザーが停車中に行う。
現在の進行は HANDOFF.md と tasks/todo.md を参照。APK・ログ・署名鍵はGit対象外。
