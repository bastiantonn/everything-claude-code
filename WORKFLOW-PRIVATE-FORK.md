# Private Fork Workflow

> Wie du Updates vom öffentlichen Original-Repo holst und eigene private Anpassungen behältst

## Setup (Einmalig)

### 1. Repository auf Private stellen

**GitHub Web:**
1. Gehe zu: https://github.com/bastiantonn/everything-claude-code/settings
2. Scrolle zu "Danger Zone"
3. "Change repository visibility" → "Make private"
4. Bestätige mit Repository-Namen

### 2. Remotes konfiguriert (Already Done ✅)

```bash
# origin = Dein privates Repo (für deine Änderungen)
origin: https://github.com/bastiantonn/everything-claude-code.git

# upstream = Original public Repo (für Updates)
upstream: https://github.com/affaan-m/everything-claude-code.git
```

**Remotes anzeigen:**
```bash
git remote -v
```

---

## Daily Workflow

### Eigene Änderungen committen

```bash
# 1. Status prüfen
git status

# 2. Dateien adden
git add .

# 3. Committen
git commit -m "feat: Meine private Anpassung"

# 4. Zu deinem PRIVATE Repo pushen
git push origin main
```

**Wichtig:** `git push` geht immer zu `origin` (dein private repo)! ✅

---

## Updates vom Original-Repo holen

### Alle paar Tage/Wochen: Upstream Updates holen

```bash
# 1. Upstream Updates fetchen
git fetch upstream

# 2. Upstream Changes in deinen Branch mergen
git merge upstream/main

# 3. Falls Merge Conflicts:
# - Löse Conflicts in Editor
# - git add [resolved files]
# - git commit

# 4. Zu deinem Private Repo pushen
git push origin main
```

**Als ein Command:**
```bash
git fetch upstream && git merge upstream/main && git push origin main
```

---

## Strategien für eigene Anpassungen

### Option 1: Separate Branches (Empfohlen für große Anpassungen)

```bash
# Branch für deine custom features
git checkout -b custom-features

# Deine Änderungen
[... edit files ...]
git add .
git commit -m "feat: Custom feature XY"

# Pushen zu deinem private repo
git push origin custom-features

# Später mergen
git checkout main
git merge custom-features
```

**Vorteil:** Original `main` bleibt clean, einfacher upstream updates zu mergen.

---

### Option 2: Direkt auf Main (Für kleine Anpassungen)

```bash
# Direkt auf main committen
git add .
git commit -m "feat: Meine Anpassung"
git push origin main
```

**Vorteil:** Einfacher.
**Nachteil:** Merge conflicts wahrscheinlicher bei upstream updates.

---

### Option 3: Eigene Dateien in separaten Ordnern (Empfohlen!)

**Struktur:**
```
everything-claude-code/
├── agents/              # ← Original (wird von upstream updated)
├── commands/            # ← Original
├── skills/              # ← Original
├── rules/               # ← Original
│
├── my-agents/          # ← DEINE custom agents (private)
├── my-commands/        # ← DEINE custom commands (private)
├── my-skills/          # ← DEINE custom skills (private)
├── my-rules/           # ← DEINE custom rules (private)
│
└── .gitignore          # ← secrets/ in .gitignore
```

**Vorteil:**
- Null Merge Conflicts!
- Original-Dateien bleiben unberührt
- Updates vom upstream konfliktfrei

**Plugin Config:**
```json
{
  "enabledPlugins": {
    "everything-claude-code@everything-claude-code": true
  }
}
```

Deine `my-*` Ordner werden automatisch geladen wenn sie in:
- `~/.claude/agents/` (user-level)
- `.claude/agents/` (project-level)

---

## Merge Conflicts lösen

### Wenn `git merge upstream/main` Conflicts hat:

```bash
# 1. Konflikt-Dateien anzeigen
git status

# Zeigt z.B.:
# both modified:   commands/tdd.md
# both modified:   skills/frontend-patterns/SKILL.md

# 2. Datei öffnen und Conflicts suchen
# Suche nach:
<<<<<<< HEAD
(Deine Version)
=======
(Upstream Version)
>>>>>>> upstream/main

# 3. Entscheide:
# - Behalte deine Version
# - Übernimm upstream Version
# - Kombiniere beide

# 4. Lösche die Conflict Markers (<<<<, ====, >>>>)

# 5. Add resolved file
git add commands/tdd.md

# 6. Commit den Merge
git commit -m "Merge upstream updates, resolved conflicts"

# 7. Push zu deinem repo
git push origin main
```

### Pro-Tip: Conflicts vermeiden

**Eigene Dateien umbenennen:**
```bash
# Statt: commands/tdd.md zu editieren
# Erstelle: commands/tdd-custom.md

# Dann:
cp commands/tdd.md commands/tdd-custom.md
# Edit tdd-custom.md
```

**Oder:** Nutze `my-*` Ordner (siehe Option 3).

---

## Typische Workflows

### 1. Neues Original Feature ist raus

```bash
# Update holen
git fetch upstream
git merge upstream/main

# Testen ob alles funktioniert
claude /help

# Pushen
git push origin main
```

---

### 2. Du willst ein Original Feature anpassen

**Option A: Kopie erstellen (Empfohlen)**
```bash
# Kopiere Original File
cp commands/tdd.md my-commands/tdd-custom.md

# Edit deine Version
# commands/tdd.md bleibt original (für updates)
# my-commands/tdd-custom.md ist deine Version
```

**Option B: Direkt editieren**
```bash
# Edit
vim commands/tdd.md

# Commit
git add commands/tdd.md
git commit -m "feat: Custom TDD workflow"
git push origin main

# Später bei upstream update:
git fetch upstream
git merge upstream/main
# → Merge Conflict in commands/tdd.md
# → Manuell lösen
```

---

### 3. Du hast neue private Components

```bash
# Erstelle in my-* Ordnern
touch my-commands/mein-workflow.md
touch my-skills/mein-skill/SKILL.md
touch my-agents/mein-agent.md

# Commit & Push
git add my-*
git commit -m "feat: Add private custom components"
git push origin main

# Bei upstream updates:
git fetch upstream && git merge upstream/main
# → Keine Conflicts! my-* Dateien gibt's nur bei dir
```

---

## Best Practices

### ✅ DO:

1. **Regelmäßig upstream updates holen** (wöchentlich)
2. **Eigene Komponenten in `my-*` Ordner** (conflict-free)
3. **Beschreibende Commit Messages** (`feat:`, `fix:`, `chore:`)
4. **Vor upstream merge: Status prüfen** (`git status` sollte clean sein)
5. **Nach merge: Testen ob alles funktioniert**

### ❌ DON'T:

1. **Secrets committen** (nutze `.gitignore`)
2. **Original-Dateien editieren** (besser: kopieren)
3. **Upstream updates ignorieren** (veraltete Features/Bugs)
4. **Ungetestedete Merges pushen**

---

## Secrets & Sensitive Data

### .gitignore anpassen

```bash
# .gitignore
secrets/
*.env
*.key
*-private.md
my-credentials.json
.claude/sessions/*-private.md
```

**Testen:**
```bash
git status
# Sollte secrets/ nicht anzeigen
```

---

## Troubleshooting

### "Already up to date" aber ich sehe neue Features auf GitHub

```bash
# Upstream re-fetchen
git fetch upstream --force

# Merge nochmal versuchen
git merge upstream/main
```

---

### "Please commit your changes before merging"

```bash
# Changes committen oder stashen
git add .
git commit -m "wip: Save current work"

# Dann merge
git merge upstream/main
```

---

### "Merge conflict in [file]"

Siehe "Merge Conflicts lösen" oben.

---

### Ich hab aus Versehen zu upstream gepusht

```bash
# Keine Sorge, das geht nicht!
# Du hast keine Push-Rechte zu affaan-m/everything-claude-code

# git push geht immer zu origin (dein repo)
```

---

## Quick Reference

```bash
# Eigene Changes
git add .
git commit -m "feat: My change"
git push origin main

# Upstream Updates
git fetch upstream
git merge upstream/main
git push origin main

# Status
git status
git remote -v
git log --oneline -10

# Branches
git branch
git checkout -b feature-xyz
git merge feature-xyz
```

---

## Recap

**Du hast jetzt:**
- ✅ Private Fork mit deinen Anpassungen
- ✅ Zwei Remotes (origin=private, upstream=public)
- ✅ Workflow um Updates zu holen
- ✅ Strategie für conflict-freie Anpassungen

**Workflow:**
1. Eigene Changes → `git push origin main` (private)
2. Updates holen → `git fetch upstream && git merge upstream/main`
3. Eigene Komponenten in `my-*` Ordnern → Zero Conflicts

---

**Happy Coding! 🚀**

Bei Fragen → Issues in deinem Private Repo oder direkt in Claude fragen!
