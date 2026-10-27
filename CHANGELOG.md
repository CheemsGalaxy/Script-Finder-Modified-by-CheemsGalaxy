# Changelog

> Entries are kept as written when released, so each one describes the code at
> that moment. Where a later release replaced an earlier approach — for example
> the `cdn.githubraw.com` update flow added in `2026.10.27.1` — the newer entry
> is authoritative and the older text is left untouched as history.
>
> Mỗi entry giữ nguyên như lúc phát hành, nên nó mô tả code ở đúng thời điểm đó.
> Khi bản sau thay đổi cách làm cũ — ví dụ luồng cập nhật `cdn.githubraw.com`
> thêm ở `2026.10.27.1` — entry mới hơn là bản có hiệu lực, entry cũ được giữ
> lại như lịch sử.

## 2026.10.27.5 — 2026-10-27

### Fixed / Sửa lỗi

**English**

- Light-theme muted text (`--muted`) was `#737373`. It reached 4.74:1 on the
  page background but only 4.31:1 on the `--hover` surface used by inactive
  segmented buttons and the close button — below WCAG AA. Darkened to `#6b6b6b`:
  4.85:1 on `--hover`, 5.11:1 on `--card`, 5.33:1 on `--bg`, so every muted
  label now passes AA in both themes.
- `tools/csslint.js` no longer compares against a hard-coded colour list. It
  parses the `--*` tokens out of the light and dark `:host` blocks and checks
  seven foreground/background pairs per theme, so a future palette edit can no
  longer reintroduce this kind of defect unnoticed.

**Tiếng Việt**

- Chữ phụ ở theme sáng (`--muted`) là `#737373`: đạt 4.74:1 trên nền trang
  nhưng chỉ 4.31:1 trên nền `--hover` của segmented button (trạng thái không
  chọn) và nút đóng — dưới ngưỡng WCAG AA. Đã tối lại thành `#6b6b6b`: 4.85:1
  trên `--hover`, 5.11:1 trên `--card`, 5.33:1 trên `--bg`, nên mọi nhãn chữ phụ
  giờ đạt AA ở cả hai theme.
- `tools/csslint.js` không còn so với danh sách màu cứng. Nó đọc token `--*` từ
  khối `:host` của light và dark rồi kiểm 7 cặp chữ/nền mỗi theme, nên lần sau
  đổi palette sẽ không thể vô tình làm hỏng tương phản mà không báo.

### Changed / Thay đổi

**English**

- Changelog housekeeping: the `2026.10.27.4` dropdown restyle moved from *Fixed*
  to *Changed*, "four duplicated `display: flex` blocks" corrected to seven
  (4 `.stack` + 3 `.cluster`), and its accessibility claim narrowed to the
  install button that was actually fixed in that release.

**Tiếng Việt**

- Dọn lại changelog: mục dropdown của `2026.10.27.4` chuyển từ *Fixed* sang
  *Changed*, "4 khối `display: flex` bị lặp" sửa thành 7 (4 `.stack` + 3
  `.cluster`), và câu về trợ năng thu hẹp đúng phạm vi: nút Install là thứ đã
  được sửa trong bản đó.

## 2026.10.27.4 — 2026-10-27

### Changed / Thay đổi

**English**

- Design tokens: the panel now has one scale for spacing (`--sp-1..9`), type
  (`--fs-1..7`), radius (`--r-1..5`, `--r-pill`), duration (`--dur-0..3`) and
  easing (`--ease-out`). Dozens of one-off padding/font/radius values were
  replaced by them, so spacing is consistent and future tweaks are one-line.
- Composition primitives `.stack` (vertical) and `.cluster` (wrapping row) with
  a `--stack-gap` / `--cluster-gap` override, in the spirit of Every Layout.
  They replace seven duplicated `display: flex` blocks (4 `.stack` + 3
  `.cluster`); markup classes are unchanged otherwise.
- Toolbar controls (segmented group, sort select, ghost button) share a
  `min-height: 34px`, so they line up on one baseline.
- Contrast fix: the install button hard-coded `color: #fff` while `--ok`
  changed between themes (`3.3:1` in light, `2.2:1` in dark). It now uses an
  explicit `--ok-fg` pair: `#15803d`/`#fff` (5.02:1) in light and
  `#16a34a`/`#052e16` (4.52:1) in dark.
- Dropdown (sort select) restyled: native appearance removed, custom SVG
  chevron, hover tint, focus ring matching the search field, disabled state and
  themed option colours. `color-scheme: light dark` on the shadow root keeps the
  native option list and scrollbars in the panel theme instead of always light.
- `prefers-reduced-motion` also disables the select transition.

**Tiếng Việt**

- Design token: panel giờ có thang chung cho khoảng cách (`--sp-1..9`), cỡ chữ
  (`--fs-1..7`), bo góc (`--r-1..5`, `--r-pill`), thời lượng (`--dur-0..3`) và
  easing (`--ease-out`). Hàng chục giá trị padding/font/radius rời rạc đã được
  thay bằng token, nên khoảng cách đồng đều và sửa về sau chỉ cần 1 dòng.
- Thêm primitive bố cục `.stack` (dọc) và `.cluster` (hàng wrap) với biến ghi
  đè `--stack-gap` / `--cluster-gap`, theo tinh thần Every Layout. Chúng thay 7
  khối `display: flex` bị lặp (4 `.stack` + 3 `.cluster`); phần còn lại của
  markup không đổi.
- Các control trên toolbar (nhóm segmented, select sắp xếp, nút ghost) dùng
  chung `min-height: 34px` nên thẳng hàng trên cùng một baseline.
- Sửa lỗi tương phản: nút Install hard-code `color: #fff` trong khi `--ok` đổi
  theo theme (`3.3:1` ở light, `2.2:1` ở dark). Nay dùng cặp `--ok-fg` rõ ràng:
  `#15803d`/`#fff` (5.02:1) ở light và `#16a34a`/`#052e16` (4.52:1) ở dark.
- Dropdown (select sắp xếp) được làm lại: bỏ giao diện mặc định của trình duyệt,
  mũi tên SVG riêng, hover có màu nhấn, focus ring giống ô tìm kiếm, có trạng
  thái disabled và màu option theo theme. `color-scheme: light dark` trên shadow
  root giữ danh sách option và scrollbar theo theme của panel thay vì luôn sáng.
- `prefers-reduced-motion` tắt luôn transition của select.

## 2026.10.27.3 — 2026-10-27

### Fixed / Sửa lỗi

**English**

- Auto-update was dead: `@updateURL`, `@downloadURL` and the in-panel check all
  pointed at `cdn.githubraw.com`, which answers with
  `cache-control: max-age=315569000, immutable` and therefore pins the first
  cached copy forever (it still served `2026.10.26.1`). Every update URL now
  uses `raw.githubusercontent.com` (`max-age=300`).
- `maybeCheckUpdate()` falls back to `cdn.jsdelivr.net` when the raw host fails
  or replies `403`, and stays silent if both sources fail.
- `@connect raw.githubusercontent.com` and `@connect cdn.jsdelivr.net` replace
  `@connect cdn.githubraw.com`.
- README install/update links switched from the CDN to the raw URL.
- Version bumped to `2026.10.27.3`. Existing installs need one manual reinstall
  from the raw URL, because their stored `@updateURL` can never resolve to a
  newer version.

**Tiếng Việt**

- Auto-update hỏng: `@updateURL`, `@downloadURL` và bước kiểm tra trong panel
  đều trỏ `cdn.githubraw.com`, nơi trả `cache-control: max-age=315569000,
  immutable` nên giữ vĩnh viễn bản cache đầu tiên (vẫn là `2026.10.26.1`). Toàn
  bộ URL cập nhật nay dùng `raw.githubusercontent.com` (`max-age=300`).
- `maybeCheckUpdate()` tự chuyển sang `cdn.jsdelivr.net` khi host raw lỗi hoặc
  trả `403`, và im lặng nếu cả hai nguồn đều thất bại.
- `@connect raw.githubusercontent.com` và `@connect cdn.jsdelivr.net` thay cho
  `@connect cdn.githubraw.com`.
- Link cài đặt/cập nhật trong README chuyển từ CDN sang raw URL.
- Nâng phiên bản lên `2026.10.27.3`. Bản đã cài cần cài lại một lần từ raw URL,
  vì `@updateURL` đã lưu trong máy không bao giờ thấy bản mới.

## 2026.10.27.2 — 2026-10-27

### Fixed / Sửa lỗi

**English**

- Sort dropdown opened but had no effect on the list. The value sent for "Total
  installs" was `total-installs`; Greasy Fork expects `total_installs`, and the
  server silently ignored the wrong value and returned the default "daily
  installs" ordering. Fixed in `SORTS`.
- JSON requests were sent to `greasyfork.org` / `sleazyfork.org`.
  `greasyfork.org/scripts/by-site/<domain>.json?sort=…` answered `403` with a
  Cloudflare challenge (the query-less URL only worked via a `308` redirect to
  `api.greasyfork.org`), and `sleazyfork.org` redirected `308` to
  `api.sleazyfork.org` while dropping the query string, losing `sort`, `q` and
  `page`. `fetchPageNet()` now builds the by-site and search JSON URLs from a
  new `api` field on `SERVICES` (`https://api.greasyfork.org`,
  `https://api.sleazyfork.org`). This also repairs server-side search and
  "Load more" (`page=2`).
- HTML fallback and every script/install link still use the public `base` host;
  only the JSON endpoints were moved to the API hosts.
- `PAGE_SIZE` raised from `50` to `100` to match the API page size, removing the
  `"[Script Finder+] PAGE_SIZE mismatch: got 100, expected 50"` console warning
  and making `hasMore` detection correct.
- `@connect api.greasyfork.org` and `@connect api.sleazyfork.org` added, and the
  version bumped to `2026.10.27.2` (metadata and `INSTALLED_VERSION` fallback).
  Existing installs must be updated/reinstalled for the new `@connect` entries
  to take effect.

**Tiếng Việt**

- Dropdown sort mở được nhưng chọn xong danh sách không đổi. Giá trị gửi cho
  "Tổng lượt cài" là `total-installs`, trong khi Greasy Fork dùng
  `total_installs`; server bỏ qua giá trị sai và trả về thứ tự mặc định "cài
  đặt hàng ngày". Đã sửa trong `SORTS`.
- Request JSON đang gọi `greasyfork.org` / `sleazyfork.org`.
  `greasyfork.org/scripts/by-site/<domain>.json?sort=…` trả `403` kèm
  Cloudflare challenge (URL không có query chỉ chạy được nhờ redirect `308`
  sang `api.greasyfork.org`), còn `sleazyfork.org` redirect `308` sang
  `api.sleazyfork.org` nhưng làm mất query string, nên `sort`, `q` và `page`
  đều bị bỏ. `fetchPageNet()` giờ tạo URL JSON by-site và search từ field `api`
  mới trong `SERVICES` (`https://api.greasyfork.org`,
  `https://api.sleazyfork.org`). Lỗi này cũng làm search server-side và "Load
  more" (`page=2`) hỏng, nay đã hoạt động lại.
- Fallback HTML và toàn bộ link script/install vẫn dùng host `base` công khai;
  chỉ các endpoint JSON được chuyển sang host API.
- `PAGE_SIZE` tăng từ `50` lên `100` cho khớp số item mỗi trang của API, hết
  cảnh báo `"[Script Finder+] PAGE_SIZE mismatch: got 100, expected 50"` và
  việc xác định `hasMore` chính xác hơn.
- Thêm `@connect api.greasyfork.org` và `@connect api.sleazyfork.org`, nâng
  phiên bản lên `2026.10.27.2` (metadata và fallback `INSTALLED_VERSION`). Bản
  đã cài cần được cập nhật/cài lại để 2 `@connect` mới có hiệu lực.

## 2026.10.27.1 — 2026-10-27

### New / Tính năng mới

**English**

- In-panel update banner. A newer version is detected by comparing the remote
  `@version` in the CDN copy of the userscript against the installed version.
  The check runs only when the panel is opened, at most once every 24 hours.
- "Update now" opens the CDN install URL and closes the panel. "Skip this
  version" stores the skipped version so the banner stays hidden until a newer
  version appears.
- "Clear cache" script-manager menu command. It clears the in-memory response
  cache and the stored update-check timestamp, so the next panel open checks
  again immediately.
- Footer "Feedback" link to the GitHub Issues page.
- i18n keys `UpdateAvailable`, `UpdateNow`, `SkipVersion`, `ClearCache`, and
  `Feedback` in every language block: en/en-GB, zh-CN, zh-TW, ja, and vi.
- `@connect cdn.githubraw.com` so the update check can read the CDN copy through
  `GM_xmlhttpRequest`.
- Version bumped to `2026.10.27.1`.

**Tiếng Việt**

- Banner cập nhật ngay trong panel. Phiên bản mới được phát hiện bằng cách so
  sánh `@version` trên bản CDN với phiên bản đang cài. Chỉ kiểm tra khi mở
  panel, tối đa một lần mỗi 24 giờ.
- "Cập nhật ngay" mở URL cài đặt trên CDN rồi đóng panel. "Bỏ qua bản này" lưu
  phiên bản đã bỏ qua để banner ẩn cho tới khi có bản mới hơn.
- Lệnh menu "Xoá cache" của trình quản lý userscript: xoá cache phản hồi trong
  bộ nhớ và mốc thời gian kiểm tra cập nhật, để lần mở panel kế tiếp kiểm tra
  lại ngay.
- Liên kết "Phản hồi" ở footer, trỏ tới trang GitHub Issues.
- Thêm khoá i18n `UpdateAvailable`, `UpdateNow`, `SkipVersion`, `ClearCache`,
  `Feedback` cho mọi khối ngôn ngữ: en/en-GB, zh-CN, zh-TW, ja, vi.
- Thêm `@connect cdn.githubraw.com` để bước kiểm tra cập nhật đọc được bản CDN
  qua `GM_xmlhttpRequest`.
- Nâng phiên bản lên `2026.10.27.1`.

### Fixed / Sửa lỗi

**English**

- Trusted Types fallback: if the page rejects the `sf-policy` policy and
  `DOMParser.parseFromString` throws, a regex extractor now pulls script id,
  name, and version out of the raw HTML, logs a console warning, and lets the
  panel render results instead of showing `TTBlocked`.
- `getSiteDomain` now uses a broad second-level suffix table (co.uk, org.uk,
  com.au, co.jp, co.kr, com.br, com.vn, com.cn, com.tw, com.hk, com.sg, com.my,
  com.ph, com.id, com.th, com.tr, com.ru, com.ua, com.pl, com.de, com.fr,
  com.es, com.it, com.nl, com.be, com.ch, com.at, com.se, com.no, com.dk,
  com.fi, com.ie, com.pt, com.gr, com.cz, com.hu, com.ro, com.bg, com.hr,
  com.rs, com.sk, com.si, com.lt, com.lv, com.ee, plus co.nz, co.za, com.ar,
  com.mx, com.co, com.pe). Three-label suffixes are checked first, then
  two-label ones.
- Greasyfork HTML parsing tries several selectors in order and uses the first
  non-empty result.
- A failed server-side query keeps the previous list visible, resets the
  pending-replace flag, and shows `LoadFailed` with the error detail. Retry
  restarts the failed query instead of appending to the stale list.
- A console warning is logged when the number of returned items differs from
  the assumed `PAGE_SIZE`.
- The local text filter is now always applied as a second safety net, including
  for results that came from a server-side query.
- `applyFilter` batches DOM updates with `requestAnimationFrame` and cancels any
  previously scheduled frame to avoid jank.
- The footer feedback link is rendered in the panel footer and its label is
  bound through `t()` in `refreshLabels`.

**Tiếng Việt**

- Fallback Trusted Types: nếu trang từ chối policy `sf-policy` và
  `DOMParser.parseFromString` ném lỗi, bộ trích xuất bằng regex sẽ lấy id, tên
  và phiên bản script từ HTML thô, ghi cảnh báo ra console, và panel vẫn hiển
  thị kết quả thay vì `TTBlocked`.
- `getSiteDomain` dùng bảng hậu tố bậc hai đầy đủ hơn (co.uk, org.uk, com.au,
  co.jp, co.kr, com.br, com.vn, com.cn, com.tw, com.hk, com.sg, com.my, com.ph,
  com.id, com.th, com.tr, com.ru, com.ua, com.pl, com.de, com.fr, com.es,
  com.it, com.nl, com.be, com.ch, com.at, com.se, com.no, com.dk, com.fi,
  com.ie, com.pt, com.gr, com.cz, com.hu, com.ro, com.bg, com.hr, com.rs,
  com.sk, com.si, com.lt, com.lv, com.ee, cùng co.nz, co.za, com.ar, com.mx,
  com.co, com.pe). Kiểm tra hậu tố 3 nhãn trước, rồi tới 2 nhãn.
- Phân tích HTML Greasyfork thử nhiều selector theo thứ tự và dùng kết quả đầu
  tiên không rỗng.
- Truy vấn phía server thất bại sẽ giữ danh sách cũ, đặt lại cờ pending-replace
  và hiện `LoadFailed` kèm chi tiết lỗi. Nút Thử lại chạy lại truy vấn đã lỗi
  thay vì nối thêm vào danh sách cũ.
- Ghi cảnh báo console khi số item trả về khác `PAGE_SIZE` giả định.
- Bộ lọc văn bản cục bộ luôn được áp dụng như lớp bảo hiểm thứ hai, kể cả với
  kết quả đến từ truy vấn phía server.
- `applyFilter` gộp cập nhật DOM bằng `requestAnimationFrame` và huỷ frame đã
  hẹn trước đó để tránh giật.
- Liên kết phản hồi được render ở footer panel và nhãn của nó được gắn qua
  `t()` trong `refreshLabels`.
