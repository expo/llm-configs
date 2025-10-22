# Expo Router Quick Onboarding Guide

## What is Expo Router?

File-based router for React Native and web applications built on React Navigation. Automatic deep linking, universal navigation across platforms.

## Core Concepts

### File Structure Rules

- All routes live in `app/` directory
- Each file = a screen/page with automatic URL
- `app/index.tsx` = initial route (`/`)
- `app/_layout.tsx` = root layout (replaces App.tsx)
- Non-navigation code goes outside `app/`

### File Naming Convention

- `home.tsx` → `/home` (static route)
- `[id].tsx` → `/123` (dynamic route)
- `(tabs)/` → route group (doesn't affect URL)
- `index.tsx` → default route for directory
- `_layout.tsx` → navigation layout
- `+not-found.tsx` → catch-all error page

## Navigation

### Imperative Navigation

```tsx
import { useRouter } from "expo-router";

const router = useRouter();
router.navigate("/about"); // Navigate to route
router.push("/about"); // Push onto stack
router.back(); // Go back
router.replace("/about"); // Replace current
```

### Declarative Navigation

```tsx
import { Link } from 'expo-router';

<Link href="/about">About</Link>
<Link href="/user/123">User Profile</Link>

// With params
<Link href={{
  pathname: '/user/[id]',
  params: { id: '123' }
}}>Profile</Link>

// Pressable links
<Link href="/about" asChild>
  <Pressable><Text>About</Text></Pressable>
</Link>
```

### Dynamic Routes & Params

```tsx
// File: app/user/[id].tsx
import { useLocalSearchParams } from "expo-router";

export default function User() {
  const { id } = useLocalSearchParams();
  return <Text>User ID: {id}</Text>;
}
```

## Layouts

### Root Layout (`app/_layout.tsx`)

```tsx
import { Stack } from "expo-router";
import { useFonts } from "expo-font";

export default function RootLayout() {
  const [loaded] = useFonts({
    /* fonts */
  });

  if (!loaded) return null;

  return <Stack />;
}
```

### Stack Layout

```tsx
import { Stack } from "expo-router";

export default function Layout() {
  return (
    <Stack>
      <Stack.Screen name="index" options={{ title: "Home" }} />
      <Stack.Screen name="about" options={{ headerShown: false }} />
    </Stack>
  );
}
```

### Tab Layout

```tsx
import { Tabs } from "expo-router";
import { FontAwesome } from "@expo/vector-icons";

export default function TabLayout() {
  return (
    <Tabs screenOptions={{ tabBarActiveTintColor: "blue" }}>
      <Tabs.Screen
        name="index"
        options={{
          title: "Home",
          tabBarIcon: ({ color }) => (
            <FontAwesome size={28} name="home" color={color} />
          ),
        }}
      />
      <Tabs.Screen name="settings" options={{ title: "Settings" }} />
    </Tabs>
  );
}
```

### Slot Layout (No Navigator)

```tsx
import { Slot } from "expo-router";

export default function Layout() {
  return (
    <>
      <Header />
      <Slot />
      <Footer />
    </>
  );
}
```

## Common Patterns

### Tabs with Stack

```
app/
  _layout.tsx           # Root stack
  (tabs)/
    _layout.tsx         # Tab navigator
    index.tsx           # Home tab
    profile.tsx         # Profile tab
  modal.tsx             # Modal over tabs
```

### Authentication Flow

```tsx
// app/_layout.tsx
export default function RootLayout() {
  return (
    <Stack>
      <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
      <Stack.Screen name="login" options={{ presentation: "modal" }} />
    </Stack>
  );
}
```

### Protected Routes

```tsx app/_layout.tsx
import { Stack } from "expo-router";

const isLoggedIn = false;

export function AppLayout() {
  return (
    <Stack>
      <Stack.Protected guard={!isLoggedIn}>
        <Stack.Screen name="login" />
      </Stack.Protected>

      <Stack.Protected guard={isLoggedIn}>
        <Stack.Screen name="private" />
      </Stack.Protected>
      {/* Expo Router includes all routes by default. Adding Stack.Protected creates exceptions for these screens. */}
    </Stack>
  );
}
```

## Tabs and Drawer

Protected routes are also available for Tabs.

```tsx app/_layout.tsx
import { Tabs } from "expo-router";

const isLoggedIn = false;

export default function TabLayout() {
  return (
    <Tabs>
      <Tabs.Screen name="index" options={{ tabBarLabel: "Home" }} />
      <Tabs.Protected guard={isLoggedIn}>
        <Tabs.Screen name="private" options={{ tabBarLabel: "Private" }} />
        <Tabs.Screen name="profile" options={{ tabBarLabel: "Profile" }} />
      </Tabs.Protected>

      <Tabs.Protected guard={!isLoggedIn}>
        <Tabs.Screen name="login" options={{ tabBarLabel: "Login" }} />
      </Tabs.Protected>
    </Tabs>
  );
}
```

## Tips

- Use `router.navigate()` for most navigation
- Relative paths: `./page` (current dir), `../page` (parent dir)
- Query params: `href="/users?limit=20"` or `router.setParams({ limit: 20 })`
- Deep links work automatically: `myapp://profile/123`
- Set `initialRouteName` in layouts for proper back navigation
- Use `<Link prefetch />` for faster navigation
- Hide tabs: `<Tabs.Screen options={{ href: null }} />`

## Example App Structure

```
app/
  _layout.tsx           # Root layout
  index.tsx             # Home page (/)
  about.tsx             # About page (/about)
  (tabs)/
    _layout.tsx         # Tab layout
    index.tsx           # Home tab (/)
    profile.tsx         # Profile tab (/profile)
  user/
    [id].tsx            # Dynamic user page (/user/123)
  +not-found.tsx        # 404 page
```

Ready to build! 🚀
