# 🧑‍💻 Tmux Cheat Sheet (Prefix = Ctrl-A)

This cheat sheet is based on your custom `~/.tmux.conf` setup.

---

## 📦 Sessions (workspaces)
- **Create / attach session**  
  ```bash
  tmux new -As <name>
  ```
- **List sessions**  
  ```bash
  tmux ls
  ```
- **Attach to session**  
  ```bash
  tmux attach -t <name>
  ```
- **Detach from current session**  
  Prefix `Ctrl-A d`
- **Kill current session**  
  Prefix `Ctrl-A X` → confirm  
  Or:  
  ```bash
  tmux kill-session -t <name>
  ```
- **Kill all sessions**  
  ```bash
  tmux kill-server
  ```

---

## 🗂 Windows (tabs inside a session)
- **New window** → Prefix `c`
- **Rename window** → Prefix `,`
- **Next / Previous window** → Prefix `n` / `p`
- **Jump to window #** → Prefix `1..9`
- **Move window left / right** → Prefix `<` / `>`
- **Kill window** → Prefix `&`
- **Browse sessions/windows/panes** → Prefix `s`

---

## 🔲 Panes (splits inside a window)
- **Vertical split** → Prefix `|`
- **Horizontal split** → Prefix `-`
- **Move focus** → Prefix `h / j / k / l`
- **Resize pane** → Prefix `H / J / K / L`
- **Zoom current pane** → Prefix `z`
- **Kill pane** → Prefix `x`
- **Toggle sync panes** → Prefix `y`

---

## 🖼 Layouts
- **Even horizontal / vertical** → Prefix `=` / `+`
- **Tiled** → Prefix `t`
- **Main-horizontal / vertical** → Prefix `b` / `v`
- **Show pane numbers overlay** → Prefix `q`

---

## 📋 Copy mode (vi-style + mac clipboard)
- **Enter copy mode** → Prefix `[`  
- **Start selection** → `v`  
- **Copy selection to macOS clipboard** → `y`  
- **Paste buffer** → Prefix `]`

---

## ⚡ Miscellaneous
- **Reload config** → Prefix `r`
- **Rename session** → Prefix `$`

---

✅ Remember: everything starts with your prefix → **Ctrl-A**.
