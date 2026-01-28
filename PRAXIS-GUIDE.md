# 🚀 Everything Claude Code - Praxis-Guide für MVP-Builder

> **Für wen ist das?** Technical Founders und Builder, die schon ein paar MVPs gebaut haben, aber Claude Code optimal nutzen wollen. Spezieller Fokus auf shadcn-ui Komponenten und schnelles Iterieren.

## 📚 Inhaltsverzeichnis

- [Quick Start](#-quick-start---in-5-minuten-produktiv)
- [Die 5 wichtigsten Commands](#-die-5-wichtigsten-commands-für-mvp-development)
- [Shadcn-UI Workflows](#-shadcn-ui-workflows)
- [Sessions & Memory](#-sessions--memory-nie-wieder-kontext-verlieren)
- [Token & Kosten sparen](#-token--kosten-sparen)
- [Häufige Szenarien](#-häufige-szenarien)
- [Best Practices](#-best-practices)

---

## ⚡ Quick Start - In 5 Minuten produktiv

### Was du jetzt schon kannst:

```bash
# Feature mit Tests bauen
/tdd

# Neues Feature planen
/plan

# Code Review durchführen
/code-review

# Build-Fehler automatisch fixen
/build-fix

# Toten Code aufräumen
/refactor-clean
```

### Der Workflow für ein neues Feature:

```bash
1. /plan                    # Feature planen lassen
2. /tdd                     # Mit Tests implementieren
3. /code-review            # Quality Check
4. /build-fix              # Falls was schief geht
5. Fertig! 🎉
```

---

## 🎯 Die 5 wichtigsten Commands für MVP-Development

### 1. `/plan` - Feature Planning

**Wann nutzen?** Bevor du ein neues Feature baust.

**Was passiert?** Claude analysiert deinen Code, schlägt eine Architektur vor und erstellt einen Step-by-Step Plan.

**Beispiel:**
```
Du: /plan
Du: Ich brauche ein Dashboard mit User-Stats und einem Export-Button

Claude:
✓ Analysiert deine bestehende Architektur
✓ Schlägt passende shadcn Komponenten vor
✓ Erstellt einen detaillierten Plan
✓ Identifiziert potenzielle Probleme
```

**Pro-Tipp:** Nach dem Plan kannst du einzelne Schritte anpassen bevor es losgeht.

---

### 2. `/tdd` - Test-Driven Development

**Wann nutzen?** Beim Bauen neuer Features oder Bugfixes.

**Was passiert?** Claude schreibt erst Tests, dann Code. Garantiert 80%+ Coverage.

**Workflow:**
```
1. /tdd
2. "Baue eine Suchfunktion für die User-Tabelle"
3. Claude:
   - Schreibt Tests
   - Implementiert Feature
   - Verifiziert dass Tests grün sind
4. Fertig - mit Tests!
```

**Pro-Tipp:** Auch für Bugfixes nutzen! Tests verhindern, dass der Bug zurückkommt.

---

### 3. `/code-review` - Automatisches Code Review

**Wann nutzen?** Vor dem Commit oder wenn du unsicher bist.

**Was wird geprüft?**
- Security (keine Secrets im Code)
- Performance (unnötige Re-Renders)
- Best Practices (shadcn Patterns)
- Code Quality (Type Safety, Error Handling)
- Maintainability

**Beispiel-Output:**
```
✓ Security: Keine Secrets gefunden
⚠ Performance: UserList re-rendert zu oft
  → Lösung: useMemo() für gefilterte Liste
✓ Type Safety: Alle Komponenten typisiert
⚠ Accessibility: Button braucht aria-label
```

---

### 4. `/build-fix` - Build Errors automatisch fixen

**Wann nutzen?** Wenn npm run build fehlschlägt.

**Was passiert?**
```
1. Claude führt Build aus
2. Analysiert Error Messages
3. Findet Root Cause
4. Fixt automatisch
5. Verifiziert dass Build grün ist
```

**Besonders stark bei:**
- TypeScript Errors
- Import/Export Problemen
- Dependency Konflikten
- ESLint Violations

---

### 5. `/refactor-clean` - Dead Code entfernen

**Wann nutzen?** Nach längeren Sessions oder vor einem Release.

**Was wird entfernt?**
- Ungenutzte Komponenten
- Commented Out Code
- Console.logs
- Leere Interfaces
- Duplizierter Code
- Temporary .md Files

**Pro-Tipp:** Nutze das regelmäßig! Hält Codebase clean und spart Tokens.

---

## 🎨 Shadcn-UI Workflows

### Neues Feature mit shadcn Komponenten

**Szenario:** Du brauchst ein User-Settings Panel.

```bash
/plan

"Baue ein Settings Panel mit:
- Tabs für Profile, Notifications, Billing
- Form mit shadcn Form Components
- Toast Notifications für Erfolg/Fehler
- Data Table für Billing History
```

**Claude macht automatisch:**
1. ✅ Nutzt shadcn/ui Komponenten (Button, Form, Tabs, Toast, Table)
2. ✅ Folgt deinem bestehenden Styling (CSS Variables)
3. ✅ Implementiert mit TypeScript + Zod Validation
4. ✅ Responsive Design
5. ✅ Accessibility (ARIA Labels, Keyboard Navigation)

---

### Shadcn Komponente anpassen

**Szenario:** Standard Button ist nicht genau was du brauchst.

```
"Erstelle eine Loading-Button Variante:
- Zeigt Spinner während Loading
- Disabled während Loading
- Behält shadcn Button Styling"
```

**Claude erstellt:**
```typescript
// components/ui/loading-button.tsx
import { Button } from "@/components/ui/button"
import { Loader2 } from "lucide-react"

export function LoadingButton({
  loading,
  children,
  ...props
}: ButtonProps & { loading?: boolean }) {
  return (
    <Button disabled={loading} {...props}>
      {loading && <Loader2 className="mr-2 h-4 w-4 animate-spin" />}
      {children}
    </Button>
  )
}
```

---

### Komplexe Forms mit shadcn

**Tipp:** Claude kennt die shadcn Form Patterns auswendig.

```
/tdd

"Baue ein Multi-Step Onboarding Form:

Step 1: Personal Info (Name, Email)
Step 2: Company Info (Name, Size)
Step 3: Preferences (Newsletter, Notifications)

Requirements:
- shadcn Form + react-hook-form + zod
- Validation pro Step
- Progress Indicator
- Zurück/Weiter Buttons
- Submit nur wenn alle Steps valid"
```

**Claude baut:**
- ✅ Type-safe Form mit Zod Schema
- ✅ Step-by-Step Navigation
- ✅ Validation Messages
- ✅ Progress Bar
- ✅ Animationen zwischen Steps

---

### Data Tables schnell bauen

**shadcn + TanStack Table = 💪**

```
"Erstelle eine User-Management Tabelle:
- Columns: Avatar, Name, Email, Role, Status, Actions
- Sorting auf allen Columns
- Filtering (Search Box)
- Pagination
- Bulk Actions (Delete, Export)
- Row Actions (Edit, Delete, Resend Invite)"
```

**Claude implementiert:**
- ✅ TanStack Table Setup
- ✅ Server/Client Components (Next.js App Router)
- ✅ Shadcn Table Components
- ✅ Action Dialogs (AlertDialog für Delete)
- ✅ Toast Notifications

---

## 💾 Sessions & Memory - Nie wieder Kontext verlieren

### Problem: Context Window voll

**Situation:** Du arbeitest seit 2 Stunden, Context bei 90%, musst aber weitermachen.

**Lösung: Session speichern**

```
"Speichere den aktuellen Stand als Session-File:
- Was haben wir gebaut?
- Welche Approaches funktionieren?
- Was ist noch offen?
- Nächste Steps?"
```

**Claude erstellt:** `.claude/sessions/2026-01-28-user-dashboard.md`

```markdown
# Session: User Dashboard Implementation

## Status: In Progress (70%)

## Was funktioniert:
✅ Dashboard Layout mit shadcn Card Components
✅ User Stats API Route (/api/dashboard/stats)
✅ Real-time Updates mit SWR
✅ Responsive Design (Mobile + Desktop)

## Was noch offen ist:
⏳ Export to CSV Funktion
⏳ Date Range Filter
⏳ Chart Integration (recharts)

## Nächste Steps:
1. CSV Export mit papaparse implementieren
2. shadcn Popover für Date Range Picker
3. Chart Components mit recharts + shadcn theme
```

**Nächste Session:**
```
"Lade die Session aus .claude/sessions/2026-01-28-user-dashboard.md
und mach mit Step 1 weiter"
```

---

### Automatische Memory Persistence (Advanced)

**Setup einmalig:**

Die Hooks im Repo machen das automatisch:
- `hooks/memory-persistence/session-start.js` - Lädt letzten Stand
- `hooks/memory-persistence/session-end.js` - Speichert automatisch

**Benefit:** Du fängst jeden Tag da an wo du aufgehört hast. Null Setup.

---

## 💰 Token & Kosten sparen

### 1. Context Window im Blick behalten

**Status Bar nutzen:**
```bash
/statusline

# Zeigt dir:
affaan:~/project ctx:65% Sonnet 19:52
```

**Wenn ctx > 80%:**
- Session speichern
- `/compact` ausführen
- Oder neu starten mit Session-File

---

### 2. Model Selection ist wichtig

**Regel:** Default = Sonnet (Best Balance)

**Wann Haiku nutzen?**
- Einfache Code-Änderungen
- Dateien suchen
- Dokumentation schreiben
- Schnelle Fragen

**Wann Opus nutzen?**
- Komplexe Architektur-Entscheidungen
- Security-kritischer Code
- Schwierige Bugs debuggen
- Multi-File Refactorings

**Wie Model wechseln?**
```bash
# In ~/.claude/settings.json
{
  "model": "sonnet"  // oder "haiku" oder "opus"
}

# Oder per Agent:
# agents/simple-tasks.md → model: haiku
# agents/security-reviewer.md → model: opus
```

---

### 3. MCPs ausschalten wenn nicht benötigt

**Problem:** Jedes MCP kostet Tokens (Tool-Definitionen im Context).

**Lösung:** Nur aktivieren was du brauchst.

```bash
/mcp

# Siehst alle aktiven MCPs
# Deaktiviere alles was du heute nicht brauchst
```

**Faustregel:**
- < 10 MCPs gleichzeitig
- < 80 Tools insgesamt

---

### 4. Cleanup vor großen Tasks

```bash
/refactor-clean

# Entfernt:
- Dead Code → Weniger Context
- Console.logs → Sauberer Code
- Unused Imports → Schnellere Builds
```

---

## 🎬 Häufige Szenarien

### Szenario 1: Neue Feature Page mit shadcn

```bash
Du: /plan

Du: "Ich brauche eine Blog-Post Seite:
- Markdown Rendering
- Table of Contents (automatisch aus Headings)
- Share Buttons (Twitter, LinkedIn, Copy Link)
- Reading Time Estimation
- Related Posts Section
- Comments (Disqus oder ähnlich)"

Claude: [Erstellt Plan mit allen shadcn Components]

Du: "Looks good, let's implement"

Claude: [Baut alles mit /tdd]

Du: /code-review

Claude: [Prüft Code]

Du: "Ship it!"
```

**Zeit:** 15-30 Minuten statt 2-3 Stunden.

---

### Szenario 2: Bug Fix mit Root Cause Analysis

```bash
Du: "User können sich nicht einloggen, error in console:
'TypeError: Cannot read property uid of undefined'"

Claude:
1. ✅ Analysiert Error
2. ✅ Findet betroffenen Code (auth/login.ts:42)
3. ✅ Identifiziert Root Cause (Supabase Response Format geändert)
4. ✅ Schreibt Test der den Bug reproduziert
5. ✅ Fixt Code
6. ✅ Verifiziert dass Test grün ist
7. ✅ Checkt ob andere Stellen betroffen sind

Du: "Perfect, commit it"
```

---

### Szenario 3: UI Refactoring mit Component Library

```bash
Du: "Refactor die Settings Page zu shadcn Components:
- Alte Custom Forms → shadcn Form
- Custom Modals → shadcn Dialog
- Custom Dropdowns → shadcn Select
- Inline Alerts → shadcn Alert
Aber: Funktionalität bleibt gleich!"

Claude:
1. ✅ Erstellt Branch
2. ✅ Refactored Komponente für Komponente
3. ✅ Behält Props/Types identisch
4. ✅ Updated Tests
5. ✅ Prüft dass alles noch funktioniert

Du: /code-review
Du: [Approve & Merge]
```

---

### Szenario 4: Performance Optimization

```bash
Du: "Die User-List Page ist super langsam mit 1000+ Users"

Claude:
1. ✅ Analysiert Component
2. ✅ Identifiziert Problems:
   - Re-renders bei jedem Keystroke
   - Keine Virtualisierung
   - Filtering im Render
   - Alle User-Daten auf einmal geladen
3. ✅ Schlägt Optimierungen vor:
   - useMemo für gefilterte Liste
   - TanStack Virtual für Liste
   - Debounced Search
   - Pagination/Infinite Scroll
4. ✅ Implementiert Schritt für Schritt
5. ✅ Misst Performance vorher/nachher

Du: "Much better! Ship it"
```

---

## ✨ Best Practices

### 1. Projekt-Regeln definieren

**Einmalig Setup:**

Erstelle `.claude/rules/meine-regeln.md`:

```markdown
# Projekt-Regeln

## UI/UX
- Immer shadcn/ui Components nutzen
- Dark Mode Support (via next-themes)
- Alle Buttons brauchen Loading States
- Toast für Success/Error Messages

## Code Style
- TypeScript Strict Mode
- ESLint + Prettier
- Keine console.logs in Production
- Components in components/ strukturieren

## Testing
- Mindestens 70% Coverage
- Unit Tests für Utils
- Integration Tests für API Routes

## Performance
- Images via next/image
- Dynamic Imports für große Components
- Memoization für teure Berechnungen
```

**Effekt:** Claude folgt automatisch deinen Regeln in JEDER Session.

---

### 2. Skills für wiederkehrende Tasks

**Beispiel: Dein eigener shadcn Workflow**

Erstelle `skills/shadcn-feature/SKILL.md`:

```markdown
# Shadcn Feature Development

## Workflow

1. **Komponenten identifizieren:**
   - Welche shadcn Components passen?
   - Müssen welche angepasst werden?

2. **Setup:**
   - shadcn add [components]
   - Types definieren
   - Zod Schemas für Forms

3. **Implementation:**
   - TDD: Tests first
   - shadcn Components nutzen
   - Proper Error Handling
   - Loading States
   - Accessibility

4. **Polish:**
   - Responsive Check
   - Dark Mode Check
   - Keyboard Navigation
   - Toast Notifications

## Checklist

- [ ] shadcn Components installiert
- [ ] TypeScript Types
- [ ] Zod Validation (bei Forms)
- [ ] Loading States
- [ ] Error Handling
- [ ] Toast Notifications
- [ ] Responsive Design
- [ ] Dark Mode Support
- [ ] Accessibility (ARIA)
- [ ] Tests (>70% Coverage)
```

**Nutzen:**
```
"Build ein neues Feature following /skills/shadcn-feature"
```

---

### 3. Command Chaining für Workflows

**Häufiger Workflow? → Chaine Commands!**

```bash
# Quick Ship Workflow
/plan && /tdd && /code-review && /build-fix

# Cleanup Workflow
/refactor-clean && /test-coverage && /code-review
```

**Noch besser: Eigenen Command erstellen**

`commands/quick-ship.md`:
```markdown
---
name: quick-ship
description: Full workflow - Plan, Build, Test, Review, Fix
---

Execute this workflow:

1. /plan - Create implementation plan
2. Wait for approval
3. /tdd - Implement with tests
4. /code-review - Quality check
5. /build-fix - Fix any build errors
6. Summary of changes
```

---

### 4. Git Workflow Integration

**Claude kann Commits für dich machen:**

```bash
Du: "Commit die Changes mit einer guten Message"

Claude:
git add [files]
git commit -m "feat: Add user dashboard with stats and export

- Implement dashboard layout with shadcn cards
- Add stats API route with real-time updates
- Add CSV export functionality with papaparse
- Add date range filter with shadcn popover
- Responsive design for mobile/desktop

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
```

**PR erstellen:**
```bash
Du: "Create a PR for this feature"

Claude:
[Erstellt Branch]
[Pushed Changes]
[Erstellt PR mit Summary + Test Plan]

Du: [Review PR Link] ✅
```

---

### 5. Testing Strategy

**Für MVPs: Pragmatisch testen**

**Was MUSS getestet werden:**
- ✅ Business Logic (Utils, Helpers)
- ✅ API Routes (Integration Tests)
- ✅ Custom Hooks
- ✅ Complex Components

**Was KANN warten:**
- ⏸️ Simple presentational Components
- ⏸️ Styling/Layout
- ⏸️ Third-Party Component Wrappers

**Test Commands:**
```bash
/tdd              # TDD Workflow
/test-coverage    # Coverage Check
/e2e              # E2E Tests (Playwright)
```

---

## 🚨 Troubleshooting

### Context Window voll

```bash
# Option 1: Kompakt
/compact

# Option 2: Session speichern
"Save current session state to .claude/sessions/"

# Option 3: Neue Session mit Kontext
"Load session from [file]"
```

---

### Build schlägt fehl

```bash
/build-fix

# Falls das nicht hilft:
"Show me the full error log"
[Copy Error]
"Fix this error: [Error]"
```

---

### Tests schlagen fehl

```bash
# Test Output zeigen
"Show me the test output"

# Fix einzelner Test
"Fix the failing test in [file]"

# Alle Tests fixen
"Fix all failing tests"
```

---

### Type Errors

```bash
# TypeScript Check
"Run tsc --noEmit and fix all errors"

# Specific Type Error
"Fix the type error in [file]:[line]"
```

---

## 🎓 Weiterführende Ressourcen

### In diesem Repo

- `/the-shortform-guide.md` - Complete Setup Guide
- `/the-longform-guide.md` - Advanced Patterns
- `/workflows/` - More workflow examples
- `/examples/` - Code examples

### Wichtige Commands

```bash
/help              # Hilfe
/plugins           # Plugins verwalten
/mcp               # MCPs verwalten
/statusline        # Status Bar konfigurieren
/fork              # Conversation forken
/rename            # Session umbenennen
```

### Community

- GitHub: `github.com/bastiantonn/everything-claude-code`
- Original Repo: `github.com/affaan-m/everything-claude-code`

---

## 🎯 Quick Wins für heute

**30 Minuten Setup, 10x Productivity:**

1. ✅ Lies diesen Guide (Done!)
2. ✅ Teste `/plan` und `/tdd` für ein kleines Feature
3. ✅ Erstelle `.claude/rules/meine-regeln.md` mit deinen Präferenzen
4. ✅ Nutze `/code-review` vor dem nächsten Commit
5. ✅ Setup `/statusline` um Context im Blick zu haben

**Next Level (diese Woche):**

1. Erstelle eigenen Skill für deinen shadcn Workflow
2. Setup Memory Persistence Hooks
3. Baue einen Command für deinen häufigsten Workflow
4. Experimentiere mit Agents für Code Review

---

**Happy Building! 🚀**

Bei Fragen: Issues auf GitHub oder direkt in deiner Claude Session fragen!
