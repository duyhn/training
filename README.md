# Hướng dẫn Git cho người mới bắt đầu

Git là hệ thống quản lý phiên bản phổ biến: giúp bạn theo dõi thay đổi mã nguồn, làm việc nhóm và quay lại phiên bản cũ khi cần.

## Cài đặt và cấu hình lần đầu

1. **Cài Git**: tải từ [git-scm.com](https://git-scm.com/downloads) (Windows/macOS/Linux).

2. **Khai báo tên và email** (dùng cho mỗi commit):

```bash
git config --global user.name "Tên của bạn"
git config --global user.email "email@example.com"
```

3. **Kiểm tra cấu hình**:

```bash
git config --list
```

## Hai cách bắt đầu với một kho mã

### Tạo kho mới trên máy

```bash
cd thu-muc-du-an
git init
```

Lệnh `git init` tạo thư mục ẩn `.git` — từ đó thư mục được Git theo dõi.

### Sao chép kho có sẵn (GitHub, GitLab, …)

```bash
git clone https://github.com/to-chu/ten-repo.git
cd ten-repo
```

## Chu trình làm việc cơ bản

| Bước | Lệnh | Ý nghĩa |
|------|------|---------|
| Xem trạng thái | `git status` | File nào đã sửa, đã staged hay chưa |
| Thêm file vào vùng chờ commit | `git add ten-file` hoặc `git add .` | Chuẩn bị đưa thay đổi vào commit |
| Ghi lại phiên bản | `git commit -m "Mô tả ngắn"` | Lưu snapshot có thông điệp |
| Gửi lên máy chủ | `git push` | Đẩy commit lên remote (sau khi đã liên kết) |
| Lấy thay đổi từ máy chủ | `git pull` | Tải và hợp nhất commit mới nhất |

Thứ tự thường dùng: **sửa file → `git add` → `git commit` → `git push`**.

## Nhánh (branch)

Nhánh cho phép phát triển tính năng song song mà không làm hỏng nhánh chính (thường là `main` hoặc `master`).

```bash
git branch              # Liệt kê nhánh
git branch ten-nhanh    # Tạo nhánh mới
git checkout ten-nhanh  # Chuyển sang nhánh (cách cũ)
git switch ten-nhanh    # Chuyển nhánh (Git mới khuyên dùng)
git switch -c ten-nhanh # Tạo và chuyển sang nhánh mới
```

Sau khi xong việc trên nhánh phụ, thường sẽ **merge** vào nhánh chính trên GitHub/GitLab (qua Pull Request/Merge Request) hoặc merge trực tiếp bằng Git.

## Remote (máy chủ từ xa)

```bash
git remote -v                    # Xem địa chỉ remote
git remote add origin URL.git    # Thêm remote tên origin
git push -u origin main          # Lần đầu đẩy nhánh main và gán upstream
```

- **origin**: tên mặc định thường dùng cho remote chính (ví dụ GitHub).

## Xem lịch sử và so sánh

```bash
git log --oneline    # Lịch sử commit gọn
git diff             # Thay đổi chưa staged
git diff --staged    # Thay đổi đã add, chưa commit
```

## Một số tình huống thường gặp

- **Bỏ thay đổi chưa commit trên một file** (cẩn thận, mất nội dung chưa commit):

  ```bash
  git checkout -- ten-file
  # hoặc (Git 2.23+)
  git restore ten-file
  ```

- **Sửa thông điệp commit vừa rồi** (chưa push hoặc chỉ bạn dùng nhánh):

  ```bash
  git commit --amend -m "Thông điệp mới"
  ```

- **Tạm cất việc đang làm để chuyển nhánh**:

  ```bash
  git stash
  git switch nhanh-khac
  # ... làm việc ...
  git switch lai-nhanh-cu
  git stash pop
  ```

## Thói quen nên có

- Commit nhỏ, thường xuyên; thông điệm commit rõ ràng (làm gì, vì sao).
- `git pull` trước khi `git push` nếu làm việc nhóm trên cùng nhánh.
- Đọc kỹ thông báo lỗi Git — thường gợi ý lệnh xử lý.
- Không commit mật khẩu, khóa API; dùng biến môi trường hoặc file `.env` đã liệt kê trong `.gitignore`.

## Hướng dẫn SourceTree

[SourceTree](https://www.sourcetreeapp.com/) là ứng dụng **miễn phí** của Atlassian để làm việc với Git (và Mercurial) qua giao diện đồ họa. Trên Windows và macOS, SourceTree giúp bạn thực hiện các thao tác giống phần dòng lệnh ở trên mà không cần nhớ hết cú pháp.

### Cài đặt và lần chạy đầu

1. Tải SourceTree từ trang chính thức: [sourcetreeapp.com](https://www.sourcetreeapp.com/).
2. **Git vẫn cần có trên máy** — SourceTree dùng Git bên dưới. Trên Windows, trình cài đặt thường gợi ý cài Git Embedded hoặc trỏ tới Git đã cài; trên macOS nên cài [Xcode Command Line Tools](https://developer.apple.com/download/all/) hoặc Git từ [git-scm.com](https://git-scm.com/downloads) trước.
3. Lần mở đầu, SourceTree có thể yêu cầu **đăng nhập tài khoản Atlassian/Bitbucket** hoặc đăng ký — làm theo hướng dẫn trên màn hình để hoàn tất thiết lập.
4. Cấu hình **user.name** và **user.email** (phần *Cài đặt và cấu hình lần đầu* ở trên) vẫn áp dụng cho mọi commit, kể cả khi commit từ SourceTree.

### Mở hoặc tạo kho trong SourceTree

- **Clone** (tương đương `git clone`): `File` → `Clone` — dán URL (HTTPS hoặc SSH), chọn thư mục đích, bấm **Clone**.
- **Thêm kho đã có trên máy** (đã `git init` hoặc đã clone trước đó): `File` → `Add` — chọn thư mục chứa `.git`.
- **Tạo kho mới**: `File` → `New` / `Create` (tùy phiên bản) — chọn thư mục trống hoặc thư mục dự án để khởi tạo Git tại đó.

Sau khi thêm, kho xuất hiện trong **Bookmarks**; bấm vào để vào màn hình làm việc chính.

### Giao diện làm việc hằng ngày

| Việc cần làm | Trong SourceTree (gợi ý vị trí) | Tương đương lệnh |
|--------------|----------------------------------|------------------|
| Xem file đã sửa | Tab **File status** (hoặc **Working Copy**) — danh sách *Unstaged files* | `git status` |
| Đưa file vào vùng chờ commit | Chọn file → **Stage** (hoặc kéo lên *Staged files*) | `git add` |
| Ghi commit | Ô thông điệm commit → nút **Commit** | `git commit` |
| Gửi lên remote | Nút **Push** trên thanh công cụ | `git push` |
| Lấy thay đổi từ remote | **Pull** | `git pull` |
| Chỉ tải, chưa hợp nhất | **Fetch** | `git fetch` |

Luồng quen thuộc: sửa file trong editor → quay lại SourceTree → **Stage** các file cần gói vào một commit → nhập mô tả → **Commit** → **Push** khi muốn đồng bộ lên máy chủ.

### Nhánh (branch)

- Cây **BRANCHES** ở cột bên trái liệt kê nhánh local và remote-tracking.
- **Tạo nhánh mới**: bấm chuột phải lên nhánh gốc (ví dụ `main`) → **Branch** — đặt tên → có thể chọn “Checkout new branch”.
- **Chuyển nhánh**: double-click tên nhánh hoặc chuột phải → **Checkout**.
- **Merge**: checkout nhánh đích (thường là `main`) → chuột phải nhánh cần gộp → **Merge … into current branch**.

Khi có **xung đột (conflict)**, SourceTree đánh dấu file conflict; mở file hoặc dùng **Resolve conflicts** / công cụ merge tích hợp để chọn nội dung, sau đó đánh dấu đã xử lý và hoàn tất commit merge.

### Xem lịch sử và diff

- Tab **History** (hoặc **Log**) hiển thị đồ thị commit — bấm một commit để xem chi tiết và diff từng file.
- So sánh thay đổi chưa commit: trong **File status**, chọn file để xem diff ở khung bên dưới hoặc bên cạnh.

### Stash (tạm cất thay đổi)

- Trên thanh công cụ hoặc menu **Repository** → **Stash** — nhập mô tả ngắn nếu cần → **Stash** để cất toàn bộ thay đổi chưa commit và làm sạch working copy.
- **Apply** / **Pop** stash khi muốn lấy lại thay đổi (Pop thường xóa mục stash sau khi áp dụng).

### Mẹo cho người mới bắt đầu

- Nếu nút **Push** / **Pull** bị mờ hoặc báo lỗi, kiểm tra đã **Fetch** gần đây chưa và nhánh hiện tại đã gán **upstream** (track remote) chưa — có thể chỉnh trong `Repository` → **Repository settings** → **Remotes**.
- Đọc hộp thoại lỗi của SourceTree: thường trích nguyên nhân từ Git (xác thực, conflict, rejected push).
- Bạn có thể dùng **song song** terminal và SourceTree trên cùng một thư mục; chỉ tránh thao tác trùng lúc trên cùng một bước (ví dụ hai nơi cùng merge) để dễ theo dõi.

## Tài liệu tham khảo

- [Pro Git (tiếng Anh, miễn phí)](https://git-scm.com/book/en/v2)
- Trợ giúp nhanh trong terminal: `git help <ten-lenh>` (ví dụ `git help commit`).

---

Chúc bạn làm quen Git thuận lợi.
