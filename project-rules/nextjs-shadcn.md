# Next.js 14+ · TypeScript · Tailwind · shadcn/ui — 项目规则

> 适用场景：现代全栈 Web 应用。放置在 `.cursor/rules/01-nextjs-shadcn.mdc` 或 `.github/copilot-instructions.md`

---

## 🔧 技术栈（Stack）

- Framework: Next.js 14/15 with App Router (Pages Router only if explicitly requested)
- Language: TypeScript 5.5+ with strict mode enabled
- Styling: Tailwind CSS v3/v4 with shadcn/ui component library
- State: Zustand for client state, React Query (@tanstack/react-query) for server state
- Forms: React Hook Form with Zod validation schemas
- Icons: lucide-react (preferred), @radix-ui/react-icons (for shadcn/ui primitives)
- Utilities: clsx + tailwind-merge via `cn()` helper in `@/lib/utils.ts`
- Database: Drizzle ORM or Prisma (default: Drizzle for edge compatibility)
- Auth: NextAuth.js v5 or Clerk

## 🏗️ 架构原则（Architecture Principles）

1. **Server Components by default.** Client Components ONLY when interactivity/state is required.
2. **`"use client"` directive at the TOP of client component files.** First line after imports if any, but actually first statement.
3. **Feature-based folder structure**, not type-based. Example:
   ```
   app/
     (dashboard)/
       dashboard/
         page.tsx
         layout.tsx
         components/
           DashboardHeader.tsx
           RecentActivity.tsx
     api/
       orders/
         route.ts
   components/
     ui/                # shadcn/ui primitives — NEVER hand-code alternatives
       button.tsx
       input.tsx
       dialog.tsx
       table.tsx
     layout/
       Sidebar.tsx
       Header.tsx
     features/          # Feature-specific, reusable non-UI components
       orders/
         OrderTable.tsx
       billing/
         PaymentForm.tsx
   lib/
     utils.ts           # cn() helper + general utilities
     types.ts           # Shared TypeScript types
     db/
       schema.ts        # Drizzle schema
   hooks/
     use-local-storage.ts
   ```

4. **No cross-feature imports of internal files.** A feature may import from another feature's PUBLIC API only.
5. **All API routes in `app/api/[resource]/route.ts`** — HTTP methods (GET, POST) exported as named functions.

## 🧩 shadcn/ui 组件约定（Component Usage Rules）

**Always use shadcn/ui primitives — never hand-code alternatives.**

```typescript
// ✅ GOOD
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import {
  Dialog, DialogContent, DialogHeader, DialogTitle,
  DialogDescription, DialogTrigger,
} from "@/components/ui/dialog";

// ❌ BAD — never create custom buttons/inputs/dialogs
<button onClick={...}>Submit</button>
<input value={...} onChange={...} />

// ❌ BAD — DO NOT omit DialogTitle (breaks screen readers)
// ❌ BAD — DO NOT omit DialogDescription
```

**Form pattern (React Hook Form + Zod):**

```typescript
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import { Form, FormControl, FormDescription, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form";
import { Input } from "@/components/ui/input";

const formSchema = z.object({
  email: z.string().email("Invalid email address"),
  password: z.string().min(8, "Password must be at least 8 characters"),
});

type FormValues = z.infer<typeof formSchema>;

function LoginForm() {
  const form = useForm<FormValues>({
    resolver: zodResolver(formSchema),
    defaultValues: { email: "", password: "" },
  });

  async function onSubmit(values: FormValues) {
    // business logic
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
        <FormField control={form.control} name="email" render={({ field }) => (
          <FormItem>
            <FormLabel>Email</FormLabel>
            <FormControl><Input placeholder="you@example.com" {...field} /></FormControl>
            <FormMessage />
          </FormItem>
        )} />
        {/* ... more FormFields */}
        <Button type="submit">Sign In</Button>
      </form>
    </Form>
  );
}
```

## 💅 样式约定（Styling Rules）

- **Tailwind utilities ONLY.** No inline `style={{}}` for layout/color/typography.
- **Mobile-first responsive design.** `md:`, `lg:` for desktop overrides.
- **Dark mode via `dark:` prefix** (e.g., `bg-white dark:bg-gray-900`).
- **Design tokens via CSS variables** — use `bg-background`, `text-foreground`, `border-border`, `text-muted-foreground`.
- **`cn()` from `@/lib/utils`** for conditional class names. Never template literals for conditional classes.
- **`space-x-*` / `space-y-*` are deprecated** — use `flex + gap-*` instead.

## ⚡ 代码风格（Code Style）

```typescript
// ✅ Arrow functions for components — PascalCase filename, single default export
export default function DashboardPage() {
  // ...
}

// ✅ Named exports for reusable components
export function OrderTable({ orders }: { orders: Order[] }) {
  // ...
}

// ✅ Explicit types on props, inferred elsewhere where obvious
interface UserCardProps {
  user: { id: string; name: string; email: string };
  onEdit: (id: string) => void;
}
export function UserCard({ user, onEdit }: UserCardProps) { ... }

// ✅ Early returns over nested ifs
if (error) return <ErrorDisplay error={error} />;
if (!data) return <Skeleton />;
return <Content data={data} />;

// ❌ No function keyword for components
// ❌ No unnamed default exports (hurts dev tools — components show as "Anonymous")
// ❌ No any unless unavoidable (and always add // eslint-disable-next-line with a comment)
```

## 🔍 数据获取（Data Fetching）

- **Server Components:** `await fetch()` with `{ cache: "force-cache" }` or `{ next: { revalidate: 3600 } }`
- **Client Components:** React Query (`useQuery` / `useMutation`). Never raw useEffect + fetch.
- **API Routes:** Use route handlers (`app/api/resource/route.ts`). Export `async GET`, `async POST`, etc.
- **Mutations:** invalidate query cache after mutations. Do NOT manually refetch.

## 🛡️ 包含共享安全规则

请同时参考 `shared-security.md` 中的安全规则。以下为前端特定的安全要点：

- Forms: DOMPurify for user-generated rich text content
- Links: `rel="noopener noreferrer"` on external links
- Never expose server environment variables — only `NEXT_PUBLIC_*` are available on client
- `use server` actions: validate inputs, check permissions, do NOT trust client data

---

## 文件放置建议

将此文件复制到你的项目中：

**Cursor 用户：**
```
.project/.cursor/rules/01-nextjs-shadcn.mdc
或
.cursor/rules/01-nextjs-shadcn.mdc
```

**GitHub Copilot 用户：**
```
.github/copilot-instructions.md
```

然后删除此"文件放置建议"章节。
