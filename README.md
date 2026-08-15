# 📘 BPURD GitHub Organization & Git Workflow Guide

Tài liệu này hướng dẫn cách tổ chức không gian làm việc trên GitHub Organization của dự án **BeniotPhan Ultimate Robot Dog (BPURD)** và các quy chuẩn khi làm việc với mã nguồn (code), tài liệu.

---

## 🏢 1. Cách hoạt động và Thiết lập Organization

Khác với tài khoản cá nhân, **GitHub Organization** là một "công ty ảo". Nó sở hữu các Repository, và bạn (Ben) là Owner (Chủ sở hữu). Khi dự án mở rộng, bạn có thể mời thêm người vào làm việc mà không sợ họ can thiệp sai quyền hạn.

### Những thiết lập (Settings) tiếp theo bạn nên làm:
1. **Tạo Teams (Nhóm):** 
   - Vào tab **Teams** > *New Team*.
   - Tạo các nhóm như `Hardware-Engineers` (cho team vẽ KiCad/Cơ khí) và `Firmware-Devs` (cho team STM32/Python). Sau này add thành viên vào đúng nhóm để cấp quyền truy cập repository tương ứng.
2. **Bật Branch Protection (Bảo vệ nhánh chính):**
   - Vào **Settings** của từng repository > **Branches** > *Add branch protection rule*.
   - Gõ `main` vào ô *Branch name pattern*.
   - Tích chọn **Require a pull request before merging**. Điều này ngăn chặn việc bạn (hoặc ai đó) vô tình push thẳng code lỗi lên nhánh chính làm hỏng toàn bộ hệ thống robot.
3. **Sử dụng GitHub Projects (Kanban Board):**
   - Vào tab **Projects** của Organization > *New Project*.
   - Tạo các cột: `To Do` (Cần làm), `In Progress` (Đang code/Đang vẽ mạch), `Testing` (Đang nạp thử vào mạch thực tế), `Done` (Hoàn thành).

---

## 🔄 2. Quy trình làm việc chuẩn với Git (Git Workflow)

Tuyệt đối **KHÔNG** code và push thẳng lên nhánh `main`. Hãy áp dụng quy trình **Feature Branch Workflow** theo các bước sau:

### Bước 1: Lấy code mới nhất về máy
Trước khi bắt đầu làm việc, luôn đảm bảo code trên máy bạn là mới nhất:
```bash
git checkout main
git pull origin main
```

### Bước 2: Tạo nhánh mới (Branch) cho tính năng/sửa lỗi
Mỗi khi làm một task mới (ví dụ: viết code điều khiển chân chó máy), hãy tạo một nhánh mới từ `main`:
```bash
# Cú pháp: git checkout -b loại-nhánh/tên-ngắn-gọn
git checkout -b feature/leg-kinematics
# Hoặc
git checkout -b fix/stm32-timer-bug
```

### Bước 3: Code và Lưu lại (Commit)
Sau khi viết code hoặc sửa file xong, bạn cần đưa nó vào "giỏ hàng" và ghi chú lại:
```bash
# Thêm các file đã thay đổi
git add .

# Ghi chú (Commit) những gì đã làm
git commit -m "feat: thêm công thức động học nghịch cho module Python"
```

### Bước 4: Đẩy nhánh lên GitHub (Push)
```bash
git push origin feature/leg-kinematics
```

### Bước 5: Tạo Pull Request (PR)
Lên giao diện web của GitHub, bạn sẽ thấy nút **Compare & pull request**. Nhấn vào đó để yêu cầu ghép (merge) code từ nhánh `feature/leg-kinematics` vào nhánh `main`.

Đây là lúc bạn có thể xem lại code của mình có lỗi không, hoặc nhờ người khác review.

Khi mọi thứ hoàn hảo, nhấn **Merge pull request** để hoàn thành.

---

## 📝 3. Quy chuẩn viết Commit Message (Conventional Commits)
Để nhìn vào lịch sử code biết ngay file nào làm chức năng gì (rất quan trọng khi code nhúng hoặc debug phần cứng), hãy viết commit theo cú pháp:

`mã_loại: mô tả ngắn gọn`

Các mã loại (Type) phổ biến:
- **feat:** Thêm tính năng mới (vd: `feat: add UART communication for IMU sensor`)
- **fix:** Sửa lỗi (vd: `fix: resolve motor jitter issue in PID loop`)
- **docs:** Chỉ sửa file tài liệu, README (vd: `docs: update KiCad schematic PDF`)
- **refactor:** Tối ưu hóa lại code nhưng không làm đổi tính năng (vd: `refactor: clean up inverse kinematics math functions`)
- **chore:** Cập nhật cấu hình, thư viện (vd: `chore: update C++ compiler dependencies`)

---

## 💡 4. Các Mẹo (Tips) "Cứu mạng" khi dùng Git

Code lỡ bị hỏng nhưng chưa commit? Muốn quay về trạng thái sạch sẽ trước đó:
```bash
git checkout .
```

Xem mình đang ở nhánh nào và file nào bị đổi? (Nên dùng thường xuyên)
```bash
git status
```

Lỡ commit nhầm message? (Chỉ dùng khi chưa push lên GitHub)
```bash
git commit --amend -m "tin nhắn mới chuẩn hơn"
```

Cập nhật nhánh đang code với nhánh `main` mới nhất: (Khi có ai đó đã update `main` và bạn muốn lấy code đó vào nhánh của mình)
```bash
git checkout nhánh-của-bạn
git merge main
```

---
*Tài liệu này được duy trì bởi cấu trúc tổ chức của BPURD. Hãy đọc kỹ trước khi bắt đầu đóng góp mã nguồn hoặc tài liệu phần cứng!*

### Lời khuyên thêm cho bạn:
Với dự án chế tạo robot dog, file `.gitignore` là cực kỳ quan trọng. Khi bạn push code, hãy đảm bảo đã cấu hình `.gitignore` chuẩn để bỏ qua các file build tạm thời, file cấu hình IDE cá nhân, và các file rác không cần thiết để repo luôn sạch sẽ.
