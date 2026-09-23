# オンラインサロン基盤：API・Bot比較調査

調査基準日：2026年9月23日

## 結論

推奨構成は、**独立した会員・決済・権限基盤を正本とし、Discordをコア会話層、LINE公式アカウント（OA）＋Messaging APIを告知・CRM層に置く方式**です。会員ID、契約状態、返金・解約、コンテンツ閲覧権、同意履歴は自社の会員DBで一元管理し、決済事業者の署名付きWebhookで権限を更新します。その結果をDiscordロール、会員サイト、LINE OAの案内へ投影します。

**LINE OpenChatは、Messaging APIで自由にBot管理できる対象ではありません。** OpenChatは匿名プロフィールを使う交流層として扱い、内蔵の自動応答・スパム対策・投票・ノートを利用します。OpenChatの入退室、投稿、権限、決済を外部Botで完全同期する設計は採用しません。自動化が必要なLINE機能は、LINE OAのMessaging APIを対象にします。[2] [3] [4]

**Spotifyは音楽発見・公式リンク・許容範囲のプレイリスト紹介に限定します。** 会員限定の楽曲配信、同期再生、イベントBGMの再送信、再生Bot、会費で販売する再生機能には使いません。[15] [16]

InstagramとXは集客・問い合わせの入口です。決済、会員認可、限定コンテンツの正本にはしません。公式API、審査、同意、レート制限、Bot表示、オプトアウトを前提にします。[11] [12] [13]

## サービス比較

| サービス | API・Bot | サロンでの役割 | 重要な制約 | 判断 |
|---|---|---|---|---|
| `bonsai/research-community` | 現状はMarkdown素材集。API、Bot、DB、認証、Webhook、CI/CDは未実装 | ブランド、企画、Podcast、KPI仮説の保管 | 技術基盤ではない。ライセンスと権利条件も未確定 | 企画資産として再利用 |
| Discord | Bot、OAuth2、Gateway、Interactions、Webhook、ロール、AutoMod | コア会話、講座別チャンネル、Q&A、イベント、権限 | Privileged Intent、権限階層、レート制限、データ保護 | 第一推奨 |
| LINE OA + Messaging API | Webhook、reply/push、リッチメニュー、アカウントリンク、Insights | 告知、予約、リマインド、個別案内、CRM連携 | OpenChat APIではない。通数、同意、署名検証が必要 | 第二推奨 |
| LINE OpenChat | 内蔵Bot、投票、ノート、イベント、スパム対策 | 任意参加の交流、匿名性のある雑談 | 外部Webhook、自由なBot認証、決済連動、外部投稿収集を前提にできない | 補助層 |
| Slack | App、Bot、Events API、Web API、Webhook、Block Kit、SCIM | 職能・B2B寄りの会話、告知、Q&A | ワークスペース入会、Marketplace、SCIM、履歴API制限 | 対象顧客がSlack常用なら候補 |
| Instagram | 投稿、DM、Webhook、コメント、Insights | 認知、問い合わせ、短いオンボーディング | Professional account、App Review、Advanced Access、24時間DM窓 | 入口のみ |
| X | Posts、DM、OAuth、Webhook、Activity API | 公開告知、問い合わせ、ソーシャルリスニング | 開発者申請、従量課金、Bot表示、スパム規制、opt-out | 入口のみ。苦手なら無理に使わない |
| Spotify | メタデータ、検索、プレイリスト、ユーザー同意下の再生操作 | プレイリスト紹介、公式リンク、音楽発見 | Premium、Development Mode、商用ストリーミング・同期再生制限 | 非中核 |

## 推奨アーキテクチャ

```text
Instagram / Threads / X / Podcast / Spotify公式リンク
                         ↓
          会員サイト・説明会・申込ページ
                         ↓
             認証・同意・外部決済
                         ↓
       会員DB（契約・権限・同意の唯一の正本）
                 ┌───────┼────────┐
                 ↓       ↓        ↓
          Discordロール  LINE OA  限定コンテンツ
          チャンネル     通知      会員サイト
```

会員DBには、内部会員ID、契約プラン、契約状態、有効期限、権限セット、決済イベントID、外部ID対応、同意の版と取得日時、撤回日時を保存します。LINE ID、Discord ID、SNS IDは任意連携の外部識別子として最小限に保持します。

決済Webhook、Discordイベント、LINE Webhookは、生のリクエスト本文で署名を検証します。受信後は短時間で成功応答し、キューへ移します。イベントIDで重複を除外し、失敗したロール更新や通知を再試行できるようにします。

### 層ごとの責任

- **会員・決済・権限**：会員DBと決済事業者。契約、返金、解約、閲覧権、同意の正本。
- **コアコミュニティ**：Discord。会員ロール、限定チャンネル、フォーラム、Q&A、イベント、AutoMod。
- **告知・CRM**：LINE OA。友だち追加、リッチメニュー、予約、リマインド、個別案内。
- **コンテンツ**：認証付き会員サイト。記事、ZINE、テンプレート、録画、イベント資料。
- **集客**：Instagram、Threads、X、Podcast、Spotify公式リンク。
- **分析**：同意を反映した自前イベント基盤。会話本文を無差別に収集しない。
- **モデレーション**：自動検知と人間の最終判断を分離する。

## LINE OpenChatとLINE OAの分担

| 層 | 使い方 | 自動化 | しないこと |
|---|---|---|---|
| LINE OA | 告知、予約、個別問い合わせ、会員サイトへの導線 | Messaging APIで可能 | 未同意の販促、過剰Push、OpenChatとの混同 |
| OpenChat | 会員交流、雑談、投票、ノート、イベント | 内蔵Bot・管理者運用 | 外部決済からの自動入室、外部投稿収集、CRM同期を必須化 |
| OA Membership | 月額会費と、許容される加入者特典 | 加入・更新・退会Webhook | 外部決済でOpenChatを任意制御できると解釈しない |

OpenChatを利用する場合は、参加コードまたは質問回答＋管理者承認を使います。OpenChatプロフィールはLINE本体アカウントと別の匿名プロフィールであることを説明します。会員サイトの契約状態とOpenChatの入室状態を同一視しません。

## 各サービスの実装判断

### Discord

Discordは会員サイト・決済と連携し、Botが会員ロールを付与・剥奪する構成に向きます。OAuth2でユーザーIDを連携し、決済Webhookを会員DBへ反映し、Botがロールを同期します。Slash Command、ボタン、モーダル、Webhook、Scheduled Event、AutoModを使えます。

注意点は、MESSAGE_CONTENTなどのPrivileged Intent、Botのロール階層、RESTのレート制限、Webhookの即時応答、Discordデータのプライバシーです。会話内容をLLM学習や広告ターゲティングへ使わず、必要最小限だけ保存します。[5] [6] [7]

### Slack

Slackは職能・B2B寄りのコミュニティに向きます。Events API、Block Kit、モーダル、Socket Mode、SCIMを利用できます。ただし、一般Bot APIだけで外部メールからワークスペース入会を完全自動化できません。Business+またはEnterpriseのSCIM、管理者操作、招待リンクなどが必要になる場合があります。

複数顧客向けにアプリを配布する場合はMarketplaceや配布条件を確認します。一般的な履歴APIの取得制限があるため、会話を後から大量取得して分析する設計は避け、リアルタイムイベントと必要最小限の自前ログを使います。[8] [9] [10]

### Instagram

InstagramはProfessional account向けの公式APIを使います。投稿公開、DM、コメントWebhook、Private Reply、コメントモデレーション、Insightsを利用できます。ただし、DMは原則ユーザーが先に会話を開始した後の24時間窓です。デジタル商品をDM内で販売したり、決済情報を聞いたり、定期一斉DMを送ったりする設計は不適です。

自社アカウントだけならStandard Accessで開始できる場合がありますが、第三者アカウントを扱うサービスはAdvanced Access、App Review、Business Verificationが必要になり得ます。会員登録、継続決済、限定コンテンツ、ロールは外部基盤で管理します。[11] [12] [13]

### X

X APIは投稿、DM、OAuth、Webhook、Activity APIを提供します。公式API以外のブラウザ自動化、スクレイピング、未承諾の大量DM・返信・メンション、重複投稿、エンゲージメント水増しは禁止対象です。自動アカウントには人間運用アカウントとの接続とBot表示が必要です。

開発者申請、利用目的の拘束、従量課金、レート制限、明示同意、即時opt-outを前提にします。Xが苦手なら、主戦場にせずInstagram、Threads、Podcast、メール、LINEを優先します。[14] [15] [16]

### Spotify

Spotify Web APIはメタデータ、検索、ユーザー同意下のプレイリスト操作、再生状態操作を提供します。しかしDevelopment Modeの規模制限、Premium要件、Extended Quotaの高い審査条件があります。また、会費付きのストリーミング、広告・スポンサーを伴う再生機能、音源の再送信、映像同期、ミックス、人工的な再生増加は行いません。

サロンでは、運営が作ったプレイリストの紹介、Spotify公式リンク、利用者が自分のPremiumアカウントで聴く任意連携に限定します。[17] [18] [19]

## リポジトリの評価

`bonsai/research-community`は、ブランド、コミュニティ、Podcast、SNS、収益化の企画素材としては再利用価値があります。一方、現状はMarkdown文書のみで、API、Bot、データベース、認証、決済、Webhook、テスト、CI/CD、監視はありません。技術基盤の流用ではなく、要件・コンテンツの起点として扱います。

特に再利用できる資産は、次のとおりです。

- 「自分の仕事を1つつくる7日間」という初期コミュニティ企画。
- `@oshare.dorobous`を人・服・ダンスの入口にする導線。
- `@kanalsound`をAI、音楽イベント、企画と仕事化の中心にする構成。
- `@pins.zine`を旅、地域、暮らし、ビジネスの実験媒体にする構成。
- Podcastをイベント、AI、個人事業、クラフト、配達、旅の記録にする設計。
- 再生数だけでなく、登録、予約、来場、再参加、案件化を測るKPI思想。

## Bot運用チェックリスト

- [ ] 会員資格の正本をSNSやチャットに置かない。
- [ ] OAuth、Webhook、決済通知の署名検証を実装する。
- [ ] 外部ID、同意、撤回、削除、保存期間を管理する。
- [ ] Botの送信頻度、重複排除、停止スイッチを設ける。
- [ ] 広告、紹介、協賛、提供を明示する。
- [ ] 収入、不労所得、配達報酬を保証しない。
- [ ] Instagram DMはユーザー起点の範囲に限定する。
- [ ] Xは公式APIのみを使用し、Bot表示とopt-outを用意する。
- [ ] LINE OAとOpenChatを混同しない。
- [ ] Spotifyの音源再送信、同期再生、再生Botをしない。
- [ ] 参加者の写真、音声、動画、個人情報、未公開案件をAIへ無断入力しない。
- [ ] モデレーションの自動措置に人間の異議申立て窓口を設ける。
- [ ] 退会・返金・解約後のアクセス失効をテストする。

## ロードマップ

### 0. 要件確定

対象会員、価格、提供地域、年齢、契約状態、返金、限定コンテンツ、必要な自動化を定義する。OpenChatの自由なBot管理とSpotifyの会員限定再生を要件から外す。

### 1. 手動PoC

無料コンテンツ、Podcast、説明会、小規模イベント、手動運営のDiscordまたはOpenChat、LINE OAの静的導線で価値を検証する。

### 2. クローズドβ

会員サイト、認証、同意台帳、決済テスト、会員DB、Discord OAuth、ロール同期、LINE OAの限定通知、監査ログを実装する。

### 3. 限定本番

利用規約、プライバシーポリシー、特商法表示、行動規範、通報・削除手順、障害対応、バックアップ、監視、運営当番を整える。

### 4. 選択的自動化

需要が確認できた場合だけ、InstagramのDM・コメント支援、Xの投稿・問い合わせ支援、LINE OA Membership、Slack連携を追加する。各社の審査、費用、プラン、規約を個別に確認する。

## 参考文献

[1]: https://github.com/bonsai/research-community "bonsai/research-community — GitHub repository"
[2]: https://developers.line.biz/en/docs/messaging-api/overview/ "LINE Developers — Messaging API overview"
[3]: https://developers.line.biz/en/docs/messaging-api/use-membership-features/ "LINE Developers — Use membership features"
[4]: https://openchat.line.me/jp/guide "LINE OpenChat — Official guide"
[5]: https://discord.com/developers/docs/quick-start/getting-started "Discord Developer Docs — Getting started"
[6]: https://discord.com/developers/docs/events/gateway "Discord Developer Docs — Gateway and intents"
[7]: https://support-dev.discord.com/hc/en-us/articles/8563934450327-Discord-Developer-Policy "Discord Developer Policy"
[8]: https://docs.slack.dev/apis/events-api/ "Slack Developer Docs — Events API"
[9]: https://docs.slack.dev/admins/scim-api/ "Slack Developer Docs — SCIM API"
[10]: https://slack.com/terms-of-service/api "Slack API Terms of Service"
[11]: https://developers.facebook.com/documentation/instagram-platform/overview "Meta for Developers — Instagram Platform overview"
[12]: https://developers.facebook.com/documentation/business-messaging/instagram-messaging/overview "Meta for Developers — Instagram Messaging overview"
[13]: https://developers.facebook.com/documentation/instagram-platform/app-review "Meta for Developers — Instagram App Review"
[14]: https://docs.x.com/x-api/introduction "X Developer Platform — X API introduction"
[15]: https://docs.x.com/x-api/getting-started/pricing "X Developer Platform — API pricing"
[16]: https://help.x.com/en/rules-and-policies/x-automation "X Help — Automation rules"
[17]: https://developer.spotify.com/documentation/web-api "Spotify for Developers — Web API"
[18]: https://developer.spotify.com/documentation/web-api/concepts/quota-modes "Spotify for Developers — Web API quota modes"
[19]: https://developer.spotify.com/policy "Spotify for Developers — Developer Policy"
