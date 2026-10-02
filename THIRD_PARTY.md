# Nguồn bên ngoài

Skill chép từ repo khác được ghi ở đây: lấy từ đâu, tại commit nào, giấy phép gì, và đã sửa gì.
Bản chép là **ảnh chụp tại một commit** — bow-agent không tự kéo bản mới của nguồn. Muốn cập nhật
thì làm lại các bước ở cuối file, đọc lại nội dung, rồi mới gắn tag.

| Skill | Nguồn | Commit | Giấy phép | Đã sửa |
| --- | --- | --- | --- | --- |
| `react-native-best-practices` | [callstackincubator/agent-skills](https://github.com/callstackincubator/agent-skills) `skills/react-native-best-practices` | `61e6e7dfdf3a8ee862254c200d751fcb1fb863dc` (2026-09-16) | MIT | Bỏ thư mục `references/images/` (12 ảnh PNG, 6 448 759 byte), `POWER.md`, `agents/openai.yaml`; 30 tệp còn lại nguyên văn — kể cả 10 dòng nhúng ảnh `![…](images/….png)` trong 9 tệp, nay trỏ tới ảnh không còn |
| `react-navigation` | [callstackincubator/agent-skills](https://github.com/callstackincubator/agent-skills) `skills/react-navigation` | `61e6e7dfdf3a8ee862254c200d751fcb1fb863dc` (2026-09-16) | MIT | Bỏ `agents/openai.yaml`; còn lại nguyên văn |
| `upgrading-react-native` | [callstackincubator/agent-skills](https://github.com/callstackincubator/agent-skills) `skills/upgrading-react-native` | `61e6e7dfdf3a8ee862254c200d751fcb1fb863dc` (2026-09-16) | MIT | Bỏ `agents/openai.yaml`; còn lại nguyên văn |

Văn bản giấy phép: [`LICENSES/callstack-agent-skills-MIT.txt`](LICENSES/callstack-agent-skills-MIT.txt).

Lấy từ thư mục `skills/` ở gốc repo nguồn, không lấy bản trùng dưới `plugins/`. Commit trên là
`HEAD` của nguồn lúc chép (bản marketplace `1.2.0` theo `.claude-plugin/marketplace.json`).

## Vì sao bỏ những phần đó

- **`references/images/` của `react-native-best-practices`**: agent chỉ đọc chữ, còn bow chép cả
  thư mục skill vào `.claude/skills/` của TỪNG dự án — 12 ảnh làm skill nặng ~6,4 MB, bỏ đi còn
  ~260 KB. Phần chữ quanh mỗi ảnh đã tả nội dung ảnh (treemap, flame graph, sơ đồ…), nên các tệp
  `.md` được giữ nguyên văn, không sửa dòng nhúng ảnh. Cần xem ảnh thì mở repo nguồn.
- **`POWER.md` và `agents/openai.yaml`**: tệp cho nền tảng agent khác (Kiro, OpenAI). `POWER.md` là bản
  rút gọn trùng nội dung `SKILL.md` (8 KB, cùng `name`); `openai.yaml` là 4 dòng metadata hiển thị.
  Claude Code không dùng, `SKILL.md` không trỏ tới.
- Ba skill này không có `CHANGELOG`, tệp `_template` / `_contributing`, hay mục bảo agent gửi phản
  hồi / mở issue — nên không còn gì khác phải bỏ.

## Skill của nguồn KHÔNG lấy

`react-native-tv-best-practices`, `create-react-native-library`, `assess-react-native-migration`,
`react-native-brownfield-migration`, `github-actions`, `writing-user-docs` — ngoài phạm vi việc
thường ngày của team (TV, viết thư viện, chuyển app native sang RN, CI, viết tài liệu).

## Đã soát gì trước khi chép (2026-10-02)

Đã đọc hết 51 tệp của ba skill ở nguồn (48 `.md` + 3 `agents/openai.yaml`; `POWER.md` và ba tệp `.yaml` sau đó đã bỏ — xem trên).

- **Không có script / file thực thi.** Ngoài Markdown ở nguồn chỉ có `agents/openai.yaml` (4 dòng: tên hiển
  thị + câu gợi ý cho nền tảng agent khác) ở mỗi skill, và `POWER.md` của
  `react-native-best-practices` (bản rút gọn cùng nội dung với `SKILL.md`, có thêm `author` /
  `keywords`). Claude Code không đọc hai loại tệp này; giữ lại vì luật chép nguyên văn.
- **Không có ký tự vô hình** (tag Unicode, zero-width, bidi). Ký tự ngoài ASCII chỉ gồm mũi tên,
  nét kẻ khung, gạch dài và emoji ✅ ❌ ⚠️ (kèm U+FE0F — bộ chọn dạng emoji).
- **Không có `curl … | sh`**, và chính skill dặn "do not pipe remote install scripts directly into
  a shell". Không có chỉ dẫn gửi phản hồi, mở issue, hay gửi nội dung phiên làm việc ra ngoài.
- **Lệnh có chạm mạng** (agent chạy sẽ qua cổng duyệt của bow như mọi lệnh ghi khác):
  - `upgrading-react-native`: `curl -L` tải tệp diff và danh sách `RELEASES` của
    `react-native-community/rn-diff-purge` từ `raw.githubusercontent.com` về `/tmp` (dữ liệu để
    đọc, không thực thi); `npm view react-native dist-tags.latest`; và một URL để lấy tệp nhị phân
    `android/gradle/wrapper/gradle-wrapper.jar` của bản RN đích từ cùng repo đó.
  - Cài gói qua npm / npx, có chỗ không ghim phiên bản: `npx react-compiler-healthcheck@latest`,
    `npx create-react-native-library@latest`, `babel-plugin-react-compiler@beta` / `@latest`,
    `npx source-map-explorer`, `npx bundle-phobia-cli`, `npx sharp-cli`, `npx expo …`,
    `npm install <gói>` (reanimated, flash-list, bottom-sheet, jotai, zustand, `@callstack/inspector`…).
  - `code --install-extension wix.vscode-import-cost` (cài extension VS Code).
- **Lệnh `sudo`**: `sudo xcode-select -s …` và `sudo xcodebuild -runFirstLaunch` trong
  `upgrading-react-native/references/upgrading-react-native.md` (chỉ khi toolchain Xcode lệch).
- **Tài liệu từ xa mà skill bảo agent đọc và làm theo**: skill nâng cấp Expo ở
  `github.com/expo/skills` ("follow … as a primary guide", chỉ khi dự án có `expo`), và
  `oss.callstack.com/react-native-testing-library/llms.txt` (khi lên React 19). Nội dung hai trang
  đó KHÔNG nằm trong bản chép nên không được soát ở đây.
- **Công cụ / dịch vụ ngoài được nhắc tới nhưng không kèm theo**: CLI `agent-device` của Callstack
  (skill dặn "install it through the environment's approved/trusted path or ask the user"),
  Flashlight, Emerge Tools ("Upload IPA, APK, or AAB through their web interface" — đưa bản build
  lên dịch vụ bên thứ ba), bundlephobia.com, pkg-size.dev, ui.perfetto.dev.
- **Đường link** trỏ tới: `reactnavigation.org`, `reactnative.dev`, `react.dev`,
  `playground.react.dev`, `developer.android.com`, `cs.android.com`, `docs.expo.dev`,
  `shopify.github.io`, `react-native-community.github.io`, `reactnative.directory`,
  `bundlephobia.com`, `pkg-size.dev`, `ui.perfetto.dev`, `oss.callstack.com`,
  `raw.githubusercontent.com/react-native-community`, và `github.com` (`callstackincubator`,
  `gorhom`, `expo`, `facebook/react-native`, `react-native-community`, `XcodesOrg`, `AppAndFlow`).
- Ví dụ code có `analytics.track(...)` là code của APP minh hoạ (gửi số đo hiệu năng về hệ
  analytics của chính app), không phải chỉ dẫn cho agent.

## Cập nhật từ nguồn

```bash
git clone --depth 1 https://github.com/callstackincubator/agent-skills /tmp/up && cd /tmp/up && git rev-parse HEAD
# so từng skill với bản ở đây, ĐỌC phần khác biệt (đặc biệt lệnh / link / tệp mới), rồi chép đè
diff -r -x images /tmp/up/skills/react-native-best-practices skills/react-native-best-practices
diff -r /tmp/up/skills/react-navigation skills/react-navigation
diff -r /tmp/up/skills/upgrading-react-native skills/upgrading-react-native
# chép đè xong nhớ bỏ lại thư mục ảnh
rm -rf skills/react-native-best-practices/references/images
```
Sửa bảng ở trên (commit + ngày), tăng `version` trong `bow-skill.json`, gắn tag mới.
