# CTA Spectator

Arma Reforger向けの観戦Modです。プレイヤー一覧から観戦先へカメラを移動でき、リスポーン回数の制限にも対応します。

## 主な機能

- プレイヤー一覧から観戦先へカメラを移動
- 観戦カメラ付近のローカルボイスを受信
- 観戦終了時に通常の音声受信へ復帰
- 個人別・共有チケット方式のリスポーン制限

## ミッションへの組み込み

1. ミッションのプロジェクトに`CTA_Spectator`を依存関係として追加します。
2. サーバーと全クライアントで、同じバージョンのModを読み込みます。
3. 使用するGameModeに`CTA_RespawnLimiterComponent`と`CTA_AlivePlayerComponent`があることを確認します。Mod内の`Prefabs/MP/Modes/GameMode_Base.et`に追加されています。
4. 手動で観戦へ入る場所には、`ActionsManagerComponent`のアクションとして`CTA_EnableSpectatorAction`を設定します。アクションの表示名とコンテキストも設定してください。

配置例は[`worlds/Test.ent`](worlds/Test.ent)と、その`Test_Layers`フォルダーを参照してください。  
テストワールドをそのまま本番ミッションとして利用することは想定していません。

## リスポーン制限

初期設定は`Unlimited`（無制限）です。設定値は現状、Workbenchのコンポーネントプロパティとして公開されていません。

| 方式 | 管理者チャットコマンド | 内容 |
|---|---|---|
| 個人別 | `#respawnType individual` | プレイヤーごとの死亡回数で制限 |
| 共有 | `#respawnType ticket` | 全プレイヤーの合計死亡回数で制限 |
| 無制限 | `#respawnType unlimited` | 死亡回数による観戦移行を無効化 |

回数の上限は`#respawnLimit 数値`で設定します。例えば、最初の死亡後から観戦させる場合は次の組み合わせです。

```text
#respawnType individual
#respawnLimit 0
```

回数の上限のみ、Scenario Frameworkの変数`respawnLimit`からも設定できます。変数がある場合は初期化時に読み込まれます。方式は別途設定してください。

## 動作上の注意

- 自動観戦は、制限を超えたプレイヤーが再スポーンした約1秒後に開始します。リスポーン自体を禁止する仕組みではありません。
- リスポーンする兵士・ロードアウト・場所はミッション側の設定に従います。観戦中の兵士を削除・非表示にする処理はありません。
- ヘリ搭乗対象や任務判定から観戦者を除外する処理は、組み込むミッション側でも確認してください。他Modの観戦判定との互換性は前提にしないでください。
- ローカルボイスの可聴範囲・音量・無線設定は変更しません。カメラ位置は0.5秒間隔でサーバーへ通知します。

## 作者

- [CatalpaBow](https://github.com/CatalpaBow)：CTA Spectatorの原作者。

## Contributors

- [CLY89](https://github.com/CLY89)：観戦カメラ位置でのローカルボイス受信機能、動作検証、READMEの整備。
