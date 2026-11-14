# Chat_UI

**轻量、零依赖的浏览器 LLM 聊天客户端。**  
**A lightweight, zero-dependency LLM chat client for your browser.**  
**ブラウザで使える、軽量・依存関係ゼロの LLM チャットクライアント。**

[简体中文](#简体中文) · [English](#english) · [日本語](#日本語)  
[最新版本 / Latest release / 最新リリース](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases/latest) · [问题反馈 / Issues / 不具合報告](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/issues)

| 界面语言 / Interface language / UI 言語 | V0.8 下载 / Download / ダウンロード |
| --- | --- |
| 简体中文 | [CN.zip](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases/download/V0.8/CN.zip) |
| English | [EN.zip](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases/download/V0.8/EN.zip) |
| 日本語 | [JP.zip](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases/download/V0.8/JP.zip) |

---

## 简体中文

Chat_UI 把日常 LLM 对话、多模态输入和参数调整放进一个轻量的浏览器界面。每个语言版本仅包含 **一个 HTML 文件和一个 `config.txt`**，V0.8 解压后合计约 **100 KB**。无需安装依赖或部署客户端后端，打开 HTML、载入配置，即可连接你选择的模型服务。

### 核心特性

| 特性 | 可以做什么 |
| --- | --- |
| 轻量便携 | 两个文件即可运行，适合随身携带或快速搭建聊天环境。 |
| 三类 API 格式 | 支持 OpenAI Chat Completions、Anthropic Messages 和 Gemini 流式内容生成格式，可连接采用相应格式的服务。 |
| 灵活的模型控制 | 配置系统提示词、输出长度、采样参数、思考强度与预算；可通过 `EXTRA_BODY` 补充请求字段。参数是否生效取决于服务和模型。 |
| 多模态与文件输入 | 发送文字和图片；文本文件可直接嵌入，其他文件可按服务能力选择原生文件块或上传解析。 |
| 多会话与并发 | 在多个对话间切换，也可在同一会话中并行发送；流式展示回复，并将返回的思考内容与正文分开显示。 |
| 可导入、导出的历史 | 将对话保存为文本文件，之后重新载入；采用每行一条 JSON 的格式，便于检查和编辑。 |
| 中英日界面 | 提供独立的中文、英文、日文版本，布局适配横屏与竖屏。 |

### 快速开始

1. **下载并解压。** 从上方选择语言包，例如 [V0.8 中文版 CN.zip](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases/download/V0.8/CN.zip)。进入解压后的文件夹，找到 `Chat_UI_v0.8_CN.html` 和 `config.txt`。请下载 Releases 中的语言包；GitHub 自动生成的 `Source code` 压缩包不包含运行文件。
2. **编辑配置。** 用文本编辑器打开 `config.txt`，填写 `BASE_URL`（服务地址）、`API_KEY`（密钥）和 `MODEL`（模型 ID），并确认 `API_FORMAT` 与服务一致。更换服务时，还需按其要求调整思考字段和其他参数。
3. **打开并载入。** 在浏览器中打开 HTML，点击顶部 **「载入配置」**，选择刚才保存的 `config.txt`。确认顶部显示预期的模型名称。
4. **发送第一条消息。** 输入问题并点击 **「发送」**。如需图片或文件，先点 **「附加文件」**；需要独立话题时，点 **「开启新对话」**。

> **历史不会自动保存。** 关闭或刷新页面前，请等待需要保留的回复完成，再点击「导出历史」。下次使用时先载入配置，再用「载入历史」恢复对话。

### 配置重点

以下是需要替换为你自己的服务信息的示意，**请在下载的完整 `config.txt` 中修改对应行**：

```ini
BASE_URL=https://your-api-endpoint.example/v1
API_KEY=your_api_key
MODEL=your_model_id
API_FORMAT=openai
```

| 配置项 | 用途 |
| --- | --- |
| `BASE_URL` / `API_KEY` / `MODEL` | 服务地址、访问密钥和模型 ID；以服务商提供的信息为准。 |
| `API_FORMAT` | 选择 `openai`、`anthropic` 或 `gemini`。`openai` 使用 Chat Completions 格式。 |
| `SYSTEM_PROMPT` | 设置助手的角色、风格或任务说明。 |
| `MAX_TOKENS` / `TEMPERATURE` / `TOP_P` 等 | 控制输出长度和采样；服务不接受的可选参数可留空，使其不随请求发送。 |
| `REASONING_EFFORT` / `THINKING_BUDGET` / `*_THINKING_STYLE` | 调整思考强度、预算及请求字段；可用组合依 API 格式和模型而异。 |
| `FILE_MODE` | `inline` 内嵌文本文件；`native` 发送原生文件块；`extract` 先上传解析，还需配置 `EXTRACT_API`（`moonshot` 或 `zhipu`）和 `EXTRACT_BASE_URL`。 |
| `EXTRA_BODY` | 用 JSON 补充服务所需的额外请求体字段。 |
| `MAX_HISTORY_BYTES` | 历史导出大小上限，默认 `1048576` 字节（1 MiB）；超过时会从最早的记录开始裁剪。 |

配置文件内有更多参数注释。**保存配置文件后，需要重新点击「载入配置」才会应用修改。**

### 常用操作与历史管理

| 想要做什么 | 操作 |
| --- | --- |
| 换行或快速发送 | 横屏时 `Enter` 发送、`Shift + Enter` 换行；竖屏时 `Enter` 换行，点击「发送」提交。 |
| 切换话题 | 点「开启新对话」，再通过顶部对话标题切换会话。 |
| 开关思考 | 使用输入区的「思考」开关；实际支持程度由模型和配置决定。 |
| 保存与继续聊天 | 点「导出历史」保存 `history_时间戳.txt`；下次载入配置后，点「载入历史」选择该文件。 |
| 编辑历史文本 | 先备份导出的文件，再编辑各行 JSON 的 `content`；保留每行 JSON 格式及会话标识等其他字段，之后重新载入。 |

- **载入历史会替换当前页面的会话。** 如果当前内容需要保留，请先导出。
- 历史导出保存对话文本，不会打包图片和原始二进制附件；需要再次让模型查看时，请重新附加原文件。
- 配置文件包含 API 密钥，请妥善保管；聊天请求会发送至你配置的服务，使用 `extract` 时文件也会上传至配置的解析服务。

### 常见问题

- **能离线聊天吗？** 客户端可在本地打开；生成回复仍需连接可访问的模型 API。你需要自行准备服务地址、密钥和模型。
- **配置已载入，为什么请求失败？** 先核对地址、密钥、模型 ID 和 API 格式，再查看错误信息。若提示参数不支持，可将对应可选项留空；若浏览器报跨域错误，需要服务端允许浏览器访问（CORS）。
- **为什么没有思考内容，或附件无法识别？** 并非所有模型都会返回思考文本，也并非所有端点都接受相同的思考字段和附件类型。检查 `*_THINKING_STYLE`、模型能力和 `FILE_MODE`；PDF 等非文本文件通常需要 `native` 或 `extract`。
- **历史怎么不完整？** 等待回复完成后再导出，并检查 `MAX_HISTORY_BYTES` 是否足够；需要长期保存时，定期保留独立备份。

[返回语言选择](#chat_ui) · [查看版本更新](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases) · [许可证](LICENSE)

---

## English

Chat_UI brings everyday LLM conversations, multimodal input, and model controls into a lightweight browser interface. Each language edition consists of **one HTML file and one `config.txt`**, totaling about **100 KB** unpacked in V0.8. No dependency installation or client backend deployment is needed: open the HTML file, load your configuration, and connect to your chosen model service.

### Features

| Feature | What you can do |
| --- | --- |
| Lightweight and portable | Run from two files, ready to carry with you or set up quickly. |
| Three API formats | Connect to services using OpenAI Chat Completions, Anthropic Messages, or Gemini streaming content generation formats. |
| Flexible model controls | Configure the system prompt, output length, sampling, thinking effort, and thinking budget; add request fields with `EXTRA_BODY`. Support depends on the service and model. |
| Multimodal and file input | Send text and images. Embed text files directly, or use native file blocks or upload-and-extract processing for other files where supported. |
| Multiple chats and concurrent requests | Switch between conversations or submit concurrent requests within a chat. Stream replies and view returned thinking separately from the answer. |
| Importable, exportable history | Save conversations to a text file and load them later. One JSON record per line makes the history easy to inspect and edit. |
| Chinese, English, and Japanese UI | Choose a separate language edition, with a layout that adapts to landscape and portrait screens. |

### Quick start

1. **Download and extract.** Choose a language package above, such as [V0.8 English EN.zip](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases/download/V0.8/EN.zip). Open the extracted folder and locate `Chat_UI_v0.8_EN.html` and `config.txt`. Use the language package under Releases; GitHub's automatically generated `Source code` archives do not include the application files.
2. **Edit the configuration.** Open `config.txt` in a text editor. Fill in `BASE_URL` (service URL), `API_KEY` (key), and `MODEL` (model ID), and check that `API_FORMAT` matches your service. When switching services, also adjust thinking fields and other parameters as required.
3. **Open and load.** Open the HTML file in your browser. Click **Load config** at the top and select your saved `config.txt`. Check that the expected model name appears at the top.
4. **Send your first message.** Type a prompt and click **Send**. Use **Attach files** to add an image or file, or **Start a new chat** for a separate topic.

> **History is not saved automatically.** Before closing or refreshing the page, wait for replies you want to keep to finish, then click **Export history**. Next time, load your configuration first, then use **Load history** to restore the conversation.

### Configuration essentials

Replace the following placeholders with your own service details by **editing the matching lines in the complete `config.txt` you downloaded**:

```ini
BASE_URL=https://your-api-endpoint.example/v1
API_KEY=your_api_key
MODEL=your_model_id
API_FORMAT=openai
```

| Setting | Purpose |
| --- | --- |
| `BASE_URL` / `API_KEY` / `MODEL` | Service URL, access key, and model ID, as specified by your provider. |
| `API_FORMAT` | Choose `openai`, `anthropic`, or `gemini`. The `openai` option uses the Chat Completions format. |
| `SYSTEM_PROMPT` | Set the assistant's role, style, or task instructions. |
| `MAX_TOKENS` / `TEMPERATURE` / `TOP_P`, etc. | Control output length and sampling. Leave unsupported optional settings blank to omit them from requests. |
| `REASONING_EFFORT` / `THINKING_BUDGET` / `*_THINKING_STYLE` | Configure thinking effort, budget, and request fields. Valid combinations depend on the API format and model. |
| `FILE_MODE` | `inline` embeds text files; `native` sends native file blocks; `extract` uploads and extracts text first, requiring `EXTRACT_API` (`moonshot` or `zhipu`) and `EXTRACT_BASE_URL`. |
| `EXTRA_BODY` | Add service-specific request-body fields as JSON. |
| `MAX_HISTORY_BYTES` | History export size limit. The default is `1048576` bytes (1 MiB); the oldest records are trimmed first when the limit is exceeded. |

More settings are documented inside the configuration file. **After saving changes, click Load config again to apply them.**

### Everyday use and history

| Task | Action |
| --- | --- |
| Send or insert a line break | In landscape, `Enter` sends and `Shift + Enter` inserts a line break. In portrait, `Enter` inserts a line break; click **Send** to submit. |
| Change topics | Click **Start a new chat**, then switch conversations using their titles at the top. |
| Toggle thinking | Use the **Thinking** switch beside the input. Actual support depends on the model and configuration. |
| Save and resume | Click **Export history** to save `history_timestamp.txt`. Next time, load the configuration, then choose that file with **Load history**. |
| Edit history text | Back up the exported file, then edit the `content` values in its JSON lines. Keep each line valid JSON and preserve other fields, including session identifiers, before loading it again. |

- **Loading history replaces the conversations currently on the page.** Export anything you want to keep first.
- History exports store conversation text, without bundling images or original binary attachments. Attach the original files again when the model needs to see them.
- Your configuration file contains your API key; keep it private. Chat requests go to the service you configure. With `extract`, files are also uploaded to your configured extraction service.

### FAQ

- **Can I chat offline?** The client can open locally, but generating replies requires a reachable model API. Supply your own service URL, key, and model.
- **The configuration loaded, but requests fail. What should I check?** Verify the URL, key, model ID, and API format, then read the error message. Leave unsupported optional parameters blank. Browser cross-origin errors require the service to allow browser access through CORS.
- **Why is there no thinking text, or why is an attachment rejected?** Models differ in whether they return thinking text, and endpoints accept different thinking fields and file types. Check `*_THINKING_STYLE`, the model's capabilities, and `FILE_MODE`. Non-text files such as PDFs generally need `native` or `extract`.
- **Why is my exported history incomplete?** Wait for replies to finish before exporting and check that `MAX_HISTORY_BYTES` is large enough. Keep separate backups for long-term storage.

[Back to language selection](#chat_ui) · [Release notes](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases) · [License](LICENSE)

---

## 日本語

Chat_UI は、日常の LLM チャット、マルチモーダル入力、モデルのパラメータ調整を軽量なブラウザ UI にまとめたクライアントです。各言語版は **HTML ファイル 1 つと `config.txt` 1 つ**で構成され、V0.8 の展開後の合計サイズは約 **100 KB**。依存パッケージのインストールやクライアント用バックエンドの構築は不要です。HTML を開いて設定を読み込むと、選択したモデルサービスに接続できます。

### 主な特長

| 特長 | できること |
| --- | --- |
| 軽量・ポータブル | 2 つのファイルで動作し、持ち運びや簡単なチャット環境の準備に適しています。 |
| 3 種類の API 形式 | OpenAI Chat Completions、Anthropic Messages、Gemini のストリーミング生成形式を使用するサービスに接続できます。 |
| 柔軟なモデル設定 | システムプロンプト、出力長、サンプリング、思考の強度・予算を設定し、`EXTRA_BODY` でリクエスト項目を追加できます。対応状況はサービスとモデルによって異なります。 |
| マルチモーダル・ファイル入力 | テキストや画像を送信できます。テキストファイルは直接埋め込み、その他のファイルはサービスの対応状況に応じてネイティブ形式で送信するか、アップロードしてテキストを抽出できます。 |
| 複数の会話・並行送信 | 会話を切り替えたり、同じ会話内で並行して送信したりできます。回答をストリーミング表示し、返された思考内容と回答本文を分けて表示します。 |
| 履歴の保存・読込 | 会話をテキストファイルに保存して、後から読み込めます。1 行に 1 件の JSON を記録するため、確認や編集も容易です。 |
| 中・英・日 UI | 中国語・英語・日本語の独立したバージョンを用意し、横画面・縦画面に対応しています。 |

### はじめ方

1. **ダウンロードして展開。** 上の一覧から言語別パッケージを選びます。日本語版は [V0.8 JP.zip](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases/download/V0.8/JP.zip) です。展開したフォルダ内の `Chat_UI_v0.8_JP.html` と `config.txt` を確認してください。Releases の言語別パッケージを使用してください。GitHub が自動生成する `Source code` アーカイブには実行用ファイルが含まれていません。
2. **設定を編集。** テキストエディタで `config.txt` を開き、`BASE_URL`（サービス URL）、`API_KEY`（キー）、`MODEL`（モデル ID）を入力します。`API_FORMAT` がサービスの形式と一致していることも確認してください。サービスを変更する場合は、思考関連の項目やその他のパラメータも必要に応じて調整します。
3. **開いて設定を読込。** HTML ファイルをブラウザで開き、上部の **「設定読込」**から保存した `config.txt` を選択します。上部に目的のモデル名が表示されることを確認してください。
4. **最初のメッセージを送信。** 質問を入力して **「送信」**をクリックします。画像やファイルは **「ファイル添付」**から追加し、別の話題は **「新しいチャットを開始」**で始められます。

> **履歴は自動保存されません。** ページを閉じたり再読み込みしたりする前に、保存したい回答の生成完了を待って「履歴保存」をクリックしてください。次回は設定を読み込んだ後、「履歴読込」で会話を復元できます。

### 設定のポイント

以下は書き換える項目の例です。自分のサービス情報を使って、**ダウンロードした完全な `config.txt` の該当行を編集してください**。

```ini
BASE_URL=https://your-api-endpoint.example/v1
API_KEY=your_api_key
MODEL=your_model_id
API_FORMAT=openai
```

| 設定項目 | 用途 |
| --- | --- |
| `BASE_URL` / `API_KEY` / `MODEL` | サービス URL、アクセスキー、モデル ID。提供元の指定に従ってください。 |
| `API_FORMAT` | `openai`、`anthropic`、`gemini` から選択。`openai` は Chat Completions 形式を使用します。 |
| `SYSTEM_PROMPT` | アシスタントの役割、文体、タスクの指示を設定します。 |
| `MAX_TOKENS` / `TEMPERATURE` / `TOP_P` など | 出力長とサンプリングを調整します。未対応の任意項目は空欄にするとリクエストに含まれません。 |
| `REASONING_EFFORT` / `THINKING_BUDGET` / `*_THINKING_STYLE` | 思考の強度・予算・リクエスト項目を設定します。有効な組み合わせは API 形式とモデルによって異なります。 |
| `FILE_MODE` | `inline` はテキストを埋め込み、`native` はネイティブ形式のファイルブロックを送信。`extract` はアップロード後にテキストを抽出するため、`EXTRACT_API`（`moonshot` または `zhipu`）と `EXTRACT_BASE_URL` の設定も必要です。 |
| `EXTRA_BODY` | サービス固有のリクエストボディ項目を JSON で追加します。 |
| `MAX_HISTORY_BYTES` | 履歴の書き出しサイズ上限。初期値は `1048576` バイト（1 MiB）で、超過すると古い記録から削られます。 |

その他の項目は設定ファイル内のコメントを参照してください。**変更を保存した後は、もう一度「設定読込」をクリックして反映してください。**

### 基本操作と履歴の管理

| やりたいこと | 操作 |
| --- | --- |
| 送信・改行 | 横画面では `Enter` で送信、`Shift + Enter` で改行。縦画面では `Enter` で改行し、「送信」をクリックして送ります。 |
| 話題を切り替える | 「新しいチャットを開始」をクリック。上部の会話タイトルから会話を切り替えられます。 |
| 思考を切り替える | 入力欄の「思考」スイッチを使います。実際の対応状況はモデルと設定によって異なります。 |
| 保存・再開 | 「履歴保存」で `history_タイムスタンプ.txt` を保存。次回は設定を読み込んだ後、「履歴読込」でそのファイルを選択します。 |
| 履歴のテキストを編集する | 書き出したファイルをバックアップしてから、各行の JSON の `content` を編集します。各行の JSON 形式と、会話 ID など他の項目を維持してから再読込してください。 |

- **履歴を読み込むと、ページ上の会話は置き換わります。** 残したい内容は先に保存してください。
- 履歴には会話テキストが保存され、画像や元のバイナリ添付ファイルは同梱されません。モデルに再度見せたい場合は、元のファイルを添付し直してください。
- 設定ファイルには API キーが含まれるため、適切に保管してください。チャットのリクエストは設定したサービスに送信され、`extract` 使用時はファイルも設定した解析サービスにアップロードされます。

### よくある質問

- **オフラインで使えますか？** クライアントはローカルで開けますが、回答の生成には接続可能なモデル API が必要です。サービス URL、キー、モデルを自分で用意してください。
- **設定は読み込めたのに、リクエストが失敗します。** URL、キー、モデル ID、API 形式を確認し、エラーメッセージを参照してください。未対応の任意パラメータは空欄にできます。ブラウザのクロスオリジンエラーが出る場合は、サービス側が CORS によるブラウザアクセスを許可する必要があります。
- **思考内容が出ない、添付ファイルが認識されないのはなぜですか？** 思考テキストの出力、思考関連の項目、対応ファイル形式はモデルやエンドポイントによって異なります。`*_THINKING_STYLE`、モデルの機能、`FILE_MODE` を確認してください。PDF などの非テキストファイルには通常 `native` または `extract` が必要です。
- **保存した履歴が途中までしかありません。** 回答の生成完了後に保存し、`MAX_HISTORY_BYTES` が十分か確認してください。長期保存には定期的な別ファイルへのバックアップをおすすめします。

[言語選択に戻る](#chat_ui) · [リリースノート](https://github.com/KODAMA-TECHNOLOGY/Chat_UI/releases) · [ライセンス](LICENSE)
