# Shadcn-ui Komponenten - Praktische Beispiele

> Konkrete Prompts und Code-Beispiele für häufige shadcn Use Cases

## Inhaltsverzeichnis

- [Forms & Validation](#forms--validation)
- [Data Tables](#data-tables)
- [Dialogs & Modals](#dialogs--modals)
- [Loading States](#loading-states)
- [Notifications](#notifications)
- [Navigation](#navigation)
- [Advanced Patterns](#advanced-patterns)

---

## Forms & Validation

### Einfaches Login Form

**Prompt:**
```
Erstelle ein Login Form mit:
- shadcn Form + react-hook-form + zod
- Email Field (validation: valid email)
- Password Field (validation: min 8 chars)
- Remember Me Checkbox
- Loading Button mit Spinner
- Toast für Errors
- Redirect nach Success
```

**Erwartetes Ergebnis:**

```typescript
// components/auth/login-form.tsx
"use client"

import { zodResolver } from "@hookform/resolvers/zod"
import { useForm } from "react-hook-form"
import * as z from "zod"
import { Button } from "@/components/ui/button"
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form"
import { Input } from "@/components/ui/input"
import { Checkbox } from "@/components/ui/checkbox"
import { useToast } from "@/components/ui/use-toast"
import { Loader2 } from "lucide-react"
import { useState } from "react"
import { useRouter } from "next/navigation"

const formSchema = z.object({
  email: z.string().email("Ungültige Email-Adresse"),
  password: z.string().min(8, "Passwort muss mindestens 8 Zeichen haben"),
  rememberMe: z.boolean().default(false),
})

export function LoginForm() {
  const [loading, setLoading] = useState(false)
  const { toast } = useToast()
  const router = useRouter()

  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      email: "",
      password: "",
      rememberMe: false,
    },
  })

  async function onSubmit(values: z.infer<typeof formSchema>) {
    setLoading(true)
    try {
      // API Call
      const response = await fetch("/api/auth/login", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(values),
      })

      if (!response.ok) throw new Error("Login fehlgeschlagen")

      toast({
        title: "Erfolgreich angemeldet!",
        description: "Du wirst weitergeleitet...",
      })

      router.push("/dashboard")
    } catch (error) {
      toast({
        title: "Login fehlgeschlagen",
        description: "Bitte überprüfe deine Anmeldedaten",
        variant: "destructive",
      })
    } finally {
      setLoading(false)
    }
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Email</FormLabel>
              <FormControl>
                <Input placeholder="name@example.com" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="password"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Passwort</FormLabel>
              <FormControl>
                <Input type="password" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="rememberMe"
          render={({ field }) => (
            <FormItem className="flex items-center space-x-2">
              <FormControl>
                <Checkbox
                  checked={field.value}
                  onCheckedChange={field.onChange}
                />
              </FormControl>
              <FormLabel className="!mt-0">Angemeldet bleiben</FormLabel>
            </FormItem>
          )}
        />
        <Button type="submit" className="w-full" disabled={loading}>
          {loading && <Loader2 className="mr-2 h-4 w-4 animate-spin" />}
          Anmelden
        </Button>
      </form>
    </Form>
  )
}
```

---

### Multi-Step Form mit Wizard

**Prompt:**
```
Erstelle ein Multi-Step Form Wizard:

Steps:
1. Personal Info (Name, Email)
2. Address (Street, City, Zip, Country)
3. Preferences (Newsletter, Marketing)
4. Review & Submit

Features:
- Progress Indicator (1/4, 2/4, etc.)
- Zurück/Weiter Buttons
- Validation pro Step
- Review Screen zeigt alle Daten
- Submit speichert alles
- shadcn Components

Validation:
- Name: required, min 2 chars
- Email: required, valid email
- Address: all required
- Preferences: optional
```

**Key Components:**

```typescript
// components/wizard/progress-indicator.tsx
export function ProgressIndicator({ currentStep, totalSteps }: Props) {
  return (
    <div className="w-full">
      <div className="flex justify-between mb-2">
        {Array.from({ length: totalSteps }).map((_, index) => (
          <div
            key={index}
            className={cn(
              "h-2 flex-1 mx-1 rounded-full",
              index < currentStep ? "bg-primary" : "bg-muted"
            )}
          />
        ))}
      </div>
      <p className="text-sm text-muted-foreground text-center">
        Schritt {currentStep} von {totalSteps}
      </p>
    </div>
  )
}
```

---

## Data Tables

### User Management Table

**Prompt:**
```
Erstelle eine User Management Table:

Columns:
- Avatar (Bild oder Initialen)
- Name (sortable)
- Email (sortable)
- Role (Badge: Admin/User)
- Status (Badge: Active/Inactive)
- Actions (Dropdown: Edit, Delete, Resend Invite)

Features:
- TanStack Table
- Sorting auf allen columns
- Search Box (filter nach Name/Email)
- Pagination (10/25/50/100 per page)
- Bulk Actions (Select all, Delete selected)
- Row Actions Menu
- Loading States (Skeleton)
- Empty State
- shadcn Components

API:
- GET /api/users (mit pagination, sorting, filtering)
- DELETE /api/users/:id
- POST /api/users/:id/resend-invite
```

**Expected Structure:**

```typescript
// components/users/users-table.tsx
"use client"

import { useState } from "react"
import {
  ColumnDef,
  flexRender,
  getCoreRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  getFilteredRowModel,
  useReactTable,
} from "@tanstack/react-table"
import { User } from "@/types"
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import { Checkbox } from "@/components/ui/checkbox"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"
import { Input } from "@/components/ui/input"
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table"
import { MoreHorizontal, ArrowUpDown } from "lucide-react"

export const columns: ColumnDef<User>[] = [
  {
    id: "select",
    header: ({ table }) => (
      <Checkbox
        checked={table.getIsAllPageRowsSelected()}
        onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
      />
    ),
    cell: ({ row }) => (
      <Checkbox
        checked={row.getIsSelected()}
        onCheckedChange={(value) => row.toggleSelected(!!value)}
      />
    ),
  },
  {
    accessorKey: "avatar",
    header: "User",
    cell: ({ row }) => {
      const user = row.original
      return (
        <div className="flex items-center gap-2">
          <Avatar>
            <AvatarImage src={user.avatar} />
            <AvatarFallback>{user.name.substring(0, 2)}</AvatarFallback>
          </Avatar>
          <div>
            <div className="font-medium">{user.name}</div>
            <div className="text-sm text-muted-foreground">{user.email}</div>
          </div>
        </div>
      )
    },
  },
  {
    accessorKey: "role",
    header: "Role",
    cell: ({ row }) => {
      const role = row.getValue("role") as string
      return (
        <Badge variant={role === "admin" ? "default" : "secondary"}>
          {role}
        </Badge>
      )
    },
  },
  {
    accessorKey: "status",
    header: "Status",
    cell: ({ row }) => {
      const status = row.getValue("status") as string
      return (
        <Badge variant={status === "active" ? "success" : "secondary"}>
          {status}
        </Badge>
      )
    },
  },
  {
    id: "actions",
    cell: ({ row }) => {
      const user = row.original
      return (
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <Button variant="ghost" size="icon">
              <MoreHorizontal className="h-4 w-4" />
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            <DropdownMenuItem onClick={() => handleEdit(user)}>
              Edit
            </DropdownMenuItem>
            <DropdownMenuItem onClick={() => handleResendInvite(user)}>
              Resend Invite
            </DropdownMenuItem>
            <DropdownMenuItem
              onClick={() => handleDelete(user)}
              className="text-destructive"
            >
              Delete
            </DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      )
    },
  },
]
```

---

## Dialogs & Modals

### Confirmation Dialog

**Prompt:**
```
Erstelle einen reusable Confirmation Dialog:

Props:
- title (string)
- description (string)
- confirmText (default: "Confirm")
- cancelText (default: "Cancel")
- onConfirm (function)
- onCancel (function)
- variant ("default" | "destructive")

Features:
- shadcn AlertDialog
- Loading State auf Confirm Button
- Keyboard Support (Enter/Escape)
- Focus Management
```

**Example:**

```typescript
// components/ui/confirmation-dialog.tsx
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
} from "@/components/ui/alert-dialog"
import { Loader2 } from "lucide-react"

interface ConfirmationDialogProps {
  open: boolean
  onOpenChange: (open: boolean) => void
  title: string
  description: string
  confirmText?: string
  cancelText?: string
  onConfirm: () => Promise<void> | void
  variant?: "default" | "destructive"
  loading?: boolean
}

export function ConfirmationDialog({
  open,
  onOpenChange,
  title,
  description,
  confirmText = "Bestätigen",
  cancelText = "Abbrechen",
  onConfirm,
  variant = "default",
  loading = false,
}: ConfirmationDialogProps) {
  return (
    <AlertDialog open={open} onOpenChange={onOpenChange}>
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>{title}</AlertDialogTitle>
          <AlertDialogDescription>{description}</AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel disabled={loading}>
            {cancelText}
          </AlertDialogCancel>
          <AlertDialogAction
            onClick={onConfirm}
            disabled={loading}
            className={variant === "destructive" ? "bg-destructive" : ""}
          >
            {loading && <Loader2 className="mr-2 h-4 w-4 animate-spin" />}
            {confirmText}
          </AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  )
}
```

**Usage:**
```typescript
const [deleteDialogOpen, setDeleteDialogOpen] = useState(false)
const [deleting, setDeleting] = useState(false)

async function handleDelete() {
  setDeleting(true)
  try {
    await deleteUser(user.id)
    toast({ title: "User gelöscht" })
    setDeleteDialogOpen(false)
  } catch (error) {
    toast({ title: "Fehler", variant: "destructive" })
  } finally {
    setDeleting(false)
  }
}

return (
  <>
    <Button onClick={() => setDeleteDialogOpen(true)}>Delete</Button>
    <ConfirmationDialog
      open={deleteDialogOpen}
      onOpenChange={setDeleteDialogOpen}
      title="User löschen?"
      description="Diese Aktion kann nicht rückgängig gemacht werden."
      confirmText="Löschen"
      onConfirm={handleDelete}
      variant="destructive"
      loading={deleting}
    />
  </>
)
```

---

## Loading States

### Skeleton für Cards

**Prompt:**
```
Erstelle Skeleton Loading States für:

1. Card mit Avatar, Name, Email, Stats
2. Data Table (5 rows)
3. Form (3 fields + button)

shadcn Skeleton Component nutzen.
```

**Examples:**

```typescript
// components/skeletons/user-card-skeleton.tsx
import { Card, CardHeader, CardContent } from "@/components/ui/card"
import { Skeleton } from "@/components/ui/skeleton"

export function UserCardSkeleton() {
  return (
    <Card>
      <CardHeader className="flex flex-row items-center gap-4">
        <Skeleton className="h-12 w-12 rounded-full" />
        <div className="space-y-2">
          <Skeleton className="h-4 w-32" />
          <Skeleton className="h-3 w-48" />
        </div>
      </CardHeader>
      <CardContent>
        <div className="grid grid-cols-3 gap-4">
          {[...Array(3)].map((_, i) => (
            <div key={i} className="space-y-2">
              <Skeleton className="h-8 w-full" />
              <Skeleton className="h-3 w-16 mx-auto" />
            </div>
          ))}
        </div>
      </CardContent>
    </Card>
  )
}
```

---

## Notifications

### Toast Notifications

**Prompt:**
```
Zeige mir Best Practices für Toast Notifications:

Use Cases:
- Success (nach Create/Update/Delete)
- Error (API Errors, Validation Errors)
- Info (Background Tasks)
- Warning (Confirmation needed)

Mit shadcn Toast Component.
```

**Examples:**

```typescript
// lib/toast-helpers.ts
import { toast } from "@/components/ui/use-toast"

export const toastSuccess = (title: string, description?: string) => {
  toast({
    title,
    description,
  })
}

export const toastError = (title: string, description?: string) => {
  toast({
    title,
    description,
    variant: "destructive",
  })
}

export const toastInfo = (title: string, description?: string) => {
  toast({
    title,
    description,
  })
}

export const toastLoading = (title: string) => {
  return toast({
    title,
    description: "Bitte warten...",
    duration: Infinity, // Manually dismiss
  })
}

// Usage:
const loadingToast = toastLoading("Daten werden gespeichert")
try {
  await saveData()
  loadingToast.dismiss()
  toastSuccess("Gespeichert!")
} catch (error) {
  loadingToast.dismiss()
  toastError("Fehler beim Speichern")
}
```

---

## Navigation

### Breadcrumbs

**Prompt:**
```
Erstelle eine Breadcrumb Component:

Features:
- Auto-generate aus URL path
- Custom labels support
- Icons support
- Responsive (collapsible auf mobile)
- shadcn Separator

Example:
/dashboard/users/123/edit
→ Home / Dashboard / Users / John Doe / Edit
```

---

### Tabs Navigation

**Prompt:**
```
Erstelle eine Settings Page mit Tabs:

Tabs:
- Profile (Avatar, Name, Email, Bio)
- Account (Password, 2FA, Sessions)
- Notifications (Email, Push, Preferences)
- Billing (Plan, Payment, Invoices)

Features:
- shadcn Tabs
- URL-based Tab Selection (?tab=profile)
- Form pro Tab
- Save Button pro Tab
- Dirty State Indicator
```

---

## Advanced Patterns

### Command Palette

**Prompt:**
```
Erstelle eine Command Palette (wie Spotlight/CMD+K):

Features:
- Keyboard Shortcut: CMD+K / CTRL+K
- Search across:
  - Pages/Routes
  - Recent Items
  - Actions (Create, Delete, etc.)
- Keyboard Navigation (Arrow Keys, Enter, Escape)
- Groups (Pages, Actions, Settings)
- Icons für jede Option
- shadcn Command + Dialog

Example Actions:
- "New User" → /users/new
- "Settings" → /settings
- "Go to Dashboard" → /dashboard
- "Log out" → logout()
```

---

### Combobox with Server Search

**Prompt:**
```
Erstelle ein User-Select Combobox:

Features:
- Suche server-side (API: /api/users/search)
- Debounced Search (300ms)
- Loading State
- Empty State
- Infinite Scroll (Load more)
- Keyboard Navigation
- shadcn Combobox

Display:
- Avatar + Name + Email
```

---

## Prompt Templates für häufige Aufgaben

### Neues Feature mit Form
```
Erstelle [FEATURE NAME]:

Form Fields:
- [Field 1] ([Type], [Validation])
- [Field 2] ([Type], [Validation])

shadcn Components:
- Form + react-hook-form + zod
- [weitere Components]

Features:
- Loading States
- Error Handling (Toast)
- Success Redirect
- Accessibility

API:
- POST /api/[endpoint]
```

### Data Table
```
Erstelle eine [ENTITY] Table:

Columns:
- [Column 1] ([Type], sortable/filterable)
- [Column 2] ([Type], sortable/filterable)

Features:
- TanStack Table
- Sorting, Filtering, Pagination
- Bulk Actions
- Row Actions
- shadcn Table Components

API:
- GET /api/[endpoint]
```

### Modal Dialog
```
Erstelle ein [ACTION] Dialog:

Trigger: [Button/Link]

Content:
- [Form/Content Description]

Actions:
- Cancel (closes dialog)
- [Primary Action] (async, loading state, toast on success/error)

shadcn Components:
- Dialog
- [Form Components]
```

---

**Mit diesen Beispielen und Prompts: Shadcn Components in Minuten statt Stunden! 🎨**
