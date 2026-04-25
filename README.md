# localhost-to-figma

Capture shim for the **[Localhost to Figma](https://github.com/pradyumna-ks/localhost-to-figma)** Figma plugin.

Install this in your dev project so the plugin can read your running app's DOM and paste it into Figma as editable layers — frames, text, fills, auto layout — in one click.

---

## Install

```bash
npm install localhost-to-figma --save-dev
```

## Add to your entry file

```ts
// src/main.tsx  (or index.ts / main.ts / _app.tsx)
import 'localhost-to-figma';
```

That's it. The shim listens for a capture request from the plugin and sends back the DOM tree. It does nothing otherwise.

---

## Dev-only guard (recommended)

```ts
if (import.meta.env.DEV) {
  await import('localhost-to-figma');
}
```

Webpack / CRA:

```ts
if (process.env.NODE_ENV === 'development') {
  await import('localhost-to-figma');
}
```

---

## Next.js

```tsx
// app/layout.tsx
import Script from 'next/script';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        {process.env.NODE_ENV === 'development' && (
          <Script src="/figma-capture.js" strategy="afterInteractive" />
        )}
      </body>
    </html>
  );
}
```

---

## How it works

When the Localhost to Figma plugin clicks **Paste**, it sends a `postMessage` to your app. This shim receives it, walks the visible DOM, and returns a structured layer tree. The plugin uses that tree to create Figma nodes.

The shim is tree-shakeable when used with a dev-only dynamic import and adds no overhead to production builds.

---

## Component tagging (optional)

Tag elements you want promoted to Figma components:

```tsx
<Button data-component="Button/Primary">Save</Button>
```

Repeated `data-component` values become a Figma component + instances automatically.

---

## License

MIT — [Pradyumna KS](https://github.com/pradyumna-ks)
