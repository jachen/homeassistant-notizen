
# 🧠 Mein persönliches Git Cheatsheet – für `homeassistant-notizen`

## 🔧 Standard-Workflows

### 🟢 Neues Repo starten
```bash
git init
git remote add origin https://github.com/jachen/homeassistant-notizen.git
git branch -M main
```

### 📝 Datei zum Repo hinzufügen
```bash
git add homeassistant-vs-grafana.md
git commit -m "Datei hinzugefügt"
git push -u origin main
```

### ✏️ Bestehende Datei ändern und hochladen
```bash
git add <dateiname>
git commit -m "Änderung kommentieren"
git push
```

---

## 🔍 Nützliche Befehle

| Befehl                     | Bedeutung                                      |
|----------------------------|-----------------------------------------------|
| `git status`              | Was hat sich verändert?                       |
| `git log`                 | Zeigt commit-Historie                         |
| `git diff`                | Zeigt Änderungen seit letztem Commit          |
| `git pull`                | Holt Änderungen von GitHub                    |
| `git push`                | Schickt lokale Änderungen zu GitHub           |

---

## 🛡 Zugang speichern

Token nicht immer eintippen? → Speichern im Schlüsselbund:

```bash
git config --global credential.helper osxkeychain
```

---

## 🧹 Bonus

### 🚫 Datei vergessen?
```bash
git restore <dateiname>
```

### 🗑 Letzten Commit rückgängig?
```bash
git reset --soft HEAD~1
```

### 💣 Alles löschen und von vorn?
```bash
rm -rf .git
git init
```

---

## 📌 Tipps

- Nutze `README.md`, um dein Repo zu erklären
- Für Webseiten: `README.md` → GitHub Pages aktivieren
- Schreib Commit-Kommentare wie Tagebucheinträge ✍️

---

👨‍💻 Erstellt am: 2025-05-22  
Für: Jachen  
