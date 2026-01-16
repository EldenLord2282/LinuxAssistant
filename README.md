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

## 🚀 Complete Step-by-Step Guide (From Zero to Finished Project)

This section walks you **from absolute scratch to a fully working LinuxAssistant**, exactly how this project was built. Nothing is assumed.

---

### **Step 1: Install iSH on iOS**

1. Open the **Apple App Store** on your iPhone
2. Search for **iSH**
3. Install the app (this provides Alpine Linux)
4. Open iSH — you will see a Linux terminal prompt

You are now inside an **Alpine Linux environment on iOS**.

---

### **Step 2: Update Alpine Linux**

```sh
apk update
apk upgrade
```

---

### **Step 3: Install Required Tools**

```sh
apk add bash fzf ripgrep coreutils git
```

These tools are required for:

* scripting (`bash` / `ash`)
* fuzzy search (`fzf`)
* fast text search (`ripgrep`)
* standard Unix utilities

---

### **Step 4: Create Project Directory Structure**

```sh
cd ~
mkdir LinuxAssistant
cd LinuxAssistant
mkdir bin notes todos
```

Verify:

```sh
tree
```

Expected:

```text
LinuxAssistant/
├── bin
├── notes
└── todos
```

---

### **Step 5: Create the Main Command (`pla`)**

```sh
cd bin
touch pla
chmod +x pla
vi pla
```

Paste:

```sh
#!/bin/ash

BASE="/root/LinuxAssistant"

cmd="$1"
shift

case "$cmd" in
  note)
    "$BASE/bin/pla-note" "$@"
    ;;
  todo)
    "$BASE/bin/pla-todo" "$@"
    ;;
  *)
    echo "Usage: pla {note|todo}"
    ;;
esac
```

Save with:

```
ESC :wq Enter
```

---

### **Step 6: Create Notes Subsystem (`pla-note`)**

```sh
touch pla-note
chmod +x pla-note
vi pla-note
```

Paste:

```sh
#!/bin/ash

BASE="/root/LinuxAssistant"
NOTES="$BASE/notes"

mkdir -p "$NOTES"

action="$1"
shift

case "$action" in
  add)
    file="$NOTES/$(date +%Y-%m-%d).md"
    echo "- $*" >> "$file"
    echo "Note added."
    ;;
  find)
    rg "$*" "$NOTES"
    ;;
  browse)
    rg "" "$NOTES" | fzf
    ;;
  *)
    echo "Usage: pla note {add|find|browse}"
    ;;
esac
```

Save and exit.

---

### **Step 7: Create Todo Subsystem (`pla-todo`)**

```sh
touch pla-todo
chmod +x pla-todo
vi pla-todo
```

Paste:

```sh
#!/bin/ash

BASE="/root/LinuxAssistant"
TODO_DIR="$BASE/todos"
TODO_FILE="$TODO_DIR/todo.txt"

mkdir -p "$TODO_DIR"
touch "$TODO_FILE"

add_todo() {
  echo "[ ] $*" >> "$TODO_FILE"
}

done_todo() {
  sed -i "${1}s/\[ \]/[x]/" "$TODO_FILE"
}

delete_todo() {
  sed -i "${1}d" "$TODO_FILE"
}

print_ui() {
  echo
  echo "PLA TODO"
  echo "========"
  nl -w2 -s'. ' "$TODO_FILE" 2>/dev/null || echo "(no tasks)"
  echo
  echo "Commands:"
  echo "  add <text>"
  echo "  done <N>"
  echo "  delete <N>"
  echo "  exit"
  echo
}

interactive_ui() {
  while true; do
    print_ui
    printf "> "
    read cmd arg rest

    case "$cmd" in
      add)
        add_todo "$arg $rest" && echo "Added task."
        ;;
      done)
        done_todo "$arg" && echo "Done $arg."
        ;;
      delete)
        delete_todo "$arg" && echo "Deleted $arg."
        ;;
      exit)
        exit 0
        ;;
      *)
        echo "Unknown command."
        ;;
    esac
  done
}

action="$1"
shift

case "$action" in
  "" | list)
    interactive_ui
    ;;
  add)
    add_todo "$@" && echo "Todo added."
    ;;
  done)
    done_todo "$1" && echo "Done $1."
    ;;
  delete)
    delete_todo "$1" && echo "Deleted $1."
    ;;
  *)
    echo "Usage: pla todo {add|list|done|delete}"
    ;;
esac
```

---

### **Step 8: Add LinuxAssistant to PATH**

```sh
echo 'export PATH="/root/LinuxAssistant/bin:/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/sbin:/usr/local/bin"' >> /root/.profile
source /root/.profile
```

Verify:

```sh
which pla
```

---

### **Step 9: Test Everything**

```sh
pla note add "First note from iPhone Linux"
pla note find First

pla todo add "Revise linked lists"
pla todo
```

---

### **Step 10: Initialize Git Repository**

```sh
git init
git add .
git commit -m "Initial release: LinuxAssistant CLI toolkit"
```

---

### **Step 11: Push to GitHub**

```sh
git branch -M main
git remote add origin https://github.com/<your-username>/LinuxAssistant.git
git push -u origin main
```

---

At this point, your project is **complete, reproducible, and GitHub-ready**.

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
