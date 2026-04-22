# Framework Implementation Patterns

Production-ready patterns for the most common frontend frameworks. Consult the relevant section when generating code — these patterns ensure token integration, accessibility, and platform conventions.

---

## Table of Contents
- [React + Tailwind v4](#react--tailwind-v4)
- [Next.js 15](#nextjs-15)
- [React Native (Expo)](#react-native-expo)
- [SwiftUI](#swiftui)
- [Flutter](#flutter)

---

## React + Tailwind v4

### Token → Tailwind Mapping

Tailwind v4 uses CSS-first configuration. Map design tokens to `@theme`:

```css
/* app/globals.css */
@import "tailwindcss";

@theme {
  --color-action-primary: oklch(55% 0.2 260);
  --color-text-primary: oklch(15% 0.02 260);
  --color-surface-page: oklch(98% 0.005 260);
  --color-border-default: oklch(90% 0.01 260);
  --radius-button: 6px;
  --radius-card: 8px;
  --shadow-focus-ring: 0 0 0 2px #ffffff, 0 0 0 4px oklch(55% 0.2 260);
}
```

### Component Pattern: `cva` + `cn()`

```typescript
// lib/utils.ts
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";
export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

// components/ui/button.tsx
import { cva, type VariantProps } from "class-variance-authority";
import { forwardRef, type ButtonHTMLAttributes } from "react";
import { cn } from "@/lib/utils";

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 font-medium rounded-[var(--radius-button)] transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none",
  {
    variants: {
      variant: {
        primary: "bg-action-primary text-white hover:bg-action-primary/90",
        secondary: "bg-surface-page border border-border-default text-text-primary hover:bg-surface-sunken",
        ghost: "text-text-primary hover:bg-surface-sunken",
        destructive: "bg-red-600 text-white hover:bg-red-700",
      },
      size: {
        sm: "h-8 px-3 text-sm",
        md: "h-10 px-4 text-base",
        lg: "h-12 px-6 text-lg",
      },
    },
    defaultVariants: { variant: "primary", size: "md" },
  }
);

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement>, VariantProps<typeof buttonVariants> {
  asChild?: boolean;
}

const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, ...props }, ref) => (
    <button className={cn(buttonVariants({ variant, size }), className)} ref={ref} {...props} />
  )
);
Button.displayName = "Button";
export { Button, buttonVariants };
```

### Key Rules
- Always use `forwardRef` — parent components need ref access
- Always accept `className` prop — allow composability via `cn()`
- Use `cva` for variant management — cleaner than conditional classes
- Use `focus-visible` not `focus` — prevents focus ring on mouse click
- Dark mode: Tailwind v4 uses `@variant dark` in CSS, no `dark:` prefix hacks needed

---

## Next.js 15

### Key Patterns
- **Server Components by default** — only add `"use client"` when you need hooks/interactivity
- **`next/font`** for font loading — no FOUT, no layout shift
- **`next/image`** for all images — automatic optimization, lazy loading, responsive
- **Server Actions** for form mutations — no API routes needed for simple forms

```typescript
// app/layout.tsx — Server Component, font loading
import { Inter } from "next/font/google";
const inter = Inter({ subsets: ["latin"], variable: "--font-sans" });

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={inter.variable}>
      <body>{children}</body>
    </html>
  );
}
```

```typescript
// app/settings/page.tsx — Server Component, data fetching
export default async function SettingsPage() {
  const settings = await getSettings(); // Direct DB/API call, no useEffect
  return <SettingsForm settings={settings} />;
}

// components/settings-form.tsx — Client Component (needs interactivity)
"use client";
export function SettingsForm({ settings }) {
  // hooks, state, event handlers here
}
```

### Route Organization
```
app/
├── (auth)/          # Route group — shared auth layout
│   ├── login/page.tsx
│   └── register/page.tsx
├── (dashboard)/     # Route group — shared dashboard layout
│   ├── layout.tsx   # Sidebar + header
│   ├── page.tsx     # Dashboard home
│   └── settings/page.tsx
├── loading.tsx      # Global loading state
└── error.tsx        # Global error boundary
```

---

## React Native (Expo)

### Token Integration

```typescript
// theme/tokens.ts
export const colors = {
  primary: '#FF6B35',
  primaryDark: '#E55A2B',
  surface: '#FFFFFF',
  surfaceSecondary: '#F5F5F5',
  text: '#1A1A1A',
  textSecondary: '#666666',
  border: '#E0E0E0',
  success: '#2ECC71',
  error: '#E74C3C',
} as const;

export const spacing = {
  xs: 4, sm: 8, md: 12, base: 16, lg: 20, xl: 24, xxl: 32, xxxl: 48,
} as const;

export const radii = {
  sm: 4, md: 8, lg: 16, full: 9999,
} as const;
```

### Component Pattern

```typescript
import { Pressable, Text, StyleSheet, Platform } from 'react-native';

interface ButtonProps {
  label: string;
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  onPress: () => void;
  disabled?: boolean;
  loading?: boolean;
}

export function Button({ label, variant = 'primary', size = 'md', onPress, disabled, loading }: ButtonProps) {
  return (
    <Pressable
      onPress={onPress}
      disabled={disabled || loading}
      accessibilityRole="button"
      accessibilityLabel={label}
      accessibilityState={{ disabled: disabled || loading, busy: loading }}
      style={({ pressed }) => [
        styles.base,
        styles[variant],
        styles[size],
        pressed && styles.pressed,
        disabled && styles.disabled,
      ]}
    >
      {loading ? <ActivityIndicator /> : <Text style={[styles.label, styles[`${variant}Label`]]}>{label}</Text>}
    </Pressable>
  );
}
```

### Platform-Specific Patterns
```typescript
// Shadows differ between platforms
const shadow = Platform.select({
  ios: { shadowColor: '#000', shadowOffset: { width: 0, height: 2 }, shadowOpacity: 0.1, shadowRadius: 4 },
  android: { elevation: 4 },
});

// Safe areas
import { useSafeAreaInsets } from 'react-native-safe-area-context';
const insets = useSafeAreaInsets();
// Apply: paddingTop: insets.top, paddingBottom: insets.bottom
```

### Key Rules
- Use `Pressable` not `TouchableOpacity` — more flexible, better accessibility
- Always provide `accessibilityRole` and `accessibilityLabel`
- Use `FlatList` for lists (not `.map()`) — virtualized for performance
- Avoid inline styles for repeated elements — use `StyleSheet.create()`
- Test on real devices — simulator shadows and animations differ

---

## SwiftUI

### Token Integration

```swift
extension Color {
  enum DS {
    static let actionPrimary = Color("ActionPrimary") // Asset Catalog
    static let textPrimary = Color(.label)             // System adaptive
    static let surfacePage = Color(.systemBackground)
    static let borderDefault = Color(.separator)
  }
}

enum Spacing {
  static let xs: CGFloat = 4
  static let sm: CGFloat = 8
  static let md: CGFloat = 12
  static let base: CGFloat = 16
  static let lg: CGFloat = 24
  static let xl: CGFloat = 32
}
```

### Component Pattern: ButtonStyle

```swift
struct PrimaryButtonStyle: ButtonStyle {
  @Environment(\.isEnabled) var isEnabled

  func makeBody(configuration: Configuration) -> some View {
    configuration.label
      .font(.DS.label)
      .foregroundStyle(.white)
      .padding(.horizontal, Spacing.lg)
      .padding(.vertical, Spacing.md)
      .background(configuration.isPressed ? Color.DS.actionPrimary.opacity(0.8) : Color.DS.actionPrimary)
      .clipShape(RoundedRectangle(cornerRadius: 8))
      .opacity(isEnabled ? 1 : 0.5)
      .scaleEffect(configuration.isPressed ? 0.98 : 1)
      .animation(.easeOut(duration: 0.15), value: configuration.isPressed)
  }
}
```

### Key Rules
- Use `@ScaledMetric` for Dynamic Type support — sizes scale with user preference
- Use `@Environment(\.accessibilityReduceMotion)` to respect motion preferences
- Use `#if os(iOS)` / `#if os(macOS)` for platform-specific code
- Use system colors (`Color(.label)`, `Color(.systemBackground)`) — they auto-adapt to dark mode
- Prefer semantic system styles over hardcoded values

---

## Flutter

### Token Integration

```dart
class AppTokens {
  static const primary = Color(0xFFFF6B35);
  static const surface = Color(0xFFFFFFFF);
  static const text = Color(0xFF1A1A1A);
  static const border = Color(0xFFE0E0E0);

  static const spacingSm = 8.0;
  static const spacingMd = 12.0;
  static const spacingBase = 16.0;
  static const spacingLg = 24.0;

  static const radiusSm = 4.0;
  static const radiusMd = 8.0;
}
```

### Theme Integration

```dart
ThemeData appTheme() => ThemeData(
  colorScheme: ColorScheme.fromSeed(seedColor: AppTokens.primary),
  useMaterial3: true,
  textTheme: const TextTheme(
    headlineLarge: TextStyle(fontSize: 32, fontWeight: FontWeight.bold),
    bodyLarge: TextStyle(fontSize: 16),
    labelLarge: TextStyle(fontSize: 14, fontWeight: FontWeight.w500),
  ),
);
```

### Key Rules
- Use `Theme.of(context)` to access tokens — enables theming
- Use `MediaQuery.of(context).textScaleFactor` for Dynamic Type support
- Use `Semantics` widget for custom interactive element accessibility
- Prefer `const` constructors for performance
- Use `ListView.builder` for long lists — not `Column` with `children.map()`
