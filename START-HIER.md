# 🚀 Start Hier - Deine Everything Claude Code Setup

> Quick Start Guide für dein privates Setup

## ⚡ Direkt loslegen

### 1. Heute produktiv werden (15 Min)

```bash
# Teste die wichtigsten Commands
/plan              # Feature planen
/tdd               # Feature bauen mit Tests
/code-review       # Code checken
```

**Lies:** [`PRAXIS-GUIDE.md`](./PRAXIS-GUIDE.md)
- Für MVP-Builder wie dich
- Fokus auf shadcn-ui
- Konkrete Beispiele
- Häufige Szenarien

---

### 2. Shadcn Workflows verstehen (20 Min)

**Lies:**
- [`workflows/shadcn-mvp-workflow.md`](./workflows/shadcn-mvp-workflow.md) - Kompletter 6-Phasen Workflow
- [`examples/shadcn-komponenten.md`](./examples/shadcn-komponenten.md) - Code-Beispiele & Prompts

**Teste:**
```bash
/plan

"Baue ein User Dashboard mit:
- Stats Cards (shadcn Card)
- Data Table (TanStack Table + shadcn)
- Export Button (CSV Download)
- Date Range Filter (shadcn Popover)"
```

---

### 3. Private Fork Workflow einrichten (10 Min)

**WICHTIG:** Du willst Updates vom Original aber eigene private Anpassungen.

**Lies:** [`WORKFLOW-PRIVATE-FORK.md`](./WORKFLOW-PRIVATE-FORK.md)

**Quick Setup:**

1. **Repo auf Private stellen:**
   - https://github.com/bastiantonn/everything-claude-code/settings
   - "Change repository visibility" → "Make private"

2. **Remotes sind bereits konfiguriert:**
   ```bash
   git remote -v
   # origin = dein private repo
   # upstream = original public repo
   ```

3. **Workflow:**
   ```bash
   # Eigene Changes
   git add .
   git commit -m "feat: Meine Anpassung"
   git push origin main

   # Updates vom Original holen
   git fetch upstream
   git merge upstream/main
   git push origin main
   ```

---

## 📚 Die Guides im Überblick

### Neu erstellt (speziell für dich):

1. **[PRAXIS-GUIDE.md](./PRAXIS-GUIDE.md)** ⭐ START HERE
   - Für Technical Founders & MVP-Builder
   - Die 5 wichtigsten Commands
   - Shadcn-UI Workflows
   - Sessions & Memory
   - Token sparen
   - Häufige Szenarien
   - Best Practices

2. **[workflows/shadcn-mvp-workflow.md](./workflows/shadcn-mvp-workflow.md)**
   - 6-Phasen Workflow
   - Phase 1: Planning (5-10 Min)
   - Phase 2: Implementation (30-60 Min)
   - Phase 3: UI/UX Polish (15-30 Min)
   - Phase 4: Code Review (10 Min)
   - Phase 5: Testing & Build (5-10 Min)
   - Phase 6: Deployment (5 Min)
   - Komplettes Feature in 60 Minuten

3. **[examples/shadcn-komponenten.md](./examples/shadcn-komponenten.md)**
   - Forms & Validation
   - Data Tables
   - Dialogs & Modals
   - Loading States
   - Notifications
   - Navigation
   - Advanced Patterns
   - Prompt Templates

4. **[WORKFLOW-PRIVATE-FORK.md](./WORKFLOW-PRIVATE-FORK.md)**
   - Private Fork Setup
   - Updates vom Original holen
   - Eigene Anpassungen committen
   - Merge Conflicts lösen
   - Best Practices
   - Troubleshooting

### Original Guides (vom Creator):

5. **[the-shortform-guide.md](./the-shortform-guide.md)**
   - Complete Setup
   - Skills, Commands, Hooks
   - Subagents, MCPs, Plugins
   - Editor Setup (Zed, VS Code)
   - Keyboard Shortcuts
   - Tips & Tricks

6. **[the-longform-guide.md](./the-longform-guide.md)**
   - Advanced Patterns
   - Token Optimization
   - Memory Persistence
   - Verification Loops
   - Parallelization
   - Subagent Orchestration
   - Continuous Learning

---

## 🎯 Deine nächsten Schritte

### Heute (30 Min):
- [ ] Lies PRAXIS-GUIDE.md
- [ ] Teste `/plan` für ein kleines Feature
- [ ] Teste `/tdd` für die Implementation
- [ ] Stelle Repo auf Private (GitHub Settings)

### Diese Woche:
- [ ] Lies shadcn-mvp-workflow.md
- [ ] Baue ein Feature mit dem 6-Phasen Workflow
- [ ] Erstelle eigene Rules in `.claude/rules/meine-regeln.md`
- [ ] Setup Memory Persistence (aus Longform Guide)

### Diesen Monat:
- [ ] Erstelle eigenen Skill für deinen Workflow
- [ ] Baue eigenen Command für häufige Tasks
- [ ] Experimentiere mit Subagents
- [ ] Setup Continuous Learning

---

## 💡 Quick Wins

### 1. Statusline konfigurieren
```bash
/statusline
```
Zeigt dir: Context %, Model, Branch, Todos

### 2. Erste eigene Rule
```bash
# .claude/rules/meine-regeln.md
# Projekt-Regeln

## UI/UX
- Immer shadcn/ui Components nutzen
- Dark Mode Support
- Loading States für alle Buttons
- Toast für Success/Error

## Code
- TypeScript Strict Mode
- ESLint + Prettier
- Keine console.logs
```

### 3. MCPs aufräumen
```bash
/mcp
```
Deaktiviere alles was du heute nicht brauchst (< 10 MCPs)

---

## 🎨 Shadcn Cheat Sheet

```bash
# Feature mit Form
/plan
"Baue [Feature] mit shadcn Form + react-hook-form + zod"

# Data Table
/plan
"Baue [Entity] Table mit TanStack Table + shadcn Components"

# Dialog/Modal
/plan
"Baue [Action] Dialog mit shadcn Dialog + Form"

# Loading States
"Add loading states with shadcn Skeleton"

# Notifications
"Add toast notifications for success/error"
```

---

## 🔥 Häufige Workflows

### Neues Feature bauen
```bash
/plan → /tdd → /code-review → /build-fix → git commit
```

### Bug fixen
```bash
/tdd → "Fix bug: [description]" → /code-review
```

### Code aufräumen
```bash
/refactor-clean → /test-coverage
```

### UI verbessern
```bash
"Refactor to shadcn components" → /code-review
```

---

## 📞 Support

### Bei Fragen:
1. Frag direkt in deiner Claude Session
2. Issues in deinem Private Repo
3. Schau in die Guides

### Bei Updates vom Original:
```bash
git fetch upstream
git merge upstream/main
```

---

## 🎓 Learning Path

**Woche 1: Basics**
→ PRAXIS-GUIDE.md
→ Commands ausprobieren
→ Erstes Feature bauen

**Woche 2: Shadcn Mastery**
→ shadcn-mvp-workflow.md
→ shadcn-komponenten.md
→ Komplexes Feature bauen

**Woche 3: Advanced**
→ the-shortform-guide.md
→ Eigene Skills/Commands
→ Memory Persistence

**Woche 4: Expert**
→ the-longform-guide.md
→ Token Optimization
→ Subagent Orchestration

---

## ⚡ Power User Tips

1. **Command Chaining:**
   ```bash
   /plan && /tdd && /code-review
   ```

2. **Session speichern:**
   ```
   "Save session to .claude/sessions/[name].md"
   ```

3. **Parallel arbeiten:**
   ```bash
   /fork  # Neue Conversation für Recherche
   ```

4. **Schneller navigieren:**
   ```
   @ → File suchen
   / → Command ausführen
   ! → Bash command
   ```

5. **Context im Blick:**
   ```
   /statusline → ctx:65% anzeigen
   ```

---

**Du bist ready! Los geht's! 🚀**

Fang mit dem PRAXIS-GUIDE an und baue dein erstes Feature mit dem shadcn-mvp-workflow!

Bei Fragen einfach in Claude fragen - ich helfe dir gerne weiter.
