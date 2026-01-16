# LinuxAssistant 📱

A **terminal-first productivity toolkit** built on Alpine Linux (via iSH on iOS), designed to prove that powerful developer tools don’t need heavy GUIs.

LinuxAssistant provides:

* 🧠 **Notes system** (searchable, Markdown-based)
* ✅ **Todo manager** (CLI + interactive command UI)
* ⚡ **Fast, offline, Unix-style workflows**

This project is intentionally lightweight, portable, and educational — showcasing **real Linux engineering practices** under constraints.

---

## ✨ Why LinuxAssistant?

* Works on **Alpine Linux (iSH on iPhone)**
* No databases, no frameworks, no cloud dependency
* Plain-text storage → Git-friendly
* Demonstrates:

  * Shell scripting
  * PATH management
  * CLI design
  * Interactive terminal UX
  * Debugging under constraints

> Built as a learning + showcase project for software engineering fundamentals.

---

## 📂 Project Structure

```text
LinuxAssistant/
├── bin/
│   ├── pla          # main command router
│   ├── pla-note     # notes subsystem
│   └── pla-todo     # todo subsystem
├── notes/           # markdown notes (auto-created)
└── todos/           # todo storage (plain text)
```

---

## 🚀 Installation

### 1️⃣ Clone the repository

```sh
git clone https://github.com/<your-username>/LinuxAssistant.git
cd LinuxAssistant
```

### 2️⃣ Install dependencies (Alpine Linux)

```sh
apk add bash fzf ripgrep coreutils
```

### 3️⃣ Make scripts executable

```sh
chmod +x bin/*
```

### 4️⃣ Add to PATH

```sh
echo 'export PATH="$(pwd)/bin:$PATH"' >> ~/.profile
source ~/.profile
```

Verify:

```sh
which pla
```

---

## 🧠 Notes System (`pla note`)

### ➕ Add a note

```sh
pla note add "Binary search edge cases"
```

Notes are stored as daily Markdown files:

```
notes/YYYY-MM-DD.md
```

---

### 🔍 Search notes

```sh
pla note find binary
```

---

### 📚 Browse notes (fzf)

```sh
pla note browse
```

---

## ✅ Todo System (`pla todo`)

LinuxAssistant offers **two ways** to manage todos.

---

### 🧑‍💻 Direct CLI mode

```sh
pla todo add "Revise linked lists"
pla todo list
pla todo done 1
pla todo delete 2
```

---

### 🖥 Interactive UI mode (REPL-style)

Launch with:

```sh
pla todo
# or
pla todo list
```

Example session:

```
PLA TODO
========
1. [ ] revise linked lists
2. [x] practice DP
3. [ ] read OS notes

> done 1
Marked task 1 as done.

> delete 3
Deleted task 3.

> add prepare CN
Added task.

> exit
```

Supported commands inside UI:

* `add <text>`
* `done <N>`
* `delete <N>`
* `exit`

---

## 🧩 Design Philosophy

* **Unix philosophy**: small tools, composed well
* **Plain text over database**
* **Works offline**
* **Easy to debug, easy to extend**

---

## 🛠 Extending LinuxAssistant

Ideas you can build next:

* Tags for notes (`#dsa #linux`)
* Todo priorities (`[!]`, `[!!]`)
* Daily stats (`pla stats`)
* Archive completed todos
* Encryption (GPG)

The code is intentionally readable to encourage experimentation.

---

## 📱 Tested Environment

* iPhone X
* iSH (Alpine Linux)
* POSIX shell (`ash`)

Should work on **any Linux system**.

---

## 📜 License

MIT License — free to use, modify, and share.

---

## 🙌 Author

Built by **Darshan** as a learning-focused systems project.

If this helped you understand Linux, shell scripting, or CLI design — ⭐ the repo and share it with others.

