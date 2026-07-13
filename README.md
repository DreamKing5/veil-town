# VEIL TOWN — Supabase Realtime修正版

招待制・4〜12人用のブラウザ型ソーシャル推理ゲームです。スマホ、PC、iPadから各プレイヤーが自分の端末で参加し、役職配布・昼夜進行・能力処理・投票・勝敗判定を自動で行います。

## 接続方式を変更した理由

初版のPeerJSは端末同士を直接つなぐP2P方式でした。異なるWi-Fi、携帯回線、学校・集合住宅の回線などでは、NATやファイアウォールの種類によって接続できない場合があります。

修正版では通信を **Supabase Realtime** に変更しました。ブラウザ同士を直接つながないため、異なるネットワーク間でも参加できます。

## 完全無料構成

- 公開: GitHub Pages または個人利用のVercel Hobby
- リアルタイム通信: Supabase Realtime Free
- データベース: 不要
- SQL設定: 不要
- 専用ゲームサーバー: 不要
- 月額費用: 無料枠内なら0円

Supabase無料プロジェクトは長期間アクセスがないと一時停止することがあります。遊ぶ前にSupabase Dashboardを開き、プロジェクトが稼働中か確認してください。

## 最初の1回だけ必要な設定

1. https://supabase.com で無料プロジェクトを1つ作る。
2. Supabase Dashboardの `Project Settings → API` を開く。
3. `Project URL` と `anon / publishable key` を確認する。
4. Dashboardの `Realtime → Settings` を開く。
5. **Allow public access** をONにする。今回の招待制チャンネルはpublic channelとして動作します。
6. 公開した `index.html` を開き、先に「Supabase接続設定」を押す。
7. 画面内の設定フォームを開いたまま、Supabase Dashboardへ切り替える。
8. Project URLをコピーして戻り、1つ目の欄へ貼り付ける。
9. 再びDashboardへ切り替え、anon / publishable keyをコピーして戻る。
10. 2つ目の欄へ貼り付け、「保存して続ける」を押す。
11. 以後は同じブラウザへ設定が保存される。

設定はブラウザの警告ダイアログではなく、閉じない画面内フォームで入力します。別タブ・別アプリへ切り替えても入力内容は消えません。

`anon / publishable key` はブラウザ向けの公開キーです。service_role keyは絶対に入力しないでください。

## 参加方法

1. 作成者が「新しい部屋を作る」を押す。
2. 接続表示が「部屋を公開中」になるまで待つ。表示されない場合はSupabaseプロジェクトの稼働状態とAllow public accessを確認する。
3. 「招待URLをコピー」を押す。
4. URL全体を参加者へ送る。
5. 参加者はURLを開き、名前を入力して「参加する」を押す。
6. 「接続済み」と表示され、作成者のプレイヤー一覧へ名前が追加されれば成功。

招待URLには、ルームID・秘密キー・Supabaseの公開接続設定が含まれます。URLを途中で省略せず、`#`以降も含めて共有してください。

## 公開方法（GitHub Pages）

1. GitHubで新しいリポジトリを作る。
2. `index.html` をアップロードする。
3. `Settings → Pages → Deploy from a branch` を選ぶ。
4. `main / root` を指定して保存する。
5. 発行されたHTTPS URLを開く。

## 公開方法（Vercel）

1. Vercelで `Add New → Project` を開く。
2. このフォルダを含むGitHubリポジトリを選ぶ。
3. Framework Presetは `Other`、Build Commandは空欄のまま公開する。

## 収録役職

医師、錯乱者、警官、見張り、調査員、罠師、密告者、挑発者、追跡者、清掃屋、濡れ衣師、生存者、連続殺人犯、爆弾魔、怪盗、亡霊、魔術師。

## 注意点

- ゲーム状態は部屋作成者のブラウザメモリにあります。作成者がページを閉じると、そのゲームは終了します。
- ゲーム中は作成者の画面をスリープさせないでください。
- 招待URLを転送された人は参加できるため、参加者以外へ共有しないでください。
- 本家の画像、音声、ロゴ、文章、画面素材は含んでいません。
- 本番前にiPhone Safari、Android Chrome、iPad Safari、PC Chromeで接続確認してください。

## 修正内容

- PeerJS P2P通信を廃止
- Supabase Realtime Broadcastへ変更
- NAT・異なるWi-Fi・携帯回線を越えた接続に対応
- 招待URLへ公開接続設定を安全に同梱
- プレイヤーごとの秘密役職は個別の秘密チャンネルで送信
- 不正なプレイヤーIDの操作を参加トークンで拒否
- public channelを明示し、サーバー受信確認（ack）を有効化
- 参加通知を2秒ごとに再送し、回線遅延や初回取りこぼしに対応
- ロビー状態を3秒ごとに再同期
- 接続失敗時にAllow public accessの確認を表示
