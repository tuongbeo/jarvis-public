# Jarvis

Bộ 17 skill và công cụ MCP để điều phối công việc hằng ngày trên Slack: tiếp nhận việc, bản tin sáng, báo cáo tuần, soạn tài liệu yêu cầu (BRD, PRD, phân tích nhu cầu, kiến trúc giải pháp), rà soát chất lượng độc lập, đối soát trạng thái và tiếp tục công việc từ checkpoint. Repo này chỉ phát hành **gói cài đặt** (file zip trong mục [Releases](../../releases)) cho Claude và ChatGPT.

> **Cần có quyền truy cập máy chủ Jarvis.** Các skill gọi công cụ trên máy chủ `https://jarvis.lens.io.vn/mcp`. Máy chủ chỉ cho đăng nhập (OAuth) những email được chủ sở hữu cấp quyền. Chưa được cấp quyền thì cài gói xong vẫn không dùng được công cụ nào. Để xin quyền, liên hệ chủ sở hữu repo qua hồ sơ GitHub của họ; đừng đăng email của bạn lên Issues công khai.

## Gói gồm những gì

17 skill, mỗi skill là một thư mục dưới `skills/`:

| Skill | Dùng để |
|---|---|
| `slack-task-intake` | Biến tin nhắn hoặc thread Slack thành một đề xuất tiếp nhận việc đã chuẩn hóa |
| `command-gate` | Ghi biên nhận kỹ thuật cho vòng đời của một lệnh Slack (nhận, trùng, từ chối, hoàn tất) |
| `router-conversation` | Trả lời hội thoại ở chế độ chỉ đọc, nêu nguồn đã dùng và nguồn còn thiếu |
| `task-execution-orchestrator` | Điều phối thực thi một việc đã được ủy quyền, gồm Worker, QA độc lập và sửa có giới hạn |
| `task-handoff-preparation` | Soạn gói bàn giao có đầu vào, đích, định dạng đầu ra và tiêu chí chấp nhận rõ ràng |
| `task-quality-review` | Kiểm kết quả độc lập theo phạm vi, nguồn và tiêu chí chấp nhận |
| `task-result-reconciliation` | Đối soát khi trạng thái của một kết quả không khớp giữa các nơi |
| `task-checkpoint-resume` | Tiếp tục một việc ở phiên mới từ checkpoint đã lưu |
| `scheduled-task-governance` | Kiểm kê, đối soát và thay thế tác vụ định kỳ (Scheduled Task) |
| `personal-work-briefing` | Bản tin sáng các ngày làm việc từ danh sách việc, checkpoint và yêu cầu đăng ký |
| `weekly-work-report` | Báo cáo tuần riêng theo tuần ISO |
| `document-task-execution` | Tạo bản nháp hoặc sửa tại chỗ một tài liệu cho việc đã được ủy quyền |
| `business-requirements-document` | Viết hoặc sửa BRD có mã yêu cầu ổn định |
| `product-requirements-document` | Viết PRD có user story và tiêu chí chấp nhận |
| `customer-requirements-analysis` | Phân tích nhu cầu khách hàng thành báo cáo có sổ bằng chứng |
| `solution-architecture` | So sánh phương án, mô tả thành phần, luồng dữ liệu và truy vết nhu cầu |
| `project-offering-brief` | Bản tóm tắt đề xuất dự án ngắn gọn trước khi có hồ sơ đầy đủ |

## Tải về và kiểm tra

Tải `jarvis-<phiên bản>.zip` và `SHA256SUMS` từ bản phát hành mới nhất, rồi kiểm tra:

```bash
sha256sum -c SHA256SUMS
```

Lệnh phải báo `OK`. Khác thì đừng cài.

## Cài trên Claude

**Claude Desktop (Cowork).** Vào trang Plugins, chọn tải lên từ tệp, chọn `jarvis-<phiên bản>.zip`. Cài xong **không tự đăng nhập**: mở tab Connectors của plugin và kết nối Jarvis, đăng nhập bằng email đã được cấp quyền (theo tài liệu Claude về plugin trong Cowork).

**Claude Code.** Giải nén zip vào một thư mục, rồi chạy Claude Code với `--plugin-dir <thư mục>`. Máy chủ MCP của gói tự khởi động; đăng nhập bằng `/mcp` khi được hỏi.

Tình trạng kiểm thử: gói có cùng cấu trúc đã được cài thử trên Claude ngày 03/11/2026, skill và MCP hiện đủ và đăng nhập thành công. Cách `--plugin-dir` ở trên theo tài liệu Claude Code, **chưa được kiểm thử** với gói này.

## Cài trên ChatGPT (web)

**Phần này chưa được kiểm chứng đầy đủ.** Điều đã biết:

- ChatGPT đánh dấu **Desktop only** cho plugin nhập vào mà *khai báo* máy chủ MCP, và loại này không chạy trên web (tài liệu OpenAI về nhập plugin). Vì vậy gói này **không khai báo MCP** và không chứa id ứng dụng ChatGPT nào (id đó chỉ có nghĩa trong tài khoản đã tạo ra nó).
- Hệ quả: bạn tự tạo kết nối MCP, rồi nhập gói để lấy skill.

Các bước:

1. Trong ChatGPT, mở Plugins, bấm dấu cộng, chọn **Add custom MCP server**, nhập URL `https://jarvis.lens.io.vn/mcp`, chọn xác thực OAuth và hoàn tất đăng nhập. Gói tài khoản hoặc chính sách của workspace có thể không cho thêm máy chủ MCP tùy chỉnh.
2. Mở một chat mới, gọi **Plugin Creator** và gửi lời nhắc dưới đây kèm file zip:

```text
Use Plugin Creator. Create a new plugin named Jarvis from the attached ZIP; drop the wrapper folder if present and do not import PACKAGING.md. Do not modify anything under skills/. Do not declare any MCP server (no mcp.json, no .mcp.json).
```

3. Mở chat mới, gõ `@` và chọn plugin Jarvis, rồi hỏi một việc chỉ đọc để thử.

Chưa kiểm chứng: gói Free có thêm được máy chủ MCP tùy chỉnh không; skill của plugin có gọi được công cụ của kết nối ở bước 1 hay cần gắn thêm ứng dụng (khi đó dùng id ứng dụng của **chính tài khoản bạn**, không dùng id của người khác); và nhãn Desktop only có biến mất không. Nếu workspace của bạn có admin tắt plugin, bạn cần admin bật lại.

## Giới hạn đã biết

- Gói chỉ là phần skill và cấu hình kết nối. Nó **không** chứa máy chủ Jarvis.
- Quyền đăng nhập do chủ sở hữu cấp thủ công.
- Skill gọi Slack, danh sách việc và tài liệu của chính tổ chức bạn; bạn cần kết nối Slack và các nguồn đó trong Claude hoặc ChatGPT.

## Giấy phép

[PolyForm Internal Use License 1.0.0](LICENSE). Bạn được dùng cho hoạt động nội bộ của doanh nghiệp mình và sửa đổi để dùng nội bộ. **Không được phân phối lại** gói hay bản sửa đổi, và không được cấp phép lại cho người khác. Dùng cho mục đích không phải nội bộ doanh nghiệp (ví dụ cá nhân ngoài công việc) không được giấy phép này nêu rõ; hãy hỏi chủ sở hữu.

## Lịch sử thay đổi

### 0.3.2: bản công khai đầu tiên

- Gói công khai cho Claude và ChatGPT web: không kèm id ứng dụng ChatGPT, không khai báo MCP trong gói ChatGPT; máy chủ MCP cho Claude khai báo trong manifest của gói.
- Gỡ dữ liệu nội bộ khỏi nội dung skill: các mẫu tham chiếu của `project-offering-brief` viết lại ẩn danh; `$id` của JSON schema đổi sang `urn:jarvis:`.
- **Thay đổi hành vi:** bản tin sáng và báo cáo tuần không còn chấp nhận các dạng khóa cũ có tiền tố. Dùng `artifact:morning:<ngày>` và `delivery:morning:<ngày>:<nơi nhận>`, hoặc `artifact:weekly:<tuần>` và `delivery:weekly:<tuần>:<nơi nhận>`.
- 17 skill tăng một số `rc` trong phiên bản của chính skill.

### Trước 0.3.2

Không có bản tải công khai. Bản 0.3.1 (nội bộ) chỉ sửa mô tả của `personal-work-briefing` và `weekly-work-report` để Claude nhận gói zip.
