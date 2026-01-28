# Shadcn MVP Workflow

> Kompletter Workflow für schnelles MVP-Development mit shadcn-ui Components

## Phase 1: Feature Planning (5-10 Min)

### Command: `/plan`

**Prompt-Template:**
```
Ich möchte [FEATURE] bauen:

User Story:
Als [USER] möchte ich [AKTION] um [ZIEL]

Requirements:
- [REQ 1]
- [REQ 2]
- [REQ 3]

Tech Stack:
- Next.js 14 App Router
- shadcn/ui Components
- TypeScript
- Tailwind CSS
- [Weitere...]

Constraints:
- Mobile-first
- Dark Mode Support
- Accessible (WCAG 2.1)
```

**Claude analysiert:**
- ✅ Bestehende Architektur
- ✅ Passende shadcn Components
- ✅ Notwendige Dependencies
- ✅ Mögliche Probleme
- ✅ Schritt-für-Schritt Plan

---

## Phase 2: Implementation mit TDD (30-60 Min)

### Command: `/tdd`

**Workflow:**

1. **Tests schreiben lassen:**
```
"Implement [FEATURE] mit TDD:

Components needed:
- [Component 1] (shadcn: [shadcn-component])
- [Component 2] (shadcn: [shadcn-component])

Business Logic:
- [Logic 1]
- [Logic 2]

Start with tests!"
```

2. **Claude macht automatisch:**
   - ✅ Test Setup
   - ✅ Component Tests
   - ✅ Integration Tests
   - ✅ Implementation
   - ✅ Test Verification

3. **Feedback Loop:**
```
"Tests sind grün!
Jetzt noch:
- Loading States hinzufügen
- Error Handling
- Toast Notifications
Mit Tests natürlich!"
```

---

## Phase 3: UI/UX Polish (15-30 Min)

### shadcn Components richtig nutzen

**Checklist für jedes Feature:**

```markdown
## shadcn Component Checklist

### Basis Components
- [ ] Button (mit Loading State)
- [ ] Form (react-hook-form + zod)
- [ ] Input / Textarea
- [ ] Label

### Feedback
- [ ] Toast (Erfolg/Fehler Messages)
- [ ] Alert (Warnungen)
- [ ] Skeleton (Loading States)

### Navigation
- [ ] Dialog/Sheet (Modals/Sidebars)
- [ ] Popover (Dropdowns/Tooltips)
- [ ] Tabs (Multi-View)

### Data Display
- [ ] Card (Content Container)
- [ ] Table (Data Tables)
- [ ] Badge (Status Indicators)
- [ ] Avatar (User Images)

### Accessibility
- [ ] Alle Buttons haben aria-labels
- [ ] Form Fields haben labels
- [ ] Keyboard Navigation funktioniert
- [ ] Focus States sichtbar
```

**Prompt:**
```
"Polish das UI:

1. Loading States:
   - Skeleton für initiales Loading
   - LoadingButton während Submit
   - Spinner für async actions

2. Error Handling:
   - Toast für Errors
   - Form Field Validation Messages
   - Fallback UI für Failed States

3. Accessibility:
   - Alle Buttons: aria-labels
   - Focus Management
   - Keyboard Navigation

4. Responsive:
   - Mobile: Sheet statt Dialog
   - Tablet: Adjusted spacing
   - Desktop: Full features

Nutze ausschließlich shadcn Components!"
```

---

## Phase 4: Code Review (10 Min)

### Command: `/code-review`

**Was wird geprüft:**

1. **Security:**
   - Keine API Keys im Code
   - Input Validation
   - XSS Prevention

2. **Performance:**
   - Unnecessary Re-renders
   - Memoization wo nötig
   - Image Optimization

3. **shadcn Best Practices:**
   - Components richtig genutzt
   - Theme Variables verwendet
   - Consistent Styling

4. **TypeScript:**
   - Proper Types
   - No `any`
   - Type Safety

5. **Accessibility:**
   - ARIA Labels
   - Keyboard Navigation
   - Screen Reader Support

---

## Phase 5: Testing & Build (5-10 Min)

### Commands:

```bash
# 1. Test Coverage prüfen
/test-coverage

# 2. Build testen
/build-fix

# 3. Falls E2E Tests nötig
/e2e
```

**Was Claude macht:**

1. **Coverage Check:**
   ```
   ✅ Components: 85% Coverage
   ✅ Utils: 92% Coverage
   ⚠️ API Routes: 65% Coverage
   → Empfehlung: Add more integration tests
   ```

2. **Build Verification:**
   ```
   ✅ TypeScript: No errors
   ✅ ESLint: No violations
   ✅ Build: Successful
   ✅ Bundle Size: Within limits
   ```

3. **E2E Tests (Optional):**
   ```
   ✅ User can login
   ✅ User can create item
   ✅ User can edit item
   ✅ User can delete item
   ✅ Error handling works
   ```

---

## Phase 6: Deployment (5 Min)

### Commit & Push

**Prompt:**
```
"Commit die Changes mit einer descriptive message.
Dann create a PR mit Summary und Test Plan."
```

**Claude macht:**

1. **Git Commit:**
```bash
git add .
git commit -m "feat: Add user dashboard with analytics

- Implement dashboard layout with shadcn cards
- Add real-time stats with SWR
- Add date range filter with shadcn popover
- Add CSV export functionality
- Responsive design for all screen sizes
- Full test coverage (87%)

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
```

2. **Pull Request:**
```markdown
## Summary
Implements user dashboard with real-time analytics and export functionality.

## Changes
- New `/dashboard` route with stats cards
- Date range filter component
- CSV export with papaparse
- shadcn components: Card, Popover, Button, Badge
- API route for stats aggregation
- Comprehensive test suite

## Test Plan
- [ ] Dashboard loads with correct data
- [ ] Date filter updates stats
- [ ] CSV export downloads correct file
- [ ] Responsive on mobile/tablet/desktop
- [ ] Dark mode works
- [ ] All tests pass

## Screenshots
[Screenshots attached]
```

---

## Beispiel: komplettes Feature in 60 Minuten

### Feature: User Onboarding Flow

**Phase 1: Plan (5 Min)**
```
/plan

"Multi-Step Onboarding für neue User:

Step 1: Willkommen + Name
Step 2: Company Info
Step 3: Präferenzen (Newsletter, Notifications)
Step 4: Success Screen mit Next Steps

Requirements:
- shadcn Form Components
- Progress Indicator (Steps 1-4)
- Validation pro Step
- Zurück/Weiter Navigation
- Skip Option für Step 3
- Persist zu Supabase nach Step 4"
```

**Phase 2: Implement (30 Min)**
```
/tdd

"Implement den Onboarding Flow:

Components:
- OnboardingWizard (Parent)
- StepIndicator (Progress)
- WelcomeStep (Step 1)
- CompanyStep (Step 2)
- PreferencesStep (Step 3)
- SuccessStep (Step 4)

shadcn Components:
- Form + react-hook-form + zod
- Card (Step Container)
- Button (Navigation)
- Input, Select, Checkbox
- Progress (Step Indicator)

Validation:
- Step 1: Name required (min 2 chars)
- Step 2: Company required
- Step 3: Optional
- Navigate nur wenn valid

Start with tests!"
```

**Phase 3: Polish (15 Min)**
```
"Polish the Onboarding:

1. Animations:
   - Fade in/out zwischen Steps
   - Slide transitions

2. Loading States:
   - Skeleton während Init
   - Loading Button bei Submit

3. Error Handling:
   - Toast bei API Errors
   - Form Validation Messages

4. Responsive:
   - Mobile: Full Screen
   - Desktop: Centered Card

5. Accessibility:
   - Focus Management zwischen Steps
   - Keyboard Navigation (Enter/Tab)
   - Screen Reader Announcements"
```

**Phase 4: Review (5 Min)**
```
/code-review
```

**Phase 5: Test & Build (5 Min)**
```
/test-coverage
/build-fix
```

**Phase 6: Ship (5 Min)**
```
"Commit and create PR"
```

**Total: ~60 Minuten** für komplettes, getestetes, polished Feature! 🚀

---

## Pro-Tips

### 1. shadcn Components vorher installieren

```bash
# Alle häufig genutzten installieren
npx shadcn@latest add button form input label card dialog toast table
```

**Dann in Prompt erwähnen:**
```
"Nutze die bereits installierten shadcn components:
button, form, input, label, card, dialog, toast, table"
```

### 2. Theme Variables nutzen

```
"Nutze die shadcn theme variables für alle colors:
- background / foreground
- primary / secondary
- muted / accent
- destructive
Keine hardcoded colors!"
```

### 3. Component Patterns dokumentieren

Erstelle `docs/component-patterns.md`:
```markdown
# Component Patterns

## Loading Button Pattern
[Beispiel Code]

## Form with Toast Pattern
[Beispiel Code]

## Data Table with Actions Pattern
[Beispiel Code]
```

**Dann:**
```
"Implement following /docs/component-patterns.md"
```

### 4. Reusable Compositions

```
"Erstelle reusable compositions:

1. FormCard:
   - Card Container
   - Form mit shadcn
   - Submit Button mit Loading
   - Toast Notifications

2. DataTableWrapper:
   - Table mit TanStack
   - Sorting, Filtering, Pagination
   - Actions Column
   - Row Selection

Save in /components/compositions/"
```

---

## Cheat Sheet

```bash
# Feature bauen
/plan → /tdd → /code-review → /build-fix

# Nur UI ändern
"Update UI to use shadcn [component]"

# Form erstellen
"Create form with shadcn + react-hook-form + zod"

# Table erstellen
"Create data table with TanStack Table + shadcn"

# Loading States
"Add loading states with skeleton"

# Error Handling
"Add error handling with toast"

# Responsive
"Make responsive: mobile-first"

# Dark Mode
"Verify dark mode works"

# Accessibility
"Add ARIA labels and keyboard navigation"
```

---

**Mit diesem Workflow: Features in Stunden statt Tagen! ⚡**
