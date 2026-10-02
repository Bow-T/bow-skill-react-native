# bow-skill-react-native

Bộ skill cho công nghệ **React Native**, dùng với [bow-agent](https://github.com/Bow-T/bow-agent).
Theo hướng **mỗi công nghệ một repo**: bộ này chỉ lo phần React Native; dự án dùng Supabase thì tick
thêm bộ [`supabase`](https://github.com/Bow-T/bow-skill-supabase) ở ô **Công nghệ**.

## Cấu trúc

```
bow-skill-react-native/
├── bow-skill.json        # manifest: id "react-native" + quy tắc tự nhận diện + `needs`
├── THIRD_PARTY.md        # skill nào chép từ đâu, commit nào, giấy phép gì, đã sửa gì
├── LICENSES/             # văn bản giấy phép của nguồn bên ngoài
└── skills/
    ├── rn-convention/                 # team — cấu trúc thư mục, đặt tên, ranh giới UI ⟷ data
    ├── rn-supabase-data/              # team — lớp data Supabase            (cần bộ `supabase`)
    ├── rn-realtime/                   # team — realtime Supabase, không leak (cần bộ `supabase`)
    ├── react-native-best-practices/   # ⬇ Callstack — hiệu năng: FPS, TTI, bundle, bộ nhớ (29 tệp tham khảo)
    ├── react-navigation/              # ⬇ Callstack — React Navigation 7: stack, tab, drawer, sheet, safe area
    └── upgrading-react-native/        # ⬇ Callstack — nâng phiên bản RN theo Upgrade Helper
```

| Skill | Dùng khi | Nguồn |
| --- | --- | --- |
| `rn-convention` | tạo màn hình / component / hook mới; soát code có theo quy ước team không | team Bow |
| `rn-supabase-data` | thêm / sửa truy vấn hoặc mutation Supabase trong app RN | team Bow |
| `rn-realtime` | thêm / sửa subscription realtime, badge / count sống | team Bow |
| `react-native-best-practices` | app giật, khởi động chậm, bundle to, rò bộ nhớ; viết native module; soát hiệu năng | Callstack (MIT) |
| `react-navigation` | dựng điều hướng bằng React Navigation 7, chỉnh header, xử lý safe area | Callstack (MIT) |
| `upgrading-react-native` | nâng phiên bản React Native (diff template, dependency, cấu hình iOS / Android) | Callstack (MIT) |

Ba skill của Callstack chép nguyên văn; riêng `react-native-best-practices` đã bỏ thư mục ảnh
(chi tiết ở [`THIRD_PARTY.md`](THIRD_PARTY.md)). Ba skill của team giữ nguyên từ v1.

### Skill chỉ trải khi dự án có cả Supabase (`needs`)

```json
"needs": { "rn-supabase-data": ["supabase"], "rn-realtime": ["supabase"] }
```

`needs` nghĩa là: skill đó chỉ được trải vào `.claude/skills/` khi dự án tick **CẢ** bộ ghi trong
danh sách (ở đây là `supabase`). Dự án React Native gọi REST API riêng chỉ nhận 4 skill còn lại,
khỏi tốn token cho mô tả skill không dùng tới. Skill không có trong `needs` thì luôn được trải.

`rn-realtime` nhắc tới `[[flutter-realtime]]` (bộ `flutter`) như bản gốc để đối chiếu — không có bộ
đó thì skill vẫn dùng được độc lập.

### Không lấy gì, vì sao

Cố ý KHÔNG lấy các skill còn lại của Callstack (`react-native-tv-best-practices`,
`create-react-native-library`, `assess-react-native-migration`, `react-native-brownfield-migration`,
`github-actions`, `writing-user-docs`) và bộ skill của Expo (`expo/skills`) — quy ước của team không
dùng Expo. Dự án nào dùng Expo thì thêm sau.

Skill Callstack vẫn có ví dụ dùng gói Expo (`@expo/vector-icons`, `expo-blur`, `npx expo …`) và
nhánh riêng cho dự án Expo; dự án theo quy ước team thì đi nhánh React Native CLI mà chính các skill
đó ghi kèm. **Khi hai nguồn nói khác nhau, quy ước team thắng.**

## Tự nhận diện

bow-agent tự tick bộ này cho dự án chưa khai công nghệ khi repo khớp quy tắc ở `targets.detect`:

| Quy tắc | Nghĩa |
| --- | --- |
| `package.json` chứa `"react-native"` (kể cả dấu nháy) | app có khai gói `react-native` (tìm cả trong thư mục con, vd `apps/mobile/package.json`) |

Chỉ soi file git đã track. Dấu nháy đóng nằm trong chuỗi so khớp nên `"react-native-web"` hay
`"react-native-svg"` đứng một mình KHÔNG khớp. App Expo cũng khai `react-native` nên vẫn khớp.

## Đổi gì so với v1

- **id đổi**: v1 là `react-native-supabase` ("React Native + Supabase"); v2 là `react-native`.
- **Thêm 3 skill của Callstack**: `react-native-best-practices`, `react-navigation`,
  `upgrading-react-native`.
- **Skill Supabase dùng chung** (`supabase`, `supabase-postgres-best-practices`, `supabase-migration`,
  `supabase-security-review`) nằm ở [bow-skill-supabase](https://github.com/Bow-T/bow-skill-supabase),
  không nằm ở đây. Hai skill RN gắn với Supabase (`rn-supabase-data`, `rn-realtime`) vẫn ở repo
  này nhưng chỉ trải khi dự án tick cả `supabase` (mục `needs` ở trên).
- `targets.detect` chỉ còn một quy tắc (`package.json` chứa `"react-native"`); bỏ quy tắc `app.json`,
  thư mục `ios` và `monorepoHint` của v1.

## Đăng ký vào bow-agent

Thêm vào `stacks` của `~/.bow-agent/registry.json` (bow chỉ tải repo có trong danh sách này, đúng tag đã ghim):

```json
{ "id": "react-native", "label": "React Native", "repo": "github.com/Bow-T/bow-skill-react-native", "ref": "v2.0.0" }
```

Vì id đã đổi, mục cũ `react-native-supabase` (ref `v1.0.0`) trong registry cần thay bằng mục trên;
dự án đang chọn id cũ thì chọn lại `react-native` (kèm `supabase` nếu dùng).

Thử local trước khi đẩy GitHub: `"repo": "file:///đường/dẫn/tới/bow-skill-react-native"`.

## Phát hành phiên bản mới

Sửa skill qua PR → tăng `version` trong `bow-skill.json` → gắn tag (`git tag v2.0.1 && git push --tags`)
→ cập nhật `ref` trong registry. bow tải theo tag và cache theo `<id>@<ref>`, nên sửa nội dung mà giữ
nguyên tag sẽ KHÔNG được tải lại. Cập nhật skill từ nguồn bên ngoài: xem cuối `THIRD_PARTY.md`.
