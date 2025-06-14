# Complete Guide: Next.js + Tailwind CSS v4 + shadcn/ui Setup

This guide will walk you through setting up a modern Next.js project with JavaScript, Tailwind CSS v4, and shadcn/ui components.

## Prerequisites

-   Node.js 18.18 or later
-   npm, yarn, or pnpm package manager
-   Basic knowledge of React and Next.js

## Step 1: Create a New Next.js Project

```bash
npx create-next-app@latest my-project
cd my-project
```

or use . to create in current directory

```bash
npx create-next-app@latest .
```

When prompted, choose the following options:

-   Would you like to use TypeScript? → **No**
-   Would you like to use ESLint? → **Yes**
-   Would you like to use Tailwind CSS? → **Yes**
-   Would you like to use `src/` directory? → **Yes** (recommended)
-   Would you like to use App Router? → **Yes**
-   Would you like to use Turbopack? → **No**
-   Would you like to customize the default import alias? → **No**

## Step 2: Install shadcn/ui

Initialize shadcn/ui in your project:

```bash
npx shadcn@latest init
```

**Choose your base color theme:**

-   **Neutral** - Clean, minimal design
-   **Gray** - Classic, timeless design
-   **Zinc** - Modern, industrial look
-   **Stone** - Warm, earthy palette
-   **Slate** - Cool, sophisticated theme

### Customizing Themes

**Changing themes later:** Delete the `components.json` file and re-run `npx shadcn@latest init` to select a different base theme.

**Custom themes:** Edit `src/app/globals.css` and modify the CSS variables in the `:root` and `.dark` selectors to create your own color palette.

### Helpful Resources

-   [shadcn/ui Theme Gallery](https://ui.shadcn.com/themes) - Visual preview of all color palettes
-   [shadcn/ui Theming Guide](https://ui.shadcn.com/docs/theming) - Complete theming documentation
-   [Tailwind CSS Colors](https://tailwindcss.com/docs/customizing-colors) - Advanced color customization

## Step 4: Test Your Setup

Add your first shadcn/ui component:

```bash
npx shadcn@latest add button
```

Update `src/app/page.js` to test the setup:

```javascript
import { Button } from "@/components/ui/button";

export default function Home() {
    return (
        <main className="flex min-h-screen flex-col items-center justify-center p-24">
            <div className="z-10 max-w-5xl w-full items-center justify-between font-mono text-sm">
                <h1 className="text-4xl font-bold mb-8">
                    Next.js + Tailwind CSS v4 + shadcn/ui
                </h1>
                <div className="space-x-4">
                    <Button>Default Button</Button>
                    <Button variant="secondary">Secondary Button</Button>
                    <Button variant="outline">Outline Button</Button>
                </div>
            </div>
        </main>
    );
}
```

## Step 5: Run Your Project

Start the development server:

```bash
npm run dev
```

Visit `http://localhost:3000` to see your project with working Tailwind CSS v4 and shadcn/ui components.

## Adding More Components

You can add more shadcn/ui components as needed:

```bash
npx shadcn@latest add card
npx shadcn@latest add input
npx shadcn@latest add label
npx shadcn@latest add textarea
npx shadcn@latest add select
npx shadcn@latest add dialog
```

Visit `https://ui.shadcn.com/docs/components` to see the full list of available components.

## Project Structure

Your project structure should look like this:

```
my-project/
├── src/
│   ├── app/
│   │   ├── globals.css
│   │   ├── layout.js
│   │   └── page.js
│   ├── components/
│   │   ├── theme-provider.jsx
│   │   ├── theme-toggle.jsx
│   │   └── ui/
│   │       └── button.jsx
│   └── lib/
│       └── utils.js
├── next.config.js
└── package.json
```

## Important Notes

1. **Tailwind CSS**: The latest version is automatically installed when you choose "Yes" for Tailwind CSS during create-next-app setup. The `@import "tailwindcss";` syntax automatically scans your project files.

2. **Component Files**: shadcn/ui components are created as `.jsx` files by default, but they work perfectly in JavaScript projects.

3. **CSS Variables**: The setup uses CSS variables for theming, making it easy to implement dark mode and custom themes.

4. **Import Aliases**: The `@/` alias is configured to point to the `src` directory for cleaner imports.

## Troubleshooting

### Common Issues:

1. **CSS not loading**: Ensure `@import "tailwindcss";` is at the top of your `globals.css` file
2. **Components not found**: Check your import paths and aliases
3. **Styling issues**: Tailwind CSS automatically scans your files - ensure your components are using valid class names
4. **Build errors**: Make sure all shadcn/ui dependencies are installed properly

### Getting Help:

-   [Next.js Documentation](https://nextjs.org/docs)
-   [Tailwind CSS v4 Documentation](https://tailwindcss.com/docs)
-   [shadcn/ui Documentation](https://ui.shadcn.com)

## Step 6: Set Up Dark Mode with Custom Theme Toggle

### Install next-themes

```bash
npm install next-themes
```

### Create Theme Provider

Create `src/components/theme-provider.jsx`:

```javascript
"use client";

import * as React from "react";
import { ThemeProvider as NextThemesProvider } from "next-themes";

export function ThemeProvider({ children, ...props }) {
    return <NextThemesProvider {...props}>{children}</NextThemesProvider>;
}
```

### Update Root Layout

Modify `src/app/layout.js` to include the theme provider:

```javascript
import { Inter } from "next/font/google";
import "./globals.css";
import { ThemeProvider } from "@/components/theme-provider";

const inter = Inter({ subsets: ["latin"] });

export const metadata = {
    title: "Create Next App",
    description: "Generated by create next app",
};

export default function RootLayout({ children }) {
    return (
        <html lang="en" suppressHydrationWarning>
            <body className={inter.className}>
                <ThemeProvider
                    attribute="class"
                    defaultTheme="system"
                    enableSystem
                    disableTransitionOnChange
                >
                    {children}
                </ThemeProvider>
            </body>
        </html>
    );
}
```

### Create Custom Theme Toggle Button

Create `src/components/theme-toggle.jsx`:

```javascript
"use client";

import { Button } from "@/components/ui/button";
import { Moon, Sun } from "lucide-react";
import { useTheme } from "next-themes";
import * as React from "react";

export function ThemeToggle() {
    const { theme, setTheme, resolvedTheme } = useTheme();
    const [mounted, setMounted] = React.useState(false);

    // Only render after hydration to prevent SSR mismatch
    React.useEffect(() => {
        setMounted(true);
    }, []);

    if (!mounted) {
        // Return a placeholder button during SSR/hydration
        return (
            <Button variant="outline" size="icon" disabled>
                <Sun className="size-[1.2rem]" />
                <span className="sr-only">Loading theme toggle</span>
            </Button>
        );
    }

    const toggleTheme = () => {
        if (theme === "system") {
            // If system theme, switch to the opposite of current resolved theme
            setTheme(resolvedTheme === "dark" ? "light" : "dark");
        } else {
            // If explicit theme set, toggle between light and dark
            setTheme(theme === "dark" ? "light" : "dark");
        }
    };

    const isDark = resolvedTheme === "dark";

    return (
        <Button
            variant="outline"
            size="icon"
            onClick={toggleTheme}
            className="relative overflow-hidden rounded-full"
        >
            <Sun
                className={`size-[1.2rem] transition-all duration-300 ${
                    isDark
                        ? "rotate-90 scale-0 opacity-0"
                        : "rotate-0 scale-100 opacity-100"
                }`}
            />
            <Moon
                className={`absolute size-[1.2rem] transition-all duration-300 ${
                    isDark
                        ? "rotate-0 scale-100 opacity-100"
                        : "-rotate-90 scale-0 opacity-0"
                }`}
            />
            <span className="sr-only">
                {isDark ? "Switch to light mode" : "Switch to dark mode"}
            </span>
        </Button>
    );
}
```

### Add Theme Toggle to Layout

Update `src/app/layout.js` to include the theme toggle component in the top-right corner of every page:

```javascript
import { ThemeProvider } from "@/components/theme-provider";
import { ThemeToggle } from "@/components/theme-toggle";
import { Inter } from "next/font/google";
import "./globals.css";

const inter = Inter({ subsets: ["latin"] });

export const metadata = {
    title: "Create Next App",
    description: "Generated by create next app",
};

export default function RootLayout({ children }) {
    return (
        <html lang="en" suppressHydrationWarning>
            <body className={inter.className}>
                <ThemeProvider
                    attribute="class"
                    defaultTheme="system"
                    enableSystem
                    disableTransitionOnChange
                >
                    <div className="absolute top-4 right-4">
                        <ThemeToggle />
                    </div>
                    {children}
                </ThemeProvider>
            </body>
        </html>
    );
}
```

### Update Your Main Page

Now update `src/app/page.js` to showcase the theme toggle and demonstrate your shadcn/ui components:

```javascript
import { Button } from "@/components/ui/button";

export default function Home() {
    return (
        <main className="flex min-h-screen flex-col items-center justify-center p-24">
            <div className="z-10 max-w-5xl w-full items-center justify-between font-mono text-sm">
                <h1 className="text-4xl font-bold mb-8 text-center">
                    Next.js + Tailwind CSS v4 + shadcn/ui
                </h1>

                <div className="text-center mb-8">
                    <p className="text-muted-foreground">
                        Try the theme toggle in the top-right corner!
                    </p>
                </div>

                <div className="flex justify-center space-x-4">
                    <Button>Default Button</Button>
                    <Button variant="secondary">Secondary Button</Button>
                    <Button variant="outline">Outline Button</Button>
                </div>
            </div>
        </main>
    );
}
```

## Theme Toggle Features

The custom theme toggle provides:

-   **Smooth Animations**: Icons rotate and scale with CSS transitions
-   **System Theme Awareness**: Handles system theme intelligently
-   **Hydration Safe**: Prevents SSR/client mismatches
-   **Accessible**: Screen reader support and keyboard navigation
-   **Beautiful Transitions**: Sun/moon icons with rotation and scale effects

### Key Features:

1. **Smooth Animations**: Icons rotate and scale with CSS transitions
2. **Hydration Safe**: Prevents SSR/client mismatches
3. **System Theme Aware**: Properly handles system preference detection
4. **Accessible**: Includes screen reader labels and keyboard navigation
5. **State Management**: Uses next-themes for persistent theme storage
6. **Visual Feedback**: Clear visual indicators for current theme state

## Optional: Alternative Theme Toggle with Dropdown Menu

If you want to provide users with explicit theme options in a dropdown menu instead of a simple toggle button, you can replace your existing theme toggle with this enhanced dropdown version that offers Light, Dark, and System options.

First, add the dropdown menu component:

```bash
npx shadcn@latest add dropdown-menu
```

Then replace the content of your `src/components/theme-toggle.jsx` file with this dropdown version:

```javascript
"use client";

import { Monitor, Moon, Sun } from "lucide-react";
import { useTheme } from "next-themes";
import * as React from "react";

import { Button } from "@/components/ui/button";
import {
    DropdownMenu,
    DropdownMenuContent,
    DropdownMenuItem,
    DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";

export function ThemeToggle() {
    const { theme, setTheme, resolvedTheme } = useTheme();
    const [mounted, setMounted] = React.useState(false);

    // Only render after hydration to prevent SSR mismatch
    React.useEffect(() => {
        setMounted(true);
    }, []);

    if (!mounted) {
        // Return a placeholder button during SSR/hydration
        return (
            <Button
                variant="outline"
                size="icon"
                disabled
                className="rounded-full"
            >
                <Sun className="size-[1.2rem]" />
                <span className="sr-only">Loading theme toggle</span>
            </Button>
        );
    }

    // Simple configuration: Set to false to disable monitor icon
    const showMonitorIcon = true;

    // Determine which icon to show based on theme and configuration
    const showMonitor = theme === "system" && showMonitorIcon;
    const showSun =
        theme === "light" ||
        (theme === "system" && !showMonitorIcon && resolvedTheme === "light");
    const showMoon =
        theme === "dark" ||
        (theme === "system" && !showMonitorIcon && resolvedTheme === "dark");

    return (
        <DropdownMenu>
            <DropdownMenuTrigger asChild>
                <Button
                    variant="outline"
                    size="icon"
                    className="rounded-full relative overflow-hidden"
                >
                    <Sun
                        className={`size-[1.2rem] transition-all duration-300 ${
                            showSun
                                ? "scale-100 rotate-0 opacity-100"
                                : "scale-0 rotate-90 opacity-0"
                        }`}
                    />
                    <Moon
                        className={`absolute size-[1.2rem] transition-all duration-300 ${
                            showMoon
                                ? "scale-100 rotate-0 opacity-100"
                                : "scale-0 -rotate-90 opacity-0"
                        }`}
                    />
                    {/* To disable monitor icon: set showMonitorIcon to false above */}
                    {showMonitorIcon && (
                        <Monitor
                            className={`absolute size-[1.2rem] transition-all duration-300 ${
                                showMonitor
                                    ? "scale-100 rotate-0 opacity-100"
                                    : "scale-0 rotate-90 opacity-0"
                            }`}
                        />
                    )}
                    <span className="sr-only">Toggle theme</span>
                </Button>
            </DropdownMenuTrigger>
            <DropdownMenuContent align="end">
                <DropdownMenuItem onClick={() => setTheme("light")}>
                    <Sun className="size-4 mr-2" />
                    Light
                </DropdownMenuItem>
                <DropdownMenuItem onClick={() => setTheme("dark")}>
                    <Moon className="size-4 mr-2" />
                    Dark
                </DropdownMenuItem>
                <DropdownMenuItem onClick={() => setTheme("system")}>
                    <Monitor className="size-4 mr-2" />
                    System
                </DropdownMenuItem>
            </DropdownMenuContent>
        </DropdownMenu>
    );
}
```

This enhanced dropdown version provides several improvements over the simple toggle:

-   **Explicit Options**: Users can directly select Light, Dark, or System themes from a dropdown menu
-   **Smart Icon Display**: The button shows the appropriate icon for the currently selected theme:
    -   ☀️ **Sun** when Light theme is selected
    -   🌙 **Moon** when Dark theme is selected
    -   🖥️ **Monitor** when System theme is selected (if enabled)
-   **Configurable Monitor Icon**: You can disable the monitor icon by setting `showMonitorIcon` to `false` - when disabled, the system theme will show sun/moon based on your actual system preference
-   **Visual Menu**: The dropdown includes icons next to each option for better clarity
-   **Smooth Animations**: All icons transition smoothly with scale, rotation, and opacity effects

**Key Differences from Simple Toggle:**

-   **Dropdown vs Toggle**: Click opens a menu instead of cycling through themes
-   **Visual Feedback**: Button icon reflects the selected theme setting (not just the current appearance)
-   **Three Options**: Easy access to all three theme options in one click

## Next Steps

1. Add more shadcn/ui components as needed
2. Configure ESLint and Prettier for code formatting
3. Set up a state management solution (Zustand, Context API)
4. Add form handling with `react-hook-form` and `zod`
5. Customize the theme colors in your CSS variables

Your Next.js project with Tailwind CSS v4, shadcn/ui, and custom dark mode is now ready for development!
