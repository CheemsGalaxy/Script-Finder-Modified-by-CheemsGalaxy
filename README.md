# Script Finder+ <sub>· Modified by CheemsGalaxy</sub>

Find userscripts for the site you are on — without leaving the page.

A community fork of [Script Finder+](https://greasyfork.org/en/scripts/498904-script-finder) by
[ChinaGodMan](https://github.com/ChinaGodMan/UserScripts) · MIT license

**[English](#english)** · **[Tiếng Việt](#tiếng-việt)**

| | |
|---|---|
| **Version** | `2026.10.27.4` |
| **Userscript page** | [greasyfork.org/scripts/600159](https://greasyfork.org/en/scripts/600159-script-finder-modified-by-cheemsgalaxy) |
| **Install / update URL** | [`Script_Finder_Modified_by_CheemsGalaxy.user.js`](https://raw.githubusercontent.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js) (GitHub raw) |
| **Mirror** | [jsDelivr](https://cdn.jsdelivr.net/gh/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy@main/Script_Finder_Modified_by_CheemsGalaxy.user.js) (fallback only) |
| **Issues / feedback** | [GitHub Issues](https://github.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/issues) |
| **License** | MIT (inherited from the original) |

> **Important — installed `2026.10.27.2` or older?**
> Those installs store an `@updateURL` that points at a permanently cached CDN copy, so they will
> never see a newer version. Reinstall **once** from the raw URL above; afterwards updates work
> normally.

---

## English

### What it does

Script Finder+ adds a small draggable tab to the right edge of every page. Open it to see the
userscripts published on **GreasyFork** (and **SleazyFork**) that match the site you are visiting,
search the whole catalog, sort by installs / rating / age, and open an install link in one click.

- **Site-aware** — reads the current domain, including `co.uk`, `com.vn`, IP addresses and
  `localhost`.
- **Two search layers** — instant local filtering over loaded results, plus a debounced
  server-side query that reaches the rest of the catalog.
- **Two services** — switch GreasyFork ⇄ SleazyFork from the panel, no reload.
- **Remembers state** — service, sort order, tab position and skipped versions.

### Install

1. Install a userscript manager: **Tampermonkey**, **Violentmonkey** or **Userscripts** (Safari).
2. Open the install URL — [raw GitHub](https://raw.githubusercontent.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js).
3. Accept the permissions listed [below](#permissions); the manager installs the `.user.js` file.
4. Reload any page and look for the tab on the right edge (`Scripts`).

### Updating

- `@updateURL` and `@downloadURL` both point at `raw.githubusercontent.com`, so managers offer
  updates automatically. The in-panel check runs at most once every 24 hours.
- If the raw host fails or answers `403`, the check falls back to `cdn.jsdelivr.net`.
- To force an update: userscript manager → *Check for updates*, or reopen the install URL.

### Features

| | |
|---|---|
| Service switcher | GreasyFork / SleazyFork segmented control |
| Sorting | Daily installs · Recently updated · Total installs · Top rated (server-side) |
| Search | Local filter + server query from 2 characters, 500 ms debounce |
| Result cards | Name, description, author, installs, daily installs, dates, rating, install button |
| Pagination | 100 results per request, "Load more" in the footer |
| UI | Shadow DOM, light/dark via `prefers-color-scheme`, bottom sheet under 600 px |
| Accessibility | Focus trap, `Esc` to close, `Enter`/`Space` on the tab, `prefers-reduced-motion` |
| Menu commands | Open/close · Reset tab position · Clear cache |

### How this fork differs from upstream

Details and dates for every release live in [CHANGELOG.md](CHANGELOG.md).

| Area | Original | This fork |
|---|---|---|
| UI isolation | page-wide `GM_addStyle` | Shadow DOM with `all: initial` |
| DOM building | `innerHTML` | `createElement` / `createElementNS` only |
| Trusted Types | none | `sf-policy` + regex fallback when the policy is rejected |
| Network | no timeout, no status check | 20 s timeout, 2xx required, 403/404/429 handled |
| Throttling | none | 700 ms between requests |
| Rate limiting | none | honours `Retry-After` (5–300 s) with a visible countdown |
| Caching | none | 5-minute response cache, clearable from the menu |
| Pagination | `loadedPages` as number *and* `"max"` | separate `page` / `hasMore` / `loading` |
| Search | local filter only | local + server query (≥ 2 chars) |
| Sorting | fixed | 4 server-side sorts, persisted |
| Services | GreasyFork only | GreasyFork + SleazyFork |
| Domain detection | `hostname.split(".").slice(-2)` | `getSiteDomain()` handles SLD, IP and port |
| Languages | broken `en` fallback | en/en-GB · zh-CN · zh-TW · ja · vi |
| Errors | silent failure / `ReferenceError` | status line + retry button |
| Mobile | centered panel | bottom sheet ≤ 600 px |
| Upstream URL | `cdn.githubraw.com` (immutable cache) | `raw.githubusercontent.com` + jsDelivr fallback |

### Permissions

| Permission | Why it is needed |
|---|---|
| `@match *://*/*` | the tab must be available on any site |
| `GM_xmlhttpRequest` | read result pages and the JSON API |
| `@connect greasyfork.org` · `sleazyfork.org` · `api.greasyfork.org` · `api.sleazyfork.org` | the only hosts contacted for results |
| `@connect raw.githubusercontent.com` · `cdn.jsdelivr.net` | update check |
| `GM_getValue` / `GM_setValue` | service, sort order, tab position, skipped version |
| `GM_registerMenuCommand` | the three menu entries |
| `GM_openInTab` | open a script page in a new tab |

No analytics and no third-party requests: the only outbound traffic goes to the hosts above.
`@noframes` keeps the script out of iframes, and `@run-at document-idle` keeps it off the
critical path.

### Troubleshooting

| Symptom | Reason / fix |
|---|---|
| No tab on the page | The page is an iframe (`@noframes`), or the manager blocked the script. Check the console for `[Script Finder+]` lines |
| `Trusted Types policy rejected` in console | Expected on strict-CSP sites; the regex fallback takes over and results still load |
| "Rate limited — retry in N s" | GreasyFork answered `429`. Wait for the countdown; "Load more" re-enables itself |
| Empty list, no error | Cloudflare challenge or API outage. The HTML fallback is tried; retry later |
| Updates are never offered | Installed `2026.10.27.2` or older — reinstall once from the raw URL |
| Tab covers page content | Drag it vertically, or use the menu entry *Reset tab position* |
| Stale results after a search | Menu entry *Clear cache*, then reload |

### Development

Single file, no build step, no dependencies.

```
Script_Finder_Modified_by_CheemsGalaxy.user.js   the userscript
CHANGELOG.md                                     per-release notes (EN/VI)
README.md
tools/
  check.js        logic harness: getSiteDomain, cmpVersion, HTML fallback parser
  urlcheck.js     prints the API URLs the panel would request
  updatecheck.js  simulates the update check incl. the jsDelivr fallback
  csslint.js      CSS vars, brace balance, version sync, WCAG AA contrast
  preview.js      shadow-DOM previews + screenshots into preview-out/
```

```powershell
node --check .\Script_Finder_Modified_by_CheemsGalaxy.user.js
node .\tools\check.js
node .\tools\csslint.js
node .\tools\preview.js      # needs Edge or Chrome, writes preview-out\*.png
```

The UI follows strict house rules: no comments, no `innerHTML`, Shadow DOM only, every string
through `t(key)`, CSS driven by design tokens (`--sp-*`, `--fs-*`, `--r-*`, `--dur-*`).

**Release checklist:** bump `@version`, bump `INSTALLED_VERSION`, add a CHANGELOG entry, update
the version row at the top of this file — `tools/csslint.js` fails if those drift apart.

### Credits

- **Original author:** [ChinaGodMan](https://github.com/ChinaGodMan/UserScripts) — creator and
  maintainer of Script Finder+.
- **Co-author (original):** [shiquda](https://github.com/shiquda).
- **Original project:** [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder) ·
  [GitHub](https://github.com/ChinaGodMan/UserScripts).
- **Fork maintainer:** [CheemsGalaxy](https://github.com/CheemsGalaxy).
- **License:** MIT, inherited from the original; all original credits are preserved.

Thanks to the GreasyFork and userscript communities for testing, feedback and inspiration.

---

## Tiếng Việt

### Công cụ này làm gì

Script Finder+ thêm một tab nhỏ (kéo được) ở mép phải mọi trang web. Mở tab để xem các userscript
đang có trên **GreasyFork** (và **SleazyFork**) khớp với website bạn đang truy cập, tìm kiếm trên
toàn bộ danh mục, sắp xếp theo lượt cài / đánh giá / thời gian và mở link cài đặt chỉ với một cú
click.

- **Nhận diện website** — đọc đúng tên miền hiện tại, kể cả `co.uk`, `com.vn`, địa chỉ IP và
  `localhost`.
- **Tìm kiếm hai lớp** — lọc cục bộ tức thời trên kết quả đã tải, cộng với truy vấn phía server
  (có debounce) để với tới phần còn lại của danh mục.
- **Hai dịch vụ** — chuyển GreasyFork ⇄ SleazyFork ngay trong panel, không cần tải lại trang.
- **Ghi nhớ trạng thái** — dịch vụ, kiểu sắp xếp, vị trí tab và phiên bản đã bỏ qua.

### Cài đặt

1. Cài một trình quản lý userscript: **Tampermonkey**, **Violentmonkey** hoặc **Userscripts**
   (Safari).
2. Mở link cài đặt — [raw GitHub](https://raw.githubusercontent.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js).
3. Chấp nhận các quyền ở [bảng bên dưới](#quyền-truy-cập); trình quản lý sẽ cài tệp `.user.js`.
4. Tải lại bất kỳ trang nào và tìm tab `Scripts` ở mép phải màn hình.

### Cập nhật

- `@updateURL` và `@downloadURL` đều trỏ `raw.githubusercontent.com`, nên trình quản lý tự đề xuất
  bản mới. Bước kiểm tra trong panel chạy tối đa một lần mỗi 24 giờ.
- Nếu host raw lỗi hoặc trả `403`, bước kiểm tra tự chuyển sang `cdn.jsdelivr.net`.
- Muốn cập nhật ngay: trình quản lý → *Check for updates*, hoặc mở lại link cài đặt.

### Tính năng

| | |
|---|---|
| Chuyển dịch vụ | Segmented control GreasyFork / SleazyFork |
| Sắp xếp | Cài đặt hàng ngày · Mới cập nhật · Tổng lượt cài · Đánh giá cao (phía server) |
| Tìm kiếm | Lọc cục bộ + truy vấn server từ 2 ký tự, debounce 500 ms |
| Thẻ kết quả | Tên, mô tả, tác giả, tổng lượt cài, lượt cài/ngày, ngày tạo/cập nhật, đánh giá, nút cài |
| Phân trang | 100 kết quả mỗi lần, nút "Tải thêm" ở footer |
| Giao diện | Shadow DOM, sáng/tối theo `prefers-color-scheme`, bottom sheet dưới 600 px |
| Trợ năng | Bẫy focus, `Esc` để đóng, `Enter`/`Space` trên tab, `prefers-reduced-motion` |
| Menu lệnh | Mở/đóng · Đặt lại vị trí tab · Xoá cache |

### Bản fork này khác bản gốc ở đâu

Chi tiết và ngày phát hành của từng bản nằm trong [CHANGELOG.md](CHANGELOG.md).

| Hạng mục | Bản gốc | Bản fork này |
|---|---|---|
| Cô lập giao diện | `GM_addStyle` toàn trang | Shadow DOM + `all: initial` |
| Tạo DOM | `innerHTML` | Chỉ `createElement` / `createElementNS` |
| Trusted Types | không có | `sf-policy` + fallback regex khi policy bị từ chối |
| Mạng | không timeout, không kiểm tra mã | Timeout 20 giây, bắt buộc 2xx, xử lý 403/404/429 |
| Giãn cách request | không có | 700 ms giữa các request |
| Giới hạn tần suất | không có | Tôn trọng `Retry-After` (5–300 giây), có đếm ngược |
| Bộ nhớ đệm | không có | Cache phản hồi 5 phút, xoá được từ menu |
| Phân trang | `loadedPages` vừa số vừa `"max"` | Tách riêng `page` / `hasMore` / `loading` |
| Tìm kiếm | chỉ lọc cục bộ | Cục bộ + truy vấn server (≥ 2 ký tự) |
| Sắp xếp | cố định | 4 kiểu phía server, có ghi nhớ |
| Dịch vụ | chỉ GreasyFork | GreasyFork + SleazyFork |
| Nhận diện tên miền | `hostname.split(".").slice(-2)` | `getSiteDomain()` xử lý SLD, IP và cổng |
| Ngôn ngữ | fallback `en` bị lỗi | en/en-GB · zh-CN · zh-TW · ja · vi |
| Xử lý lỗi | thất bại âm thầm / `ReferenceError` | Dòng trạng thái + nút thử lại |
| Di động | panel giữa màn hình | Bottom sheet ≤ 600 px |
| URL cập nhật | `cdn.githubraw.com` (cache vĩnh viễn) | `raw.githubusercontent.com` + fallback jsDelivr |

### Quyền truy cập

| Quyền | Vì sao cần |
|---|---|
| `@match *://*/*` | Tab phải có mặt trên mọi trang |
| `GM_xmlhttpRequest` | Đọc trang kết quả và JSON API |
| `@connect greasyfork.org` · `sleazyfork.org` · `api.greasyfork.org` · `api.sleazyfork.org` | Chỉ những host này được gọi để lấy kết quả |
| `@connect raw.githubusercontent.com` · `cdn.jsdelivr.net` | Kiểm tra bản cập nhật |
| `GM_getValue` / `GM_setValue` | Dịch vụ, kiểu sắp xếp, vị trí tab, phiên bản đã bỏ qua |
| `GM_registerMenuCommand` | Ba mục menu |
| `GM_openInTab` | Mở trang script ở tab mới |

Không có analytics, không gửi dữ liệu cho bên thứ ba: toàn bộ lưu lượng đi/đến chỉ tới các host
trên. `@noframes` giữ script ngoài iframe, `@run-at document-idle` giữ nó khỏi đường tới hạn của
trang.

### Xử lý sự cố

| Hiện tượng | Nguyên nhân / cách xử lý |
|---|---|
| Không thấy tab | Trang đang nằm trong iframe (`@noframes`), hoặc trình quản lý chặn script. Xem console có dòng `[Script Finder+]` không |
| Console báo `Trusted Types policy rejected` | Bình thường trên site CSP nghiêm ngặt; fallback regex tiếp quản, kết quả vẫn tải được |
| "Rate limited — retry in N s" | GreasyFork trả `429`. Chờ hết đếm ngược, nút "Tải thêm" tự bật lại |
| Danh sách trống, không báo lỗi | Trang thử thách Cloudflare hoặc API gián đoạn. Fallback HTML đã được thử; quay lại sau |
| Không bao giờ được đề xuất cập nhật | Đang cài `2026.10.27.2` trở về trước — cài lại một lần từ raw URL |
| Tab che nội dung trang | Kéo tab lên/xuống, hoặc dùng menu *Đặt lại vị trí tab* |
| Kết quả cũ sau khi tìm | Menu *Xoá cache* rồi tải lại trang |

### Phát triển

Một tệp duy nhất, không build, không dependency.

```
Script_Finder_Modified_by_CheemsGalaxy.user.js   userscript
CHANGELOG.md                                     ghi chú từng bản (EN/VI)
README.md
tools/
  check.js        harness logic: getSiteDomain, cmpVersion, parser fallback HTML
  urlcheck.js     in ra URL API mà panel sẽ gọi
  updatecheck.js  mô phỏng bước kiểm tra cập nhật, gồm cả fallback jsDelivr
  csslint.js      biến CSS, cân bằng ngoặc, đồng bộ version, tương phản WCAG AA
  preview.js      dựng preview Shadow DOM + chụp ảnh vào preview-out/
```

```powershell
node --check .\Script_Finder_Modified_by_CheemsGalaxy.user.js
node .\tools\check.js
node .\tools\csslint.js
node .\tools\preview.js      # cần Edge hoặc Chrome, ghi preview-out\*.png
```

Giao diện theo bộ quy tắc cố định: không comment, không `innerHTML`, chỉ Shadow DOM, mọi chuỗi
đi qua `t(key)`, CSS dùng design token (`--sp-*`, `--fs-*`, `--r-*`, `--dur-*`).

**Checklist phát hành:** tăng `@version`, tăng `INSTALLED_VERSION`, thêm entry CHANGELOG, cập nhật
dòng version ở đầu tệp này — `tools/csslint.js` sẽ báo lỗi nếu các chỗ đó lệch nhau.

### Ghi công

- **Tác giả gốc:** [ChinaGodMan](https://github.com/ChinaGodMan/UserScripts) — người tạo và duy trì
  Script Finder+.
- **Đồng tác giả (bản gốc):** [shiquda](https://github.com/shiquda).
- **Dự án gốc:** [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder) ·
  [GitHub](https://github.com/ChinaGodMan/UserScripts).
- **Người duy trì bản fork:** [CheemsGalaxy](https://github.com/CheemsGalaxy).
- **Giấy phép:** MIT, kế thừa từ bản gốc; toàn bộ ghi công tác giả gốc được giữ nguyên.

Cảm ơn cộng đồng GreasyFork và cộng đồng userscript đã kiểm thử, góp ý và truyền cảm hứng.
