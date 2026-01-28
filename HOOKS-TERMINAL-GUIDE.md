# 🪝 Hooks Terminal Guide

> Genau wie du Hooks im Terminal mit Claude Code verwendest

## Was sind Hooks?

Hooks sind Shell-Commands, die **automatisch** bei bestimmten Events ausgeführt werden:
- Vor/nach Tool-Aufrufen
- Beim Session-Start/-End
- Vor Context-Compaction
- Bei User-Prompt-Submit

---

## ⚙️ Hooks konfigurieren

### Location: `~/.claude/settings.json`

```json
{
  "hooks": {
    "preToolUse": {
      "command": "echo '[PRE] Tool: {{toolName}}'",
      "enabled": true
    },
    "postToolUse": {
      "command": "echo '[POST] Tool: {{toolName}} - Status: {{exitCode}}'",
      "enabled": true
    },
    "preCompact": {
      "command": "echo '[COMPACT] Tokens: {{tokenUsage}}'",
      "enabled": true
    },
    "sessionStart": {
      "command": "echo '[START] Session in {{workingDir}}'",
      "enabled": true
    },
    "sessionEnd": {
      "command": "echo '[END] Session beendet'",
      "enabled": true
    },
    "userPromptSubmit": {
      "command": "echo '[PROMPT] User: {{promptPreview}}'",
      "enabled": true
    }
  }
}
```

---

## 🎯 Hook Types & Variables

### 1. preToolUse - Vor jedem Tool-Aufruf

**Variables:**
- `{{toolName}}` - Name des Tools (z.B. "Read", "Bash", "Edit")
- `{{workingDir}}` - Aktuelles Working Directory
- `{{timestamp}}` - Unix Timestamp

**Beispiel:**
```json
{
  "preToolUse": {
    "command": "echo '[🔧 Pre] Tool: {{toolName}} | Dir: {{workingDir}}'",
    "enabled": true
  }
}
```

**Terminal Output:**
```
[🔧 Pre] Tool: Read | Dir: /Users/btonn/Desktop/DEV/my-project
```

---

### 2. postToolUse - Nach jedem Tool-Aufruf

**Variables:**
- `{{toolName}}` - Name des Tools
- `{{exitCode}}` - Exit Code (0 = success, 1 = error)
- `{{duration}}` - Duration in ms
- `{{workingDir}}` - Working Directory

**Beispiel:**
```json
{
  "postToolUse": {
    "command": "echo '[✅ Post] {{toolName}} - Code: {{exitCode}} ({{duration}}ms)'",
    "enabled": true
  }
}
```

**Terminal Output:**
```
[✅ Post] Read - Code: 0 (45ms)
[✅ Post] Bash - Code: 0 (1234ms)
```

---

### 3. preCompact - Vor Context Window Compaction

**Variables:**
- `{{tokenUsage}}` - Aktuelle Token Usage
- `{{maxTokens}}` - Max Tokens
- `{{workingDir}}` - Working Directory

**Beispiel:**
```json
{
  "preCompact": {
    "command": "echo '[💾 Compact] Tokens: {{tokenUsage}}/{{maxTokens}}'",
    "enabled": true
  }
}
```

**Terminal Output:**
```
[💾 Compact] Tokens: 185432/200000
```

---

### 4. sessionStart - Beim Session Start

**Variables:**
- `{{workingDir}}` - Working Directory
- `{{timestamp}}` - Unix Timestamp
- `{{sessionId}}` - Session ID

**Beispiel:**
```json
{
  "sessionStart": {
    "command": "echo '[🚀 Start] Session in {{workingDir}}'",
    "enabled": true
  }
}
```

**Terminal Output:**
```
[🚀 Start] Session in /Users/btonn/Desktop/DEV/everything-claude-code
```

---

### 5. sessionEnd - Beim Session End

**Variables:**
- `{{workingDir}}` - Working Directory
- `{{timestamp}}` - Unix Timestamp
- `{{sessionId}}` - Session ID

**Beispiel:**
```json
{
  "sessionEnd": {
    "command": "echo '[🛑 End] Session beendet - $(date)'",
    "enabled": true
  }
}
```

**Terminal Output:**
```
[🛑 End] Session beendet - Tue Jan 28 15:30:45 CET 2026
```

---

### 6. userPromptSubmit - Wenn User Prompt sendet

**Variables:**
- `{{promptPreview}}` - First 50 chars of prompt
- `{{promptLength}}` - Length in characters
- `{{workingDir}}` - Working Directory

**Beispiel:**
```json
{
  "userPromptSubmit": {
    "command": "echo '[💬 Prompt] Länge: {{promptLength}} - {{promptPreview}}...'",
    "enabled": true
  }
}
```

**Terminal Output:**
```
[💬 Prompt] Länge: 245 - Baue ein User Dashboard mit Stats Cards und...
```

---

## 🔥 Praktische Beispiele

### 1. Git Auto-Status nach jedem Tool

**Use Case:** Nach jedem Tool automatisch Git Status zeigen

```json
{
  "postToolUse": {
    "command": "cd {{workingDir}} && git status -s | head -5",
    "enabled": true
  }
}
```

**Output:**
```
M  src/components/Dashboard.tsx
A  src/lib/utils.ts
```

---

### 2. Token Tracking

**Use Case:** Tokens im Blick behalten

```json
{
  "preCompact": {
    "command": "echo '⚠️ TOKEN WARNING: {{tokenUsage}}/{{maxTokens}}' && osascript -e 'display notification \"Context wird komprimiert\" with title \"Claude Token Warning\"'",
    "enabled": true
  }
}
```

**Output:**
- Terminal: `⚠️ TOKEN WARNING: 185432/200000`
- macOS Notification: "Context wird komprimiert"

---

### 3. Session Logger

**Use Case:** Alle Sessions in File loggen

**sessionStart:**
```json
{
  "sessionStart": {
    "command": "echo \"$(date) - START - {{workingDir}}\" >> ~/.claude/session-log.txt",
    "enabled": true
  }
}
```

**sessionEnd:**
```json
{
  "sessionEnd": {
    "command": "echo \"$(date) - END - {{workingDir}}\" >> ~/.claude/session-log.txt",
    "enabled": true
  }
}
```

**Result:** `~/.claude/session-log.txt`
```
Tue Jan 28 10:15:30 CET 2026 - START - /Users/btonn/Desktop/DEV/gmailctl
Tue Jan 28 12:45:22 CET 2026 - END - /Users/btonn/Desktop/DEV/gmailctl
Tue Jan 28 14:20:18 CET 2026 - START - /Users/btonn/Desktop/DEV/vetsak-retail
```

---

### 4. Build Auto-Run nach Code-Änderungen

**Use Case:** Nach Edit/Write automatisch Build starten

```json
{
  "postToolUse": {
    "command": "if [ '{{toolName}}' = 'Edit' ] || [ '{{toolName}}' = 'Write' ]; then cd {{workingDir}} && npm run build 2>&1 | head -10; fi",
    "enabled": true
  }
}
```

---

### 5. Test Auto-Run

**Use Case:** Nach Code-Änderung Tests laufen lassen

```json
{
  "postToolUse": {
    "command": "if [ '{{toolName}}' = 'Edit' ]; then cd {{workingDir}} && npm test 2>&1 | tail -20; fi",
    "enabled": true
  }
}
```

---

### 6. Slack/Discord Notification bei Session End

**Use Case:** Team benachrichtigen wenn du mit Session fertig bist

```json
{
  "sessionEnd": {
    "command": "curl -X POST https://hooks.slack.com/services/YOUR/WEBHOOK/URL -H 'Content-Type: application/json' -d '{\"text\":\"Claude Session beendet in {{workingDir}}\"}'",
    "enabled": true
  }
}
```

---

### 7. Backup vor Compaction

**Use Case:** Session sichern bevor Context komprimiert wird

```json
{
  "preCompact": {
    "command": "cd {{workingDir}} && cp .claude/sessions/current.md .claude/sessions/backup-$(date +%s).md && echo 'Backup erstellt'",
    "enabled": true
  }
}
```

---

## 🛠️ Hooks aktivieren/deaktivieren

### Im Terminal

```bash
# Settings öffnen
code ~/.claude/settings.json

# Oder mit vim
vim ~/.claude/settings.json

# Oder direkt editieren
cat >> ~/.claude/settings.json << 'EOF'
{
  "hooks": {
    "postToolUse": {
      "command": "echo 'Tool done: {{toolName}}'",
      "enabled": true
    }
  }
}
EOF
```

### Hook deaktivieren

```json
{
  "postToolUse": {
    "command": "echo 'Tool done'",
    "enabled": false  // ← einfach auf false setzen
  }
}
```

---

## 🚨 Hook Blocking

**Wenn Hook fehlschlägt (Exit Code != 0), wird Tool-Aufruf blockiert!**

**Beispiel:**
```json
{
  "preToolUse": {
    "command": "[ '{{toolName}}' != 'Bash' ] || (echo 'Bash nicht erlaubt!' && exit 1)",
    "enabled": true
  }
}
```

**Result:** Alle Bash-Aufrufe werden blockiert!

**Terminal Output:**
```
❌ Hook failed: Bash nicht erlaubt!
Tool call blocked.
```

---

## 📊 Debug: Hooks testen

### 1. Hook Output sehen

Hooks laufen im Hintergrund. Output siehst du im Terminal von Claude Code.

**Test:**
```json
{
  "userPromptSubmit": {
    "command": "echo '=== HOOK TRIGGERED ===' && echo 'Prompt: {{promptPreview}}'",
    "enabled": true
  }
}
```

**Dann im Claude Code Terminal:**
```
Sende irgendein Prompt
→ Terminal zeigt:
=== HOOK TRIGGERED ===
Prompt: Sende irgendein Prompt
```

---

### 2. Hook in File schreiben

```json
{
  "preToolUse": {
    "command": "echo \"$(date) - {{toolName}}\" >> /tmp/claude-hooks.log",
    "enabled": true
  }
}
```

**Checken:**
```bash
tail -f /tmp/claude-hooks.log
```

---

## 💡 Best Practices

### ✅ DO:

1. **Kurze Commands verwenden**
   ```json
   "command": "echo '[Tool] {{toolName}}'"
   ```

2. **Error Handling**
   ```json
   "command": "cd {{workingDir}} && npm test || echo 'Tests failed'"
   ```

3. **Background Tasks mit &**
   ```json
   "command": "npm run build > /dev/null 2>&1 &"
   ```

4. **Conditional Execution**
   ```json
   "command": "[ '{{toolName}}' = 'Edit' ] && echo 'File edited!'"
   ```

### ❌ DON'T:

1. **Lange Commands die blocken**
   ```json
   "command": "npm install && npm run build"  // ❌ Dauert zu lange
   ```

2. **Interactive Commands**
   ```json
   "command": "vim file.txt"  // ❌ Wartet auf User Input
   ```

3. **Commands ohne Error Handling**
   ```json
   "command": "cd /non-existent && ls"  // ❌ Failing Hook blockt Tool
   ```

---

## 🎯 Meine empfohlene Starter-Config

```json
{
  "hooks": {
    "sessionStart": {
      "command": "echo '\n🚀 Claude Session gestartet in {{workingDir}}\n'",
      "enabled": true
    },
    "sessionEnd": {
      "command": "echo '\n🛑 Session beendet - $(date)\n'",
      "enabled": true
    },
    "preCompact": {
      "command": "echo '\n💾 Context Compaction bei {{tokenUsage}}/{{maxTokens}} Tokens\n'",
      "enabled": true
    },
    "postToolUse": {
      "command": "[ '{{exitCode}}' != '0' ] && echo '❌ {{toolName}} failed!' || true",
      "enabled": true
    }
  }
}
```

**Was macht das?**
- Session Start/End Notifications
- Token Warning vor Compaction
- Error Notifications bei Tool Failures

---

## 🔄 Reload nach Config-Änderung

**Hooks werden automatisch reloaded!**

Du musst Claude Code NICHT neu starten nach `settings.json` Änderungen.

**Test:**
```bash
# 1. Hook hinzufügen
echo '{"hooks":{"sessionStart":{"command":"echo TEST","enabled":true}}}' > ~/.claude/settings.json

# 2. Neue Session starten
claude

# → "TEST" sollte erscheinen
```

---

## 📞 Troubleshooting

### Hook wird nicht ausgeführt

**Check 1: Enabled?**
```json
"enabled": true  // ← muss true sein
```

**Check 2: Command valide?**
```bash
# Hook Command im Terminal testen
echo '[Tool] Read'  # Funktioniert?
```

**Check 3: Variables escaped?**
```json
// ❌ Falsch
"command": "echo \"Tool: {{toolName}}\""

// ✅ Richtig
"command": "echo 'Tool: {{toolName}}'"
```

---

### Hook blockt Tools

**Problem:** Hook failed → Tool wird blockiert

**Solution 1: Error ignorieren**
```json
"command": "my-command || true"  // ← ignoriert Errors
```

**Solution 2: Hook deaktivieren**
```json
"enabled": false
```

---

## 🎓 Advanced: Multi-Command Hooks

**Mehrere Commands ausführen:**

```json
{
  "sessionStart": {
    "command": "echo '[START]' && git status -s && npm test 2>&1 | tail -5",
    "enabled": true
  }
}
```

**Mit Subshell:**
```json
{
  "postToolUse": {
    "command": "(cd {{workingDir}} && npm run lint && npm test) || echo 'Checks failed'",
    "enabled": true
  }
}
```

---

## 🚀 Quick Start

### 1. Copy & Paste diese Config:

```bash
cat > ~/.claude/settings.json << 'EOF'
{
  "hooks": {
    "sessionStart": {
      "command": "echo '🚀 Claude Session START'",
      "enabled": true
    },
    "sessionEnd": {
      "command": "echo '🛑 Session END'",
      "enabled": true
    }
  }
}
EOF
```

### 2. Teste es:

```bash
claude
# → Sollte "🚀 Claude Session START" zeigen

# Exit
exit
# → Sollte "🛑 Session END" zeigen
```

### 3. Erweitere die Config nach Bedarf!

---

## 📚 Weitere Infos

- **Settings Location:** `~/.claude/settings.json`
- **Session Logs:** `~/.claude/sessions/`
- **Hook Execution:** Synchronous (blockt bei Failure)
- **Variables:** Siehe jeweilige Hook Type Section oben

---

**Happy Hooking! 🪝**

Bei Fragen einfach in Claude fragen!
