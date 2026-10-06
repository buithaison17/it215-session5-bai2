# Bài 2: Tái cấu trúc lịch sử commit bằng Interactive Rebase

## 1. Mục tiêu

Thực hiện tái cấu trúc lịch sử commit bằng Git Interactive Rebase.

Các mục tiêu:

* Sử dụng `git rebase -i` để chỉnh sửa lịch sử commit cục bộ.
* Gộp nhiều commit nhỏ thành một commit có ý nghĩa bằng `squash`.
* Thay đổi thông điệp commit sau khi gộp.
* Xóa bỏ commit không cần thiết bằng `drop`.
* Tạo lịch sử Git sạch sẽ, dễ đọc và có ý nghĩa.

---

## 2. Bối cảnh

Nhánh làm việc hiện tại chứa 4 commit liên quan đến module Authentication.

Các commit ban đầu:

```text
Commit 1: feat: khoi tao module auth
Commit 2: fix typo
Commit 3: adds utility functions
Commit 4: add temp file for debug
```

Trong đó:

* Commit 1 là commit chính, tạo module `auth.js`.
* Commit 2 sửa một lỗi nhỏ trong `auth.js`.
* Commit 3 bổ sung các utility functions cho module.
* Commit 4 tạo file `temp.txt` chỉ dùng để debug và không cần thiết.

Yêu cầu là:

* Giữ commit 1.
* Gộp commit 2 vào commit 1 bằng `squash`.
* Gộp commit 3 vào commit 1 bằng `squash`.
* Xóa commit 4 bằng `drop`.
* Đổi message của commit sau khi gộp thành:

```text
feat: hoan thien module authentication
```

---

## 3. Chuẩn bị các commit

### Commit 1: Khởi tạo module auth

Tạo file `auth.js`.

Ví dụ:

```javascript
function login(username, password) {
    return username && password;
}

module.exports = {
    login
};
```

Commit:

```bash
git add auth.js
git commit -m "feat: khoi tao module auth"
```

---

### Commit 2: Sửa typo

Tiến hành chỉnh sửa một lỗi nhỏ trong `auth.js`.

Commit:

```bash
git add auth.js
git commit -m "fix typo"
```

---

### Commit 3: Thêm utility functions

Tiếp tục chỉnh sửa `auth.js` và bổ sung utility function.

Ví dụ:

```javascript
function login(username, password) {
    return username && password;
}

function isAuthenticated(user) {
    return !!user;
}

module.exports = {
    login,
    isAuthenticated
};
```

Commit:

```bash
git add auth.js
git commit -m "adds utility functions"
```

---

### Commit 4: Tạo file tạm

Tạo file:

```text
temp.txt
```

Có thể thêm nội dung:

```text
temporary debug file
```

Commit:

```bash
git add temp.txt
git commit -m "add temp file for debug"
```

---

## 4. Kiểm tra lịch sử trước khi Interactive Rebase

Chạy:

```bash
git log --oneline
```

Lịch sử trước khi chỉnh sửa có dạng:

```text
<commit-4> add temp file for debug
<commit-3> adds utility functions
<commit-2> fix typo
<commit-1> feat: khoi tao module auth
```

Lúc này lịch sử vẫn còn các commit nhỏ và commit chứa file rác `temp.txt`.

---

# 5. Thực hiện Interactive Rebase

Vì cần chỉnh sửa 4 commit gần nhất, sử dụng:

```bash
git rebase -i HEAD~4
```

Git sẽ mở trình soạn thảo Interactive Rebase.

Ban đầu sẽ có dạng:

```text
pick <commit-1> feat: khoi tao module auth
pick <commit-2> fix typo
pick <commit-3> adds utility functions
pick <commit-4> add temp file for debug
```

---

## 6. Cấu hình Interactive Rebase

Thay đổi thành:

```text
pick <commit-1> feat: khoi tao module auth
squash <commit-2> fix typo
squash <commit-3> adds utility functions
drop <commit-4> add temp file for debug
```

Hoặc có thể sử dụng ký hiệu ngắn:

```text
pick <commit-1> feat: khoi tao module auth
s <commit-2> fix typo
s <commit-3> adds utility functions
d <commit-4> add temp file for debug
```

### Ý nghĩa

| Lệnh     | Commit                       | Ý nghĩa              |
| -------- | ---------------------------- | -------------------- |
| `pick`   | `feat: khoi tao module auth` | Giữ commit chính     |
| `squash` | `fix typo`                   | Gộp vào commit trước |
| `squash` | `adds utility functions`     | Gộp vào commit trước |
| `drop`   | `add temp file for debug`    | Xóa hoàn toàn commit |

---

## 7. Màn hình Interactive Rebase

Sau khi chỉnh sửa, giao diện cần thể hiện cấu hình tương tự:

```text
pick <commit-1> feat: khoi tao module auth
squash <commit-2> fix typo
squash <commit-3> adds utility functions
drop <commit-4> add temp file for debug
```

**Ảnh chụp màn hình cần nộp:**

> Chèn ảnh chụp giao diện Interactive Rebase tại đây.

![Interactive Rebase](interactive-rebase.png)

---

## 8. Đổi Commit Message sau khi Squash

Sau khi thực hiện `squash`, Git sẽ yêu cầu chỉnh sửa commit message.

Các message cũ có thể được hiển thị:

```text
# This is a combination of 3 commits.
# This is the 1st commit message:

feat: khoi tao module auth

# This is the 2nd commit message:

fix typo

# This is the 3rd commit message:

adds utility functions
```

Xóa các message cũ và thay bằng:

```text
feat: hoan thien module authentication
```

Lưu file và đóng trình soạn thảo.

---

## 9. Kết quả sau Interactive Rebase

Sau khi rebase hoàn tất, kiểm tra:

```bash
git status
```

Kết quả mong muốn:

```text
On branch <branch-name>
nothing to commit, working tree clean
```

Kiểm tra file:

```bash
ls
```

File `temp.txt` không còn tồn tại vì commit chứa file này đã được `drop`.

---

## 10. Kiểm tra lịch sử commit

Chạy:

```bash
git log --oneline
```

Kết quả mong đợi:

```text
<new-commit> feat: hoan thien module authentication
<commit-truoc-do> ...
```

Ba commit:

```text
feat: khoi tao module auth
fix typo
adds utility functions
```

đã được gộp thành một commit duy nhất:

```text
feat: hoan thien module authentication
```

Commit:

```text
add temp file for debug
```

đã được xóa hoàn toàn khỏi lịch sử của 4 commit đang xử lý.

---

## 11. Kiểm tra nội dung file

Sau khi rebase, module `auth.js` vẫn chứa các chức năng cần thiết.

Ví dụ:

```javascript
function login(username, password) {
    return username && password;
}

function isAuthenticated(user) {
    return !!user;
}

module.exports = {
    login,
    isAuthenticated
};
```

Trong khi đó:

```text
temp.txt
```

đã được loại bỏ.

---

## 12. Ảnh chụp kết quả

### 12.1. Interactive Rebase

Chèn ảnh chụp màn hình giao diện cấu hình:

```text
pick
squash
squash
drop
```

Tại đây:

![Ảnh Interactive Rebase](interactive-rebase.png)

---

### 12.2. Git Log sau khi hoàn tất

Chạy:

```bash
git log --oneline
```

Chèn ảnh chụp màn hình kết quả tại đây:

![Git Log sau Interactive Rebase](git-log-after-rebase.png)

Kết quả cần thể hiện commit:

```text
feat: hoan thien module authentication
```

và không còn các commit:

```text
fix typo
adds utility functions
add temp file for debug
```

---

## 13. Tổng kết

Qua bài thực hành, em đã sử dụng thành công Git Interactive Rebase để tái cấu trúc lịch sử commit.

Quy trình thực hiện:

1. Tạo 4 commit ban đầu.
2. Kiểm tra lịch sử bằng `git log --oneline`.
3. Chạy:

```bash
git rebase -i HEAD~4
```

4. Cấu hình Interactive Rebase:

```text
pick
squash
squash
drop
```

5. Gộp 3 commit liên quan đến module Authentication.
6. Đổi commit message thành:

```text
feat: hoan thien module authentication
```

7. Xóa commit chứa file `temp.txt`.
8. Kiểm tra lại lịch sử bằng:

```bash
git log --oneline
```

Kết quả cuối cùng là lịch sử commit được làm sạch, trong đó các thay đổi liên quan đến Authentication được đại diện bởi một commit duy nhất và commit thử nghiệm không cần thiết đã được loại bỏ.
