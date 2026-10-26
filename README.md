# Script Finder+ Modified by CheemsGalaxy

**A community-modified edition of the original [Script Finder+](https://greasyfork.org/en/scripts/498904-script-finder) by [ChinaGodMan](https://github.com/ChinaGodMan/UserScripts).**

- **Modified edition (GreasyFork):** [Script Finder+ Modified by CheemsGalaxy](https://greasyfork.org/en/scripts/600159-script-finder-modified-by-cheemsgalaxy)
- **Direct install / update URL (CDN):** [Script_Finder_Modified_by_CheemsGalaxy.user.js](https://cdn.githubraw.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js)
- **Original project:** [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder) · [GitHub](https://github.com/ChinaGodMan/UserScripts)
- **License:** MIT (inherited from the original)

---

## Overview

**Script Finder+** lets you discover userscripts published on Greasyfork (and SleazyFork) that are relevant to the website you are currently visiting — without leaving the page.

This **Modified by CheemsGalaxy** edition is a community fork of the original script. It was created to address long-standing limitations of the upstream version and to deliver a more robust, secure, and modern experience for daily users. The primary goals of this fork are:

- **Security first** — eliminate unsafe HTML injection and stay compatible with strict browser policies (Trusted Types / CSP).
- **Reliability** — handle network errors, HTTP failures, and rate limiting gracefully instead of silently breaking.
- **Better UX** — a cleaner, isolated interface that does not interfere with the host website.
- **Broader coverage** — search across the full Greasyfork catalog, not just the pages already loaded.

This edition is intended for users who rely on Script Finder+ regularly and want a more stable, secure, and polished tool. It is **not** a replacement for the original project — it is a complementary, community-maintained alternative.

> **Note:** This fork preserves the original author's credits and the MIT license. If you only need the basic functionality, the original script remains a perfectly valid choice.

---

## Changelog / What's New

> The sections below describe the main categories of changes in this modified edition compared to the original. Each item is annotated so the exact changes can be filled in or adjusted as the fork evolves.

### 🚀 Performance & Architecture

- **Shadow DOM isolation** — the entire UI now lives inside a Shadow Root, so the script's CSS no longer leaks into (or is affected by) the host website's styles. *(Replaces the original page-wide `GM_addStyle` injection.)*
- **No more `innerHTML`** — all UI elements are built with `createElement` / `createElementNS`, eliminating a class of rendering bugs and security risks. *(The original used `innerHTML` for script descriptions.)*
- **Trusted Types support** — a dedicated `sf-policy` is used when parsing HTML returned by Greasyfork, fixing blank-page / silent failures on websites that enforce `require-trusted-types-for 'script'` (e.g. YouTube, Google services). *(New in this fork.)*
- **Request throttling & caching** — requests to Greasyfork are spaced out (700 ms) and responses are cached for 5 minutes to reduce load and avoid rate limiting. *(New in this fork.)*
- **Promise-based network layer** — `GM_xmlhttpRequest` is wrapped in a Promise with proper `onload` / `onerror` / `ontimeout` / `onabort` handling and a 20-second timeout. *(The original had no timeout and no HTTP status checks.)*

### 🐞 Bug Fixes

- **Fixed `ReferenceError` on network error** — the original `onerror` handler referenced `scriptsInfo` before its `const` declaration, crashing the script. Replaced with a self-contained `fail()` path. *(Fixed in this fork.)*
- **Fixed i18n fallback** — the original fell back to a non-existent `translations['en']` key and failed to match languages such as `en-US` or `vi-VN`. Language resolution now normalizes codes, matches exact → base language → English. *(Fixed in this fork.)*
- **Fixed domain detection** — `hostname.split('.').slice(-2)` returned wrong results for `co.uk`, `com.vn`, IP addresses, and `localhost`. A `getSiteDomain()` helper now handles common SLDs, IPs, and ports. *(Fixed in this fork.)*
- **Fixed pagination state** — the original used `loadedPages` as both a number and the string `'max'`, and reset it to `0` after the last page. Replaced with separate `page`, `hasMore`, and `loading` flags. *(Fixed in this fork.)*
- **Fixed "Load more" button placement** — the button and loading indicator were appended inside the `<ul>` list, producing invalid HTML and odd layout. They now live in a proper `<footer>`. *(Fixed in this fork.)*
- **Fixed unencoded query strings** — `?q=${searchInput.value}` broke on keywords containing `&`, `#`, or spaces. Now uses `encodeURIComponent` for both keyword and domain. *(Fixed in this fork.)*
- **Fixed empty `q=` causing HTTP 400** — the "View on Greasyfork" button no longer appends `?q=` when the search box is empty. *(New fix in this fork.)*
- **Fixed `@noframes` omission** — the script no longer runs (and then exits) inside every iframe. *(New in this fork.)*

### 🎨 UI / UX Improvements

- **Redesigned floating tab** — a compact, rounded tab on the right edge of the screen. It expands on hover, keyboard focus, drag, or while the panel is open, and remembers its vertical position across sessions (`GM_getValue` / `GM_setValue`). *(Replaces the original `<scrbutton>` element.)*
- **Modern panel layout** — header with title, result count, and current domain; a toolbar with service switcher and sort selector; a scrollable list of cards; and a footer with status and "Load more". *(Full redesign.)*
- **Skeleton loading** — placeholder cards are shown while the first page loads, instead of a blank list. *(New in this fork.)*
- **Keyboard support** — `Tab` focus trap inside the panel, `Esc` to close, `Enter` / `Space` to open the tab. *(New in this fork.)*
- **Mobile-friendly** — on screens ≤ 600 px, the panel becomes a bottom sheet that slides up from the bottom edge. *(Improved over the original centered panel.)*
- **Dark mode & reduced motion** — respects `prefers-color-scheme` and `prefers-reduced-motion`. *(Extended over the original.)*
- **Script-manager menu commands** — "Open/Close" and "Reset tab position" are registered via `GM_registerMenuCommand`. *(New in this fork.)*

### 🔍 Search & Discovery

- **Two-layer search** — instant local filtering over already-loaded results, plus a debounced (500 ms) server-side query so scripts beyond the loaded pages can also be found. *(The original only filtered already-loaded items.)*
- **Server-side sorting** — choose between *Daily installs*, *Recently updated*, *Total installs*, and *Top rated*. The choice is persisted. *(New in this fork.)*
- **Dual service support** — switch between **Greasyfork** and **SleazyFork** directly from the panel. *(New in this fork.)*
- **Smarter filtering** — filtering now considers name, description, and author, instead of the entire item text. *(Improved over the original.)*

### 🌐 Localization

- **Expanded translations** — added new keys for retry, rate limiting, Trusted Types errors, sorting, menu commands, and more. *(Extended over the original.)*
- **More reliable language matching** — full code match, then base-language match, then English fallback. *(Fixed in this fork.)*

### 🛡️ Error Handling & Rate Limiting

- **HTTP status awareness** — 2xx is required for success; 403/404/429 are handled explicitly. *(The original ignored HTTP status entirely.)*
- **Cloudflare challenge detection** — if Greasyfork returns a challenge page instead of a list, the script reports a clear error rather than showing an empty list. *(New in this fork.)*
- **Rate-limit cooldown** — when the server returns 429, a countdown timer is shown and the "Load more" button is re-enabled once the cooldown ends. *(New in this fork.)*
- **Retry button** — failed loads now show a dedicated status line and a retry button, instead of disabling the button with an error message. *(Fixed in this fork.)*

---

## Installation & Updates

### Requirements

- A userscript manager such as **Tampermonkey**, **Violentmonkey**, or **Userscripts (Safari)**.
- Access to `greasyfork.org` and `sleazyfork.org`.

### Install

1. Open the direct install URL in your browser:
   👉 [https://cdn.githubraw.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js](https://cdn.githubraw.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js)
2. Your userscript manager should detect the `.user.js` file and prompt you to install it.
3. Accept the requested permissions (`GM_xmlhttpRequest`, `GM_getValue`, `GM_setValue`, `GM_registerMenuCommand`, `GM_openInTab`).
4. Reload any page and look for the **Script Finder+** tab on the right edge of the screen.

### Update

- If your userscript manager supports `@updateURL` / `@downloadURL`, updates will be offered automatically.
- Otherwise, re-open the install URL above — the manager will replace the existing version.
- When upgrading from the original Script Finder+, your manager may ask for **additional permissions** for the new `GM_*` APIs. Accept them to enable position memory, menu commands, and tab opening.

### Uninstall / Rollback

- Disable or remove the script from your userscript manager.
- If you want to return to the original, reinstall it from [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder).

> **Tip:** If the panel does not appear on a specific website, open DevTools → Console and look for `[Script Finder+]` messages (e.g. Trusted Types or rate-limit errors) to understand why.

---

## Credits

This modified edition would not exist without the work of the original authors:

- **Original author:** [ChinaGodMan](https://github.com/ChinaGodMan/UserScripts) — creator and maintainer of Script Finder+.
- **Co-author (original):** [shiquda](https://github.com/shiquda).
- **Original project:** [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder) · [GitHub](https://github.com/ChinaGodMan/UserScripts).
- **License:** MIT — this fork retains the original MIT license and all author credits.

Special thanks to the Greasyfork and userscript communities for testing, feedback, and inspiration.

---

*Maintained by **CheemsGalaxy**. Feedback and bug reports are welcome via the GreasyFork feedback page linked at the top of this document.*

---
---

# Script Finder+ Modified by CheemsGalaxy

**Phiên bản cộng đồng được chỉnh sửa từ [Script Finder+](https://greasyfork.org/en/scripts/498904-script-finder) gốc của [ChinaGodMan](https://github.com/ChinaGodMan/UserScripts).**

- **Bản chỉnh sửa (GreasyFork):** [Script Finder+ Modified by CheemsGalaxy](https://greasyfork.org/en/scripts/600159-script-finder-modified-by-cheemsgalaxy)
- **Liên kết cài đặt / cập nhật trực tiếp (CDN):** [Script_Finder_Modified_by_CheemsGalaxy.user.js](https://cdn.githubraw.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js)
- **Dự án gốc:** [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder) · [GitHub](https://github.com/ChinaGodMan/UserScripts)
- **Giấy phép:** MIT (kế thừa từ bản gốc)

---

## Tổng quan

**Script Finder+** giúp bạn khám phá các userscript được đăng trên Greasyfork (và SleazyFork) có liên quan đến website bạn đang truy cập — mà không cần rời khỏi trang.

Phiên bản **Modified by CheemsGalaxy** này là một bản fork cộng đồng của script gốc. Nó ra đời nhằm khắc phục những hạn chế lâu năm của phiên bản gốc và mang lại trải nghiệm ổn định, an toàn và hiện đại hơn cho người dùng hàng ngày. Các mục tiêu chính của bản fork này gồm:

- **Bảo mật là trên hết** — loại bỏ việc chèn HTML không an toàn và duy trì tương thích với các chính sách bảo mật nghiêm ngặt của trình duyệt (Trusted Types / CSP).
- **Độ tin cậy** — xử lý lỗi mạng, lỗi HTTP và giới hạn tần suất (rate limiting) một cách mạch lạc thay vì thất bại âm thầm.
- **Trải nghiệm người dùng tốt hơn** — giao diện gọn gàng, được cô lập, không ảnh hưởng đến website đang xem.
- **Phạm vi bao phủ rộng hơn** — tìm kiếm trên toàn bộ danh mục Greasyfork, không chỉ các trang đã tải.

Phiên bản này hướng đến những người dùng thường xuyên của Script Finder+, mong muốn một công cụ ổn định, an toàn và chỉn chu hơn. Đây **không phải** là bản thay thế cho dự án gốc — mà là một lựa chọn bổ sung do cộng đồng duy trì.

> **Lưu ý:** Bản fork này giữ nguyên ghi công của tác giả gốc và giấy phép MIT. Nếu bạn chỉ cần chức năng cơ bản, bản gốc vẫn là lựa chọn hoàn toàn hợp lý.

---

## Nhật ký thay đổi / Có gì mới

> Các mục dưới đây mô tả những nhóm thay đổi chính trong phiên bản chỉnh sửa này so với bản gốc. Mỗi mục đều được chú thích để bạn có thể điền hoặc điều chỉnh thay đổi chính xác khi fork tiếp tục phát triển.

### 🚀 Hiệu năng & Kiến trúc

- **Cô lập bằng Shadow DOM** — toàn bộ giao diện nay nằm trong một Shadow Root, nên CSS của script không còn rò rỉ vào (hoặc bị ảnh hưởng bởi) CSS của website đang xem. *(Thay thế cơ chế chèn `GM_addStyle` toàn trang của bản gốc.)*
- **Không còn `innerHTML`** — mọi phần tử giao diện được tạo bằng `createElement` / `createElementNS`, loại bỏ một nhóm lỗi hiển thị và rủi ro bảo mật. *(Bản gốc dùng `innerHTML` cho phần mô tả script.)*
- **Hỗ trợ Trusted Types** — một `sf-policy` chuyên dụng được dùng khi phân tích HTML trả về từ Greasyfork, khắc phục lỗi trang trắng / thất bại âm thầm trên các website áp dụng `require-trusted-types-for 'script'` (ví dụ YouTube, dịch vụ Google). *(Mới trong bản fork này.)*
- **Giãn cách & bộ nhớ đệm yêu cầu** — các yêu cầu tới Greasyfork được giãn cách (700 ms) và phản hồi được lưu đệm trong 5 phút để giảm tải và tránh bị giới hạn tần suất. *(Mới trong bản fork này.)*
- **Tầng mạng dựa trên Promise** — `GM_xmlhttpRequest` được bọc trong một Promise với xử lý đầy đủ `onload` / `onerror` / `ontimeout` / `onabort` và timeout 20 giây. *(Bản gốc không có timeout và không kiểm tra mã HTTP.)*

### 🐞 Sửa lỗi

- **Sửa lỗi `ReferenceError` khi có lỗi mạng** — trình xử lý `onerror` của bản gốc tham chiếu `scriptsInfo` trước khi khai báo `const`, làm script bị crash. Đã thay bằng luồng `fail()` độc lập. *(Đã sửa trong bản fork này.)*
- **Sửa fallback i18n** — bản gốc fallback về key `translations['en']` không tồn tại và không khớp được các ngôn ngữ như `en-US` hay `vi-VN`. Việc chọn ngôn ngữ nay chuẩn hóa mã, khớp chính xác → ngôn ngữ gốc → tiếng Anh. *(Đã sửa trong bản fork này.)*
- **Sửa nhận diện tên miền** — `hostname.split('.').slice(-2)` trả về kết quả sai với `co.uk`, `com.vn`, địa chỉ IP và `localhost`. Hàm `getSiteDomain()` nay xử lý SLD phổ biến, IP và cổng. *(Đã sửa trong bản fork này.)*
- **Sửa trạng thái phân trang** — bản gốc dùng `loadedPages` vừa là số vừa là chuỗi `'max'`, và gán lại về `0` sau trang cuối. Đã thay bằng các cờ riêng `page`, `hasMore`, `loading`. *(Đã sửa trong bản fork này.)*
- **Sửa vị trí nút "Tải thêm"** — nút và chỉ báo tải được thêm vào bên trong `<ul>`, tạo HTML không hợp lệ và bố cục kỳ lạ. Nay chúng nằm trong `<footer>` hợp lệ. *(Đã sửa trong bản fork này.)*
- **Sửa chuỗi truy vấn chưa mã hóa** — `?q=${searchInput.value}` bị hỏng với từ khóa chứa `&`, `#` hoặc dấu cách. Nay dùng `encodeURIComponent` cho cả từ khóa và tên miền. *(Đã sửa trong bản fork này.)*
- **Sửa lỗi `q=` rỗng gây HTTP 400** — nút "Xem trên Greasyfork" không còn thêm `?q=` khi ô tìm kiếm trống. *(Sửa mới trong bản fork này.)*
- **Bổ sung `@noframes`** — script không còn chạy (rồi thoát) bên trong mọi iframe. *(Mới trong bản fork này.)*

### 🎨 Cải thiện giao diện & trải nghiệm

- **Thiết kế lại tab nổi** — tab bo góc nhỏ gọn ở mép phải màn hình. Tab mở rộng khi hover, focus bàn phím, kéo, hoặc khi panel đang mở, và ghi nhớ vị trí dọc qua các phiên (`GM_getValue` / `GM_setValue`). *(Thay thế phần tử `<scrbutton>` của bản gốc.)*
- **Bố cục panel hiện đại** — header với tiêu đề, số lượng kết quả và tên miền hiện tại; thanh công cụ với bộ chuyển dịch vụ và bộ chọn sắp xếp; danh sách thẻ cuộn được; và footer với trạng thái và "Tải thêm". *(Thiết kế lại toàn bộ.)*
- **Skeleton loading** — thẻ placeholder được hiển thị trong khi tải trang đầu, thay vì danh sách trống. *(Mới trong bản fork này.)*
- **Hỗ trợ bàn phím** — bẫy focus `Tab` trong panel, `Esc` để đóng, `Enter` / `Space` để mở tab. *(Mới trong bản fork này.)*
- **Thân thiện với di động** — trên màn hình ≤ 600 px, panel trở thành bottom sheet trượt lên từ mép dưới. *(Cải thiện so với panel giữa màn hình của bản gốc.)*
- **Chế độ tối & giảm chuyển động** — tôn trọng `prefers-color-scheme` và `prefers-reduced-motion`. *(Mở rộng so với bản gốc.)*
- **Menu lệnh của trình quản lý script** — "Mở/Đóng" và "Đặt lại vị trí tab" được đăng ký qua `GM_registerMenuCommand`. *(Mới trong bản fork này.)*

### 🔍 Tìm kiếm & Khám phá

- **Tìm kiếm hai lớp** — lọc cục bộ tức thời trên kết quả đã tải, cộng với truy vấn phía server có debounce (500 ms) để tìm cả các script ngoài các trang đã tải. *(Bản gốc chỉ lọc các mục đã tải.)*
- **Sắp xếp phía server** — chọn giữa *Cài đặt hàng ngày*, *Mới cập nhật*, *Tổng lượt cài* và *Đánh giá cao*. Lựa chọn được ghi nhớ. *(Mới trong bản fork này.)*
- **Hỗ trợ hai dịch vụ** — chuyển đổi giữa **Greasyfork** và **SleazyFork** ngay từ panel. *(Mới trong bản fork này.)*
- **Lọc thông minh hơn** — bộ lọc nay xét tên, mô tả và tác giả, thay vì toàn bộ văn bản của mục. *(Cải thiện so với bản gốc.)*

### 🌐 Bản địa hóa

- **Mở rộng bản dịch** — thêm key mới cho thử lại, giới hạn tần suất, lỗi Trusted Types, sắp xếp, menu lệnh, v.v. *(Mở rộng so với bản gốc.)*
- **Khớp ngôn ngữ đáng tin cậy hơn** — khớp đầy đủ mã, rồi khớp ngôn ngữ gốc, rồi fallback tiếng Anh. *(Đã sửa trong bản fork này.)*

### 🛡️ Xử lý lỗi & Giới hạn tần suất

- **Nhận biết mã HTTP** — yêu cầu 2xx mới được coi là thành công; 403/404/429 được xử lý tường minh. *(Bản gốc bỏ qua hoàn toàn mã HTTP.)*
- **Phát hiện thử thách Cloudflare** — nếu Greasyfork trả về trang thử thách thay vì danh sách, script báo lỗi rõ ràng thay vì hiển thị danh sách trống. *(Mới trong bản fork này.)*
- **Cooldown khi bị giới hạn tần suất** — khi server trả về 429, đồng hồ đếm ngược được hiển thị và nút "Tải thêm" được bật lại khi hết thời gian chờ. *(Mới trong bản fork này.)*
- **Nút thử lại** — khi tải thất bại, nay hiển thị dòng trạng thái riêng và nút thử lại, thay vì vô hiệu hóa nút kèm thông báo lỗi. *(Đã sửa trong bản fork này.)*

---

## Cài đặt & Cập nhật

### Yêu cầu

- Một trình quản lý userscript như **Tampermonkey**, **Violentmonkey** hoặc **Userscripts (Safari)**.
- Quyền truy cập tới `greasyfork.org` và `sleazyfork.org`.

### Cài đặt

1. Mở liên kết cài đặt trực tiếp trong trình duyệt:
   👉 [https://cdn.githubraw.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js](https://cdn.githubraw.com/CheemsGalaxy/Script-Finder-Modified-by-CheemsGalaxy/main/Script_Finder_Modified_by_CheemsGalaxy.user.js)
2. Trình quản lý userscript sẽ nhận diện tệp `.user.js` và hỏi bạn có cài đặt không.
3. Chấp nhận các quyền được yêu cầu (`GM_xmlhttpRequest`, `GM_getValue`, `GM_setValue`, `GM_registerMenuCommand`, `GM_openInTab`).
4. Tải lại bất kỳ trang nào và tìm tab **Script Finder+** ở mép phải màn hình.

### Cập nhật

- Nếu trình quản lý userscript hỗ trợ `@updateURL` / `@downloadURL`, bản cập nhật sẽ được tự động đề xuất.
- Nếu không, hãy mở lại liên kết cài đặt ở trên — trình quản lý sẽ thay thế phiên bản cũ.
- Khi nâng cấp từ bản Script Finder+ gốc, trình quản lý có thể yêu cầu **thêm quyền** cho các API `GM_*` mới. Hãy chấp nhận để bật ghi nhớ vị trí, menu lệnh và mở tab.

### Gỡ cài đặt / Quay về bản gốc

- Tắt hoặc xóa script khỏi trình quản lý userscript.
- Nếu muốn quay về bản gốc, hãy cài lại từ [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder).

> **Mẹo:** Nếu panel không hiện trên một website cụ thể, mở DevTools → Console và tìm các thông báo `[Script Finder+]` (ví dụ lỗi Trusted Types hoặc giới hạn tần suất) để hiểu lý do.

---

## Ghi công

Bản chỉnh sửa này sẽ không thể tồn tại nếu không có công sức của các tác giả gốc:

- **Tác giả gốc:** [ChinaGodMan](https://github.com/ChinaGodMan/UserScripts) — người tạo và duy trì Script Finder+.
- **Đồng tác giả (bản gốc):** [shiquda](https://github.com/shiquda).
- **Dự án gốc:** [GreasyFork](https://greasyfork.org/en/scripts/498904-script-finder) · [GitHub](https://github.com/ChinaGodMan/UserScripts).
- **Giấy phép:** MIT — bản fork này giữ nguyên giấy phép MIT và toàn bộ ghi công tác giả gốc.

Xin chân thành cảm ơn cộng đồng Greasyfork và cộng đồng userscript đã kiểm thử, góp ý và truyền cảm hứng.

---

*Được duy trì bởi **CheemsGalaxy**. Hoan nghênh phản hồi và báo lỗi qua trang feedback trên GreasyFork được liên kết ở đầu tài liệu này.*
