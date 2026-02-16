# Modul 3: Git dan GitHub untuk Pemula

## 🎯 Tujuan Pembelajaran

Setelah modul ini, peserta dapat:
- Memahami konsep version control dengan Git
- Menggunakan Git commands dasar
- Bekerja dengan GitHub (remote repository)
- Menggunakan GUI Git seperti Sourcetree
- Melakukan collaboration dengan Git
- Menerapkan Git workflow yang baik

## ⏱️ Durasi: 2-3 Sesi

## 📝 Catatan Penting

**Modul Git dipelajari setelah TypeScript dan sebelum Express.js** agar peserta bisa menggunakan version control sejak awal saat belajar Express dan REST API. Ini akan membantu mereka:
- Track semua perubahan kode selama pembelajaran
- Membiasakan diri dengan Git workflow
- Siap untuk collaboration di project nyata

---

## 📚 Materi 6.1: Pengenalan Git

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Git adalah version control system
- Git membantu track perubahan kode
- Git memungkinkan collaboration
- Git berbeda dengan GitHub (Git adalah tool, GitHub adalah platform)
- Git sangat penting untuk development modern

### Materi Pembelajaran

#### 1. Apa itu Git?

**Git** adalah sistem kontrol versi (version control system) yang digunakan untuk:
- Melacak perubahan kode
- Bekerja sama dalam tim
- Mengembalikan ke versi sebelumnya jika ada masalah
- Membuat branch untuk fitur baru tanpa mengganggu kode utama

**Analoginya:**
Bayangkan Git seperti "save game" di video game:
- Setiap kali Anda save, Anda bisa kembali ke titik tersebut
- Jika ada bug, Anda bisa kembali ke versi yang bekerja
- Anda bisa mencoba fitur baru tanpa takut merusak kode yang sudah bekerja

#### 2. Mengapa Git Penting?

**Tanpa Git:**
```
project-v1.js
project-v2.js
project-v2-backup.js
project-v2-final.js
project-v2-final-really.js
project-v3.js
...
```
😱 Kacau! Tidak tahu mana versi terbaru, mana yang bekerja.

**Dengan Git:**
```
project/
├── (semua file di satu tempat)
└── Git track semua perubahan
```
✅ Rapi! Bisa lihat history, kembali ke versi sebelumnya, dll.

#### 3. Konsep Dasar Git

**Repository (Repo):**
- Folder project yang di-track oleh Git
- Berisi semua file dan history perubahan

**Commit:**
- "Snapshot" dari kode di titik waktu tertentu
- Seperti "save point" di game
- Setiap commit punya pesan yang menjelaskan perubahan

**Branch:**
- Versi paralel dari kode
- Bisa membuat fitur baru di branch terpisah
- Tidak mengganggu kode utama (main/master branch)

**Working Directory:**
- Folder tempat Anda bekerja
- File yang sedang Anda edit

**Staging Area:**
- Area persiapan sebelum commit
- File yang sudah siap untuk di-commit

**Repository:**
- Tempat menyimpan semua commit dan history

#### 4. Install Git

**Windows:**
```bash
# Download dari: https://git-scm.com/download/win
# Install dengan default settings
```

**Mac:**
```bash
# Install via Homebrew
brew install git

# Atau download dari: https://git-scm.com/download/mac
```

**Linux:**
```bash
# Ubuntu/Debian
sudo apt-get install git

# Fedora
sudo dnf install git
```

**Verifikasi Install:**
```bash
git --version
# Output: git version 2.x.x
```

#### 5. Konfigurasi Git Pertama Kali

```bash
# Set nama Anda
git config --global user.name "Nama Anda"

# Set email Anda
git config --global user.email "email@example.com"

# Set default editor (optional)
git config --global core.editor "code --wait"  # VS Code
# atau
git config --global core.editor "nano"  # Nano editor

# Verifikasi konfigurasi
git config --list
```

### Latihan 6.1

```bash
# Setup Git di komputer Anda:
# 1. Install Git (jika belum)
# 2. Konfigurasi nama dan email
# 3. Verifikasi dengan git --version
# 4. Lihat konfigurasi dengan git config --list

# Catat hasilnya:
```

---

## 📚 Materi 6.2: Git Commands Dasar

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Workflow Git: modify → stage → commit
- Commands yang paling sering digunakan
- Status file: untracked, modified, staged
- Commit message yang baik
- Jangan terlalu banyak commands sekaligus

### Materi Pembelajaran

#### 1. Inisialisasi Repository

```bash
# Buat folder project baru
mkdir my-project
cd my-project

# Inisialisasi Git repository
git init

# Output: Initialized empty Git repository in .../my-project/.git
```

**Apa yang terjadi?**
- Git membuat folder `.git` (hidden folder)
- Folder ini berisi semua informasi Git
- Jangan edit folder ini secara manual!

#### 2. Status Repository

```bash
# Lihat status repository
git status

# Output contoh:
# On branch main
# No commits yet
# nothing to commit (create/copy files and use "git add" to track)
```

**Status File:**
- **Untracked**: File baru yang belum di-track Git
- **Modified**: File yang sudah diubah
- **Staged**: File yang siap untuk di-commit

#### 3. Menambahkan File ke Staging Area

```bash
# Tambahkan satu file
git add file.js

# Tambahkan semua file
git add .

# Tambahkan file tertentu dengan pattern
git add *.js
git add src/

# Lihat status setelah add
git status
```

**Contoh Workflow:**
```bash
# 1. Buat file baru
echo "console.log('Hello');" > app.js

# 2. Lihat status
git status
# Output: app.js adalah "Untracked"

# 3. Tambahkan ke staging
git add app.js

# 4. Lihat status lagi
git status
# Output: app.js adalah "Changes to be committed"
```

#### 4. Commit Perubahan

```bash
# Commit dengan pesan
git commit -m "Pesan commit yang menjelaskan perubahan"

# Contoh:
git commit -m "Add initial app.js file"
git commit -m "Fix bug in user authentication"
git commit -m "Add user registration feature"
```

**Commit Message yang Baik:**
```bash
# ✅ Baik - jelas dan deskriptif
git commit -m "Add user login functionality"
git commit -m "Fix validation error in registration form"
git commit -m "Update README with setup instructions"

# ❌ Buruk - tidak jelas
git commit -m "update"
git commit -m "fix"
git commit -m "changes"
```

**Commit Message Best Practices:**
- Gunakan present tense: "Add" bukan "Added"
- Jelas dan spesifik
- Maksimal 50 karakter untuk subject
- Bisa tambahkan body untuk penjelasan lebih detail

#### 5. Melihat History

```bash
# Lihat semua commit
git log

# Lihat commit dengan format yang lebih ringkas
git log --oneline

# Lihat commit dengan grafik
git log --oneline --graph

# Lihat perubahan di commit tertentu
git show <commit-hash>
```

**Contoh Output:**
```
commit abc123def456 (HEAD -> main)
Author: Nama Anda <email@example.com>
Date:   Mon Feb 16 2024

    Add user login functionality

commit def456ghi789
Author: Nama Anda <email@example.com>
Date:   Sun Feb 15 2024

    Initial commit
```

#### 6. Workflow Lengkap

```bash
# 1. Buat/edit file
echo "const name = 'John';" > user.js

# 2. Lihat status
git status
# Output: user.js adalah "Untracked"

# 3. Tambahkan ke staging
git add user.js

# 4. Lihat status lagi
git status
# Output: user.js adalah "Changes to be committed"

# 5. Commit
git commit -m "Add user.js file"

# 6. Lihat history
git log --oneline
```

#### 7. Membatalkan Perubahan

```bash
# Batalkan perubahan di file yang belum di-add
git restore file.js

# Atau (cara lama)
git checkout -- file.js

# Batalkan file dari staging area (tapi tetap simpan perubahan)
git restore --staged file.js

# Atau
git reset HEAD file.js
```

**Contoh:**
```bash
# Edit file
echo "wrong content" > app.js

# Tambahkan ke staging (salah)
git add app.js

# Batalkan dari staging (tapi file masih berubah)
git restore --staged app.js

# Batalkan perubahan file
git restore app.js
```

### Latihan 6.2

```bash
# Praktikkan workflow Git:
# 1. Buat folder project baru
# 2. Inisialisasi Git repository
# 3. Buat beberapa file (misalnya: index.js, utils.js, README.md)
# 4. Tambahkan file ke staging area
# 5. Commit dengan pesan yang jelas
# 6. Edit salah satu file
# 7. Lihat status
# 8. Commit perubahan lagi
# 9. Lihat history dengan git log --oneline

# Catat setiap langkah dan hasilnya:
```

---

## 📚 Materi 6.3: Branch dan Merge

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Branch untuk mengisolasi fitur baru
- Main/master branch adalah kode utama yang stabil
- Feature branch untuk development
- Merge untuk menggabungkan branch
- Conflict resolution (advanced, bisa skip jika waktu terbatas)

### Materi Pembelajaran

#### 1. Konsep Branch

**Branch** adalah versi paralel dari kode Anda. Bayangkan seperti:
- **Main branch**: Jalan raya utama (kode yang stabil)
- **Feature branch**: Jalan kecil untuk fitur baru (development)

**Mengapa Branch Penting?**
- Bisa develop fitur baru tanpa mengganggu kode utama
- Bisa test fitur baru tanpa takut merusak yang sudah bekerja
- Bisa rollback dengan mudah jika ada masalah

#### 2. Melihat dan Membuat Branch

```bash
# Lihat semua branch
git branch

# Lihat branch dengan detail
git branch -v

# Buat branch baru
git branch nama-branch

# Buat dan pindah ke branch baru
git checkout -b nama-branch

# Atau (cara baru)
git switch -c nama-branch

# Pindah ke branch lain
git checkout nama-branch
# atau
git switch nama-branch
```

**Contoh:**
```bash
# Lihat branch saat ini
git branch
# Output: * main

# Buat branch untuk fitur baru
git checkout -b feature/user-login

# Lihat branch lagi
git branch
# Output:
#   main
# * feature/user-login  (asterisk = branch aktif)

# Buat perubahan di branch ini
echo "// login code" > login.js
git add login.js
git commit -m "Add login functionality"
```

#### 3. Merge Branch

```bash
# Pindah ke branch utama
git checkout main

# Merge branch feature ke main
git merge feature/user-login

# Setelah merge, bisa hapus branch (optional)
git branch -d feature/user-login
```

**Contoh Lengkap:**
```bash
# 1. Di main branch, buat file
echo "// main code" > app.js
git add app.js
git commit -m "Initial app.js"

# 2. Buat branch baru untuk fitur
git checkout -b feature/add-auth

# 3. Di feature branch, tambahkan fitur
echo "// auth code" > auth.js
git add auth.js
git commit -m "Add authentication"

# 4. Kembali ke main
git checkout main

# 5. Merge feature branch
git merge feature/add-auth

# 6. Lihat hasilnya
ls
# Output: app.js dan auth.js ada
```

#### 4. Merge Conflict (Advanced)

**Kapan terjadi conflict?**
- Dua branch mengubah baris yang sama
- Git tidak tahu mana yang benar

**Contoh Conflict:**
```bash
# Di main branch
echo "version 1" > config.js
git add config.js
git commit -m "Add config"

# Di feature branch
git checkout -b feature/new-config
echo "version 2" > config.js
git add config.js
git commit -m "Update config"

# Kembali ke main dan merge
git checkout main
git merge feature/new-config
# Output: CONFLICT!
```

**Menyelesaikan Conflict:**
```bash
# File yang conflict akan punya marker:
<<<<<<< HEAD
version 1
=======
version 2
>>>>>>> feature/new-config

# Edit file, pilih versi yang benar (atau gabungkan)
# Hapus marker conflict
# Save file

# Setelah resolve, add dan commit
git add config.js
git commit -m "Resolve merge conflict"
```

**Tips:**
- Conflict bisa menakutkan, tapi sebenarnya mudah diatasi
- Baca dengan teliti, pilih versi yang benar
- Atau gabungkan kedua perubahan jika perlu

### Latihan 6.3

```bash
# Praktikkan branch dan merge:
# 1. Buat branch baru: feature/add-calculator
# 2. Di branch tersebut, buat file calculator.js
# 3. Commit perubahan
# 4. Kembali ke main branch
# 5. Merge feature branch ke main
# 6. Lihat hasilnya
# 7. (Optional) Hapus feature branch

# Catat setiap langkah:
```

---

## 📚 Materi 6.4: GitHub (Remote Repository)

### Penjelasan untuk Instruktur

**Konsep Penting:**
- GitHub adalah platform untuk menyimpan Git repository
- Remote repository vs local repository
- Push untuk upload ke GitHub
- Pull untuk download dari GitHub
- Clone untuk copy repository dari GitHub
- Collaboration dengan GitHub

### Materi Pembelajaran

#### 1. Apa itu GitHub?

**GitHub** adalah platform cloud untuk menyimpan Git repository. Bayangkan:
- **Git**: Tool untuk version control (local)
- **GitHub**: Tempat menyimpan repository di cloud (remote)

**Keuntungan GitHub:**
- Backup kode Anda di cloud
- Collaboration dengan tim
- Portfolio untuk developer
- Open source projects
- Issue tracking, pull requests, dll

#### 2. Setup GitHub Account

1. Buka https://github.com
2. Sign up dengan email
3. Verifikasi email
4. Selesai!

#### 3. Membuat Repository di GitHub

1. Klik tombol **"New"** atau **"+"** → **"New repository"**
2. Isi nama repository (misalnya: `my-first-project`)
3. Pilih **Public** atau **Private**
4. **Jangan** centang "Initialize with README" (karena kita sudah punya repo local)
5. Klik **"Create repository"**

#### 4. Menghubungkan Local ke Remote

```bash
# Tambahkan remote repository
git remote add origin https://github.com/username/repo-name.git

# Lihat remote yang sudah ditambahkan
git remote -v

# Output:
# origin  https://github.com/username/repo-name.git (fetch)
# origin  https://github.com/username/repo-name.git (push)
```

**Catatan:**
- `origin` adalah nama default untuk remote repository
- Bisa pakai nama lain jika perlu
- URL bisa HTTPS atau SSH

#### 5. Push ke GitHub

```bash
# Push branch main ke GitHub
git push -u origin main

# Setelah pertama kali, cukup:
git push
```

**Apa yang terjadi?**
- Kode Anda di-upload ke GitHub
- Semua commit history ikut ter-upload
- Orang lain bisa lihat kode Anda (jika public)

**Contoh:**
```bash
# Pastikan sudah commit semua perubahan
git add .
git commit -m "Initial commit"

# Push ke GitHub
git push -u origin main

# Output:
# Enumerating objects: 5, done.
# Writing objects: 100% (5/5), done.
# To https://github.com/username/repo-name.git
#  * [new branch]      main -> main
```

#### 6. Clone Repository dari GitHub

```bash
# Clone repository (copy dari GitHub ke komputer)
git clone https://github.com/username/repo-name.git

# Clone ke folder dengan nama tertentu
git clone https://github.com/username/repo-name.git my-project

# Setelah clone, masuk ke folder
cd repo-name
```

**Apa yang terjadi?**
- Repository di-copy ke komputer Anda
- Semua history commit ikut ter-copy
- Remote sudah otomatis di-setup

#### 7. Pull dari GitHub

```bash
# Download perubahan terbaru dari GitHub
git pull

# Atau lebih spesifik
git pull origin main
```

**Kapan digunakan?**
- Ada perubahan di GitHub yang belum ada di komputer Anda
- Collaboration dengan tim
- Update kode dari komputer lain

**Contoh:**
```bash
# Di komputer A
git add .
git commit -m "Add new feature"
git push

# Di komputer B (atau setelah beberapa saat)
git pull
# Sekarang komputer B punya perubahan terbaru
```

#### 8. Workflow Collaboration

```bash
# 1. Clone repository
git clone https://github.com/username/repo-name.git
cd repo-name

# 2. Buat branch untuk fitur baru
git checkout -b feature/my-feature

# 3. Buat perubahan
# ... edit files ...

# 4. Commit perubahan
git add .
git commit -m "Add my feature"

# 5. Push branch ke GitHub
git push -u origin feature/my-feature

# 6. Di GitHub, buat Pull Request (akan dibahas nanti)

# 7. Setelah di-approve, merge di GitHub

# 8. Pull perubahan terbaru ke local
git checkout main
git pull
```

### Latihan 6.4

```bash
# Praktikkan GitHub:
# 1. Buat akun GitHub (jika belum)
# 2. Buat repository baru di GitHub
# 3. Hubungkan repository local ke GitHub
# 4. Push kode ke GitHub
# 5. Cek di GitHub, pastikan kode sudah ter-upload
# 6. Clone repository ke folder lain (test)
# 7. Buat perubahan di clone tersebut
# 8. Push perubahan
# 9. Pull di repository asli

# Catat setiap langkah:
```

---

## 📚 Materi 6.5: Menggunakan Sourcetree (GUI Git)

### Penjelasan untuk Instruktur

**Konsep Penting:**
- GUI Git memudahkan untuk pemula
- Sourcetree adalah salah satu GUI Git yang populer
- Visual representation membuat Git lebih mudah dipahami
- Tetap penting memahami Git commands dasar
- GUI adalah tool, bukan pengganti pemahaman konsep

### Materi Pembelajaran

#### 1. Install Sourcetree

**Download:**
- Windows/Mac: https://www.sourcetreeapp.com/
- Install dengan default settings

**Setup Pertama Kali:**
1. Buka Sourcetree
2. Login dengan akun GitHub/Bitbucket (optional)
3. Setup Git path (biasanya auto-detect)
4. Selesai!

#### 2. Clone Repository dengan Sourcetree

**Cara 1: Clone dari URL**
1. Klik **"Clone"** atau **"+"** → **"Clone from URL"**
2. Paste URL repository (GitHub/Bitbucket)
3. Pilih folder destination
4. Klik **"Clone"**

**Cara 2: Add Existing Repository**
1. Klik **"+"** → **"Add Existing Repository"**
2. Pilih folder yang sudah ada Git repository
3. Klik **"Add"**

#### 3. Interface Sourcetree

**Bagian-bagian utama:**

1. **File Browser** (kiri atas)
   - Daftar file di repository
   - Status file (modified, untracked, dll)

2. **Commit History** (kiri bawah)
   - Daftar semua commit
   - Visual graph untuk branch dan merge

3. **Working Directory** (tengah)
   - File yang sedang di-edit
   - Diff view untuk melihat perubahan

4. **Staging Area** (kanan)
   - File yang siap untuk di-commit
   - Bisa drag-drop file ke sini

#### 4. Basic Operations dengan Sourcetree

**A. Stage File (Add ke Staging)**

1. Edit file di editor
2. Di Sourcetree, file akan muncul di **"Unstaged files"**
3. Klik checkbox di samping file untuk stage
4. Atau drag file ke **"Staged files"**

**B. Commit**

1. Setelah stage file, tulis commit message di bawah
2. Klik tombol **"Commit"**
3. Commit selesai!

**C. Push ke GitHub**

1. Setelah commit, klik tombol **"Push"**
2. Pilih branch yang mau di-push
3. Klik **"OK"**
4. Kode ter-upload ke GitHub!

**D. Pull dari GitHub**

1. Klik tombol **"Pull"**
2. Pilih branch yang mau di-pull
3. Klik **"OK"**
4. Perubahan terbaru ter-download!

#### 5. Melihat Perubahan (Diff)

**Cara:**
1. Klik file yang sudah diubah
2. Di bagian bawah, akan muncul diff view
3. Hijau = baris yang ditambahkan
4. Merah = baris yang dihapus

**Gunakan untuk:**
- Review perubahan sebelum commit
- Memahami apa yang berubah
- Debugging

#### 6. Branch Management dengan Sourcetree

**Membuat Branch:**
1. Klik tombol **"Branch"**
2. Tulis nama branch
3. Pilih base branch (biasanya main)
4. Klik **"Create Branch"**

**Switch Branch:**
1. Double-click branch di commit history
2. Atau klik kanan branch → **"Checkout"**

**Merge Branch:**
1. Pindah ke branch target (misalnya main)
2. Klik kanan branch yang mau di-merge
3. Pilih **"Merge [branch-name] into [current-branch]"**
4. Klik **"OK"**

#### 7. Resolve Conflict dengan Sourcetree

**Jika ada conflict:**
1. Sourcetree akan menunjukkan file yang conflict
2. Klik file tersebut
3. Pilih **"Resolve using External Merge Tool"** atau edit manual
4. Setelah resolve, stage file
5. Commit untuk menyelesaikan merge

#### 8. Visual Graph

**Keuntungan GUI:**
- Visual representation dari branch dan merge
- Lebih mudah memahami history
- Lihat kapan branch dibuat dan di-merge
- Lihat commit dari berbagai branch

**Cara membaca graph:**
- Garis vertikal = branch
- Garis horizontal = merge
- Bulatan = commit
- Warna berbeda = branch berbeda

### Latihan 6.5

```bash
# Praktikkan dengan Sourcetree:
# 1. Install Sourcetree
# 2. Clone repository GitHub Anda
# 3. Buat perubahan di file
# 4. Stage file menggunakan Sourcetree
# 5. Commit dengan pesan yang jelas
# 6. Push ke GitHub menggunakan Sourcetree
# 7. Buat branch baru dengan Sourcetree
# 8. Buat perubahan di branch tersebut
# 9. Commit dan push branch
# 10. Merge branch menggunakan Sourcetree
# 11. Lihat visual graph di Sourcetree

# Catat setiap langkah:
```

---

## 📚 Materi 6.6: Best Practices dan Workflow

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Commit message yang baik
- Kapan harus commit
- Branch naming convention
- .gitignore untuk file yang tidak perlu di-track
- Workflow yang baik untuk development

### Materi Pembelajaran

#### 1. Commit Message yang Baik

**Format yang Disarankan:**
```
<type>: <subject>

<body (optional)>

<footer (optional)>
```

**Types:**
- `feat`: Fitur baru
- `fix`: Bug fix
- `docs`: Dokumentasi
- `style`: Formatting (tidak mengubah kode)
- `refactor`: Refactoring kode
- `test`: Menambah test
- `chore`: Maintenance tasks

**Contoh:**
```bash
# ✅ Baik
git commit -m "feat: Add user authentication"
git commit -m "fix: Resolve login validation error"
git commit -m "docs: Update README with setup instructions"

# ❌ Buruk
git commit -m "update"
git commit -m "fix"
git commit -m "changes"
```

#### 2. Kapan Harus Commit?

**Commit ketika:**
- ✅ Fitur kecil sudah selesai
- ✅ Bug sudah diperbaiki
- ✅ Refactoring selesai
- ✅ Test sudah ditambahkan

**Jangan commit:**
- ❌ Kode yang tidak jalan (broken code)
- ❌ Perubahan yang belum selesai
- ❌ File temporary atau test

**Prinsip:**
- Commit kecil dan sering lebih baik daripada commit besar jarang
- Setiap commit harus bisa di-build dan jalan
- Commit message harus jelas

#### 3. Branch Naming Convention

**Format yang Disarankan:**
```
<type>/<description>
```

**Types:**
- `feature/`: Fitur baru
- `bugfix/`: Bug fix
- `hotfix/`: Fix urgent untuk production
- `refactor/`: Refactoring

**Contoh:**
```bash
feature/user-authentication
feature/add-shopping-cart
bugfix/login-error
hotfix/security-patch
refactor/user-service
```

#### 4. .gitignore

**Apa itu .gitignore?**
File yang berisi daftar file/folder yang tidak perlu di-track Git.

**Mengapa penting?**
- File node_modules tidak perlu di-track (terlalu besar)
- File environment (.env) tidak boleh di-track (security)
- File build/dist tidak perlu di-track
- File temporary tidak perlu di-track

**Contoh .gitignore untuk Node.js:**
```gitignore
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment variables
.env
.env.local
.env.*.local

# Build output
dist/
build/
*.log

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
Thumbs.db
```

**Cara membuat:**
1. Buat file `.gitignore` di root project
2. Tulis pattern file/folder yang mau di-ignore
3. Git akan otomatis ignore file tersebut

#### 5. Git Workflow yang Baik

**Workflow untuk Development:**

```bash
# 1. Update main branch
git checkout main
git pull origin main

# 2. Buat branch untuk fitur baru
git checkout -b feature/my-feature

# 3. Develop fitur
# ... edit files ...
git add .
git commit -m "feat: Add initial implementation"

# ... edit lagi ...
git add .
git commit -m "feat: Add validation"

# 4. Push branch ke GitHub
git push -u origin feature/my-feature

# 5. Buat Pull Request di GitHub (akan dibahas)

# 6. Setelah di-approve dan merge, update local
git checkout main
git pull origin main

# 7. Hapus branch yang sudah di-merge (optional)
git branch -d feature/my-feature
```

#### 6. Pull Request (PR) di GitHub

**Apa itu Pull Request?**
- Request untuk merge branch ke main
- Tempat review code sebelum merge
- Diskusi tentang perubahan

**Cara membuat PR:**
1. Push branch ke GitHub
2. Di GitHub, akan muncul tombol **"Compare & pull request"**
3. Klik tombol tersebut
4. Tulis deskripsi PR
5. Request review dari team member
6. Setelah di-approve, klik **"Merge pull request"**

**Best Practices PR:**
- Deskripsi yang jelas tentang perubahan
- Reference issue jika ada
- Screenshot jika ada perubahan UI
- Pastikan semua test pass
- Request review sebelum merge

### Latihan 6.6

```bash
# Praktikkan best practices:
# 1. Buat .gitignore untuk project Node.js
# 2. Buat branch dengan naming convention yang baik
# 3. Buat beberapa commit dengan message yang baik
# 4. Push branch ke GitHub
# 5. Buat Pull Request (jika bekerja dalam tim)
# 6. Review dan merge PR

# Catat setiap langkah:
```

---

## 📚 Materi 6.7: Praktik dengan Repository Boilerplate

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Clone repository boilerplate
- Buat branch untuk development
- Commit perubahan
- Push ke GitHub
- Collaboration workflow

### Materi Pembelajaran

#### 1. Clone Repository Boilerplate

```bash
# Clone repository
git clone https://github.com/teramuza/boilerplate-express-ts.git
cd boilerplate-express-ts

# Lihat status
git status

# Lihat history
git log --oneline
```

#### 2. Setup Repository Sendiri

**Jika ingin membuat fork (copy repository ke akun Anda):**

1. Di GitHub, klik tombol **"Fork"** di repository boilerplate
2. Repository akan ter-copy ke akun GitHub Anda
3. Clone repository Anda (bukan yang original)

```bash
# Clone repository fork Anda
git clone https://github.com/username/boilerplate-express-ts.git
cd boilerplate-express-ts
```

**Atau buat repository baru:**

```bash
# Clone repository original
git clone https://github.com/teramuza/boilerplate-express-ts.git
cd boilerplate-express-ts

# Hapus remote origin
git remote remove origin

# Tambahkan remote baru (repository Anda)
git remote add origin https://github.com/username/my-boilerplate.git

# Push ke repository Anda
git push -u origin main
```

#### 3. Development Workflow

```bash
# 1. Update main branch
git checkout main
git pull origin main

# 2. Buat branch untuk fitur baru
git checkout -b feature/add-products

# 3. Develop fitur (mengikuti modul sebelumnya)
# ... buat ProductController, ProductService, dll ...

# 4. Commit perubahan
git add .
git commit -m "feat: Add products CRUD endpoints"

# 5. Push branch
git push -u origin feature/add-products

# 6. Buat Pull Request di GitHub (jika bekerja dalam tim)
# Atau langsung merge jika solo project
```

#### 4. Menggunakan Sourcetree untuk Boilerplate

**Setup:**
1. Buka Sourcetree
2. Klik **"+"** → **"Add Existing Repository"**
3. Pilih folder boilerplate
4. Klik **"Add"**

**Development:**
1. Buat branch baru di Sourcetree
2. Edit file di VS Code
3. Stage file di Sourcetree
4. Commit dengan message yang jelas
5. Push ke GitHub
6. Lihat visual graph untuk memahami history

### Latihan 6.7

```bash
# Praktikkan dengan boilerplate:
# 1. Clone atau fork repository boilerplate
# 2. Setup repository Anda sendiri
# 3. Buat branch untuk fitur baru (misalnya: feature/add-categories)
# 4. Develop fitur mengikuti modul sebelumnya
# 5. Commit dengan message yang baik
# 6. Push branch ke GitHub
# 7. (Optional) Buat Pull Request
# 8. Merge ke main branch
# 9. Update local main branch

# Gunakan Sourcetree untuk visualisasi

# Catat setiap langkah:
```

---

## ✅ Review Modul 6

### Checklist Pemahaman

Setelah modul ini, pastikan peserta memahami:

- [ ] Konsep version control dengan Git
- [ ] Git commands dasar (init, add, commit, push, pull)
- [ ] Konsep branch dan merge
- [ ] Bekerja dengan GitHub (remote repository)
- [ ] Menggunakan Sourcetree untuk operasi Git
- [ ] Best practices untuk commit message dan workflow
- [ ] Collaboration dengan Git dan GitHub

### Latihan Akhir Modul

**Final Project Git:**
1. **Setup Repository**
   - Buat repository GitHub baru
   - Clone ke komputer
   - Setup .gitignore

2. **Development Workflow**
   - Buat branch untuk fitur baru
   - Develop fitur (bisa menggunakan boilerplate)
   - Commit dengan message yang baik
   - Push branch ke GitHub

3. **Collaboration (jika bekerja dalam tim)**
   - Buat Pull Request
   - Review code
   - Merge PR

4. **Menggunakan Sourcetree**
   - Setup repository di Sourcetree
   - Gunakan Sourcetree untuk semua operasi Git
   - Lihat visual graph

**Requirements:**
- Minimal 3 commit dengan message yang baik
- Minimal 2 branch (main + feature branch)
- Push ke GitHub
- Menggunakan Sourcetree untuk visualisasi

---

## 🎯 Persiapan untuk Modul Berikutnya

Modul berikutnya akan memperkenalkan Express.js. Pastikan peserta sudah nyaman dengan:
- ✅ Git basics (init, add, commit, push, pull)
- ✅ Branch dan merge
- ✅ GitHub untuk remote repository
- ✅ Menggunakan Sourcetree untuk visualisasi

**Catatan Instruktur:** 
- Pastikan peserta sudah bisa menggunakan Git untuk track perubahan
- Dorong peserta untuk menggunakan Git di setiap project setelah ini
- Git akan sangat membantu saat belajar Express dan REST API

---

## 🎯 Kesimpulan Modul Git & GitHub

Selamat! Anda telah mempelajari:

✅ **Git Basics:**
- Version control system
- Commands dasar (init, add, commit, push, pull)
- Branch dan merge

✅ **GitHub:**
- Remote repository
- Collaboration
- Clone dan fork

✅ **GUI Tools:**
- Sourcetree untuk visualisasi
- Operasi Git melalui GUI

✅ **Best Practices:**
- Commit message yang baik
- Branch naming convention
- .gitignore
- Development workflow

**Langkah Selanjutnya:**
1. Terus praktik dengan Git di setiap project
2. Explore fitur Git yang lebih advanced
3. Pelajari Git hooks untuk automation
4. Pelajari GitHub Actions untuk CI/CD
5. Baca dokumentasi Git lebih dalam

**Sumber Belajar Tambahan:**
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)
- [Sourcetree Documentation](https://confluence.atlassian.com/sourcetree)

**Selamat menggunakan Git! 🚀**
