# Agent Guide — React Native

> React for native mobile. Same mental model, different primitives — think in native components, lists and platform differences.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/react-native/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with React Native. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Use native primitives, not web tags
- ✅ **APPLY:** Compose UIs from View, Text, Image, Pressable; all text must live inside <Text>.
- ⛔ **AVOID:** Reaching for div/span/CSS files — there's no DOM; styles are JS objects via StyleSheet.

### 2. Virtualize long lists
- ✅ **APPLY:** Use FlatList/SectionList with keyExtractor and getItemLayout for large data; they render only what's visible.
- ⛔ **AVOID:** Mapping thousands of rows inside a ScrollView — it mounts everything at once and janks or crashes.

### 3. Design for both platforms
- ✅ **APPLY:** Branch with Platform.select / Platform.OS and test on iOS and Android; respect safe areas.
- ⛔ **AVOID:** Assuming iOS behavior on Android (or vice versa) for gestures, shadows, and status bars.

### 4. Keep the JS thread free
- ✅ **APPLY:** Memoize rows, use native driver for animations (Reanimated), and offload heavy work.
- ⛔ **AVOID:** Blocking the JS thread with sync work during scroll/animation — it drops frames.

## Cheat Reference — concepts to remember

- **Use native primitives, not web tags** — Compose UIs from View, Text, Image, Pressable; all text must live inside <Text>.
- **Virtualize long lists** — Use FlatList/SectionList with keyExtractor and getItemLayout for large data; they render only what's visible.
- **Design for both platforms** — Branch with Platform.select / Platform.OS and test on iOS and Android; respect safe areas.
- **Keep the JS thread free** — Memoize rows, use native driver for animations (Reanimated), and offload heavy work.

## Full Cheat Sheet — every concept

### Core Components
- View (≈ div), Text (all text must be inside it), Image, ScrollView, TextInput.
- Pressable / TouchableOpacity for touch via onPress.

### Styling & Layout
- Styles are JS objects (StyleSheet.create), camelCase props (backgroundColor).
- Flexbox is the layout system (flexDirection defaults to column); units are dp.
- No CSS files / no DOM.

### Lists & Performance
- FlatList / SectionList virtualize long lists (keyExtractor, getItemLayout).
- Don't map huge arrays inside ScrollView — it mounts everything.
- Memoize rows; animate with the native driver (Reanimated); keep the JS thread free.

### Platform & Practices
- Platform.OS / Platform.select or .ios.tsx / .android.tsx for platform code; respect safe areas.
- Navigation via React Navigation (native stack).
- Use TypeScript, error boundaries, and accessibility labels.

## Interview Questions

#### Q1. How does React Native differ from React for web?
Same component model and hooks, but the primitives are native (View/Text/Image instead of div/span), styling is JS objects via StyleSheet (a Flexbox subset), and it bridges to native platform APIs rather than the DOM.

#### Q2. Why use FlatList over ScrollView?
FlatList virtualizes — it only renders items near the viewport and recycles them — so it scales to large datasets. ScrollView renders all children up front, which is fine only for small, bounded content.

#### Q3. How do you write platform-specific code?
Use the Platform module (Platform.OS / Platform.select) for small branches, or file extensions (.ios.tsx / .android.tsx) for whole-component variants.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
