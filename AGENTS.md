# AGENTS.md - Coding Agent Guidelines for Ditherly

## Project Overview

Ditherly is a Next.js application for exploring dithering techniques, ASCII-based rendering, and WebGL-powered visual tools.

## Project Structure

```
Ditherly/
├── ditherly/                 # Main Next.js application
│   ├── app/                  # App Router pages and layouts
│   ├── lib/                  # Utility functions
│   ├── components/           # React components (ui/ for shadcn)
│   └── public/               # Static assets
├── Assets/                   # Project assets
└── README.md
```

## Build/Lint/Test Commands

Run from the `ditherly/` directory:

```bash
# Development
bun dev                    # Start dev server (localhost:3000)

# Build
bun run build              # Production build
bun start                  # Start production server

# Linting
bun lint                   # Run ESLint
bun lint -- --fix          # Auto-fix lint issues
bun lint path/to/file.tsx  # Lint specific file

# Type Checking
bunx tsc --noEmit          # TypeScript type check
```

No test framework configured yet.

## Technology Stack

- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript 5 (strict mode)
- **Styling**: Tailwind CSS v4 + shadcn/ui (new-york style)
- **UI Components**: shadcn/ui with lucide-react icons
- **Fonts**: Geist Sans and Geist Mono
- **Package Manager**: bun (preferred)

## Code Style Guidelines

### Imports

```tsx
// 1. React/Next imports
import { useState } from "react";
import Image from "next/image";
import type { Metadata } from "next";

// 2. Third-party libraries
import { clsx, type ClassValue } from "clsx";

// 3. Internal imports with @ alias
import { cn } from "@/lib/utils";
import { Button } from "@/components/ui/button";
```

- Use `import type` for type-only imports
- Path alias `@/*` maps to `ditherly/`

### Component Structure

```tsx
// Default export for pages
export default function PageName() {
  return <div>...</div>;
}

// Named export for reusable components
export function ComponentName({ prop }: ComponentNameProps) {
  return <div>...</div>;
}

// Props - inline for simple, interface for complex
function Simple({ children }: Readonly<{ children: React.ReactNode }>) {
  return <div>{children}</div>;
}

interface ComplexProps {
  title: string;
  onClick?: () => void;
}
```

### Styling

```tsx
import { cn } from "@/lib/utils";

// Use cn() for conditional classes
<div className={cn("base-class", isActive && "active", className)} />

// Use theme variables
<div className="bg-background text-foreground border-border" />
```

### TypeScript

- Strict mode enabled - ensure correct types
- Prefer interfaces for object shapes
- Use `Readonly<T>` for immutable props
- Use `type` for unions and utility types

### Error Handling

```tsx
// Server Components
async function getData() {
  try {
    const res = await fetch(url);
    if (!res.ok) throw new Error("Failed to fetch");
    return res.json();
  } catch (error) {
    console.error("Data fetch error:", error);
    return null;
  }
}
```

### Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `ImageProcessor` |
| Functions | camelCase | `applyDithering` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RESOLUTION` |
| Component files | PascalCase | `ImageProcessor.tsx` |
| Utility files | camelCase | `dithering.ts` |

### React Best Practices

- Prefer Server Components (no "use client")
- Use "use client" only for: hooks, event handlers, browser APIs

```tsx
// Server Component (default)
export default async function Page() {
  const data = await fetchData();
  return <Display data={data} />;
}

// Client Component
"use client";
export function Interactive() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

## shadcn/ui

```bash
bunx shadcn add button    # Add component
bunx shadcn add card
```

Config in `components.json`: components at `@/components`, UI at `@/components/ui`

## Before Committing

1. `bun lint`
2. `bunx tsc --noEmit`
3. `bun run build`

## Notes

- Main code in `ditherly/` subdirectory
- Focus: dithering, ASCII rendering, WebGL, image processing
