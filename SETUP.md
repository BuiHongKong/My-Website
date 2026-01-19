# Hướng dẫn Setup: 2 Repository Architecture

## 📋 Tổng quan

Setup 2 repository:
- **Source/Staging Repository** (repo hiện tại): Nơi edit code, auto deploy staging → URL: `https://[username].github.io/[repo]/`
- **Production Repository**: Manual deploy → URL: `https://[username].github.io/[repo-production]/`

---

## 🏗️ Kiến trúc

```
Source/Staging Repo (hiện tại)
  ├─ Edit code ở đây
  ├─ Push code → Auto deploy staging vào chính repo này
  └─> Manual deploy → Production Repo
```

---

## 🚀 Bước 1: Enable GitHub Pages cho Repo hiện tại (Staging)

1. Vào repository hiện tại → **Settings** → **Pages**
2. Source: Chọn **"GitHub Actions"**
3. Click **Save**

**Lưu ý:** Repo hiện tại sẽ là staging environment.

---

## 🚀 Bước 2: Tạo Production Repository

1. Trên GitHub, tạo repository mới:
   - Tên: `[repo-name]-production` (ví dụ: `my-website-production`)
   - Public hoặc Private (nếu có GitHub Pro)
   - **KHÔNG** cần khởi tạo với README

2. Enable GitHub Pages cho repo-production:
   - Vào repo-production → **Settings** → **Pages**
   - Source: Chọn **"Deploy from a branch"**
   - Branch: `gh-pages`, folder: `/root`
   - Click **Save**

---

## 🔐 Bước 3: Tạo Personal Access Token (PAT)

Để workflow có thể deploy vào production repository:

1. GitHub → Click avatar → **Settings**
2. Scroll xuống → **Developer settings** (cuối sidebar trái)
3. **Personal access tokens** → **Tokens (classic)**
4. Click **"Generate new token"** → **"Generate new token (classic)"**
5. Đặt tên: `production-deploy-token`
6. Chọn scopes:
   - ✅ `repo` (Full control of private repositories)
   - ✅ `workflow` (Update GitHub Action workflows)
7. Click **"Generate token"**
8. **Copy token ngay** (chỉ hiện 1 lần!)

---

## 🔑 Bước 4: Thêm Secrets vào Source Repository

1. Vào **Source Repository** (repo hiện tại)
2. **Settings** → **Secrets and variables** → **Actions**
3. Click **"New repository secret"**

### Secret 1: PRODUCTION_REPO_TOKEN (BẮT BUỘC)
- Name: `PRODUCTION_REPO_TOKEN` (chính xác, viết hoa)
- Value: Paste token vừa tạo
- Click **"Add secret"**
- **⚠️ QUAN TRỌNG:** Secret này là BẮT BUỘC. Workflow sẽ fail nếu thiếu token này.

### Secret 2: PRODUCTION_REPO (Optional)
- Name: `PRODUCTION_REPO` (chính xác, viết hoa)
- Value: `[username]/[repo-production]` (ví dụ: `john-doe/my-website-production`)
- Click **"Add secret"**
- **Lưu ý:** Nếu không thêm, workflow tự động dùng `[repo-name]-production`

---

## 📝 Bước 5: Test Workflow

### Test Staging (Auto):

1. Push code vào source repo:
   ```bash
   git add .
   git commit -m "Test staging deploy"
   git push origin main
   ```

2. Vào **Actions** → Xem workflow chạy
3. Đợi workflow hoàn thành
4. Kiểm tra staging URL (chính repo hiện tại):
   ```
   https://[username].github.io/[repo-name]/
   ```

### Test Production (Manual):

1. Vào **Actions** → Chọn workflow **"Deploy to Environment"**
2. Click **"Run workflow"** → Chọn branch `main`
3. Click **"Run workflow"**
4. Đợi workflow hoàn thành
5. Kiểm tra production URL:
   ```
   https://[username].github.io/[repo-production]/
   ```

---

## 🔍 Cách hoạt động

### Staging Flow:

```
Edit code trong Source Repo
  └─> Push code
      └─> Workflow tự động chạy
          └─> Deploy vào chính Source Repo (GitHub Pages)
              └─> Staging URL: https://[username].github.io/[repo-name]/
```

### Production Flow:

```
Staging OK → Vào Actions → Run workflow (manual)
  └─> Deploy vào Production Repo (gh-pages branch)
      └─> Production URL: https://[username].github.io/[repo-production]/
```

---

## 📊 URLs

Sau khi setup xong, bạn sẽ có 2 repositories:

1. **Source/Staging Repo**: `https://[username].github.io/[repo-name]/`
   - Nơi edit code
   - Auto deploy staging khi push
   - Staging URL

2. **Production Repo**: `https://[username].github.io/[repo-production]/`
   - Manual deploy
   - Phục vụ người dùng thật
   - Production URL

---

## ❓ Troubleshooting

### 1. Staging không deploy được?

**Kiểm tra:**
- Settings → Pages → Source phải là "GitHub Actions"
- Workflow permissions: Read and write permissions
- Xem logs của workflow để biết lỗi cụ thể

### 2. Production không deploy được?

**Kiểm tra:**
- **Secret `PRODUCTION_REPO_TOKEN` đã thêm chưa?** (BẮT BUỘC)
- Token có quyền `repo` chưa?
- Repo-production đã enable GitHub Pages chưa?
- Xem logs của workflow để biết lỗi cụ thể

**Lỗi thường gặp:**
- `Action failed with "The generated GITHUB_TOKEN does not support to push to an external repository"` → Thiếu `PRODUCTION_REPO_TOKEN` secret
- `Cannot access repository` → Token không có quyền hoặc repo không tồn tại

### 3. Token không hoạt động?

**Giải pháp:**
- Tạo token mới với đầy đủ quyền `repo` (full control)
- Đảm bảo token chưa hết hạn
- Kiểm tra secret name: `PRODUCTION_REPO_TOKEN` (chính xác, viết hoa)
- Token phải được thêm vào **Source Repository** (không phải production repo)
- Workflow sẽ tự động check token ở step "Check Production Token" - nếu thiếu sẽ fail ngay

### 4. Repo-production không tìm thấy?

**Giải pháp:**
- Thêm secret `PRODUCTION_REPO` với format: `username/repo-production` (ví dụ: `BuiHongKong/CI-CD-staging-production`)
- Hoặc đảm bảo repo có tên đúng: `[repo-name]-production`
- **Lưu ý:** Format phải chính xác: `username/repo-name` (có dấu `/`, không có khoảng trắng)
- Username và repo-name phải đúng chữ hoa/chữ thường

---

## ✅ Checklist

- [ ] Đã enable GitHub Pages cho repo hiện tại (Source: GitHub Actions)
- [ ] Đã tạo repo-production với tên: `[repo-name]-production`
- [ ] Đã enable GitHub Pages cho repo-production (Deploy from branch: gh-pages)
- [ ] Đã tạo Personal Access Token với quyền `repo` (full control)
- [ ] **Đã thêm secret `PRODUCTION_REPO_TOKEN` vào source repo** (BẮT BUỘC)
- [ ] Đã thêm secret `PRODUCTION_REPO` (optional, nếu repo tên khác)
- [ ] Đã test staging deploy (push code)
- [ ] Đã test production deploy (manual)

---

## 🎯 Kết quả

Sau khi setup xong:
- ✅ 2 repositories (source/staging + production)
- ✅ 2 URLs riêng (staging + production)
- ✅ Staging auto deploy vào repo hiện tại
- ✅ Production manual deploy vào repo-production
- ✅ Đơn giản, dễ quản lý

**Chúc bạn setup thành công! 🎉**
