

#### 1. **What is Webpack? What are Bundlers in General? Why Do We Need Them?**

Webpack is a powerful **module bundler** primarily used in modern JavaScript applications. It takes modules with dependencies (e.g., JavaScript, CSS, images) and bundles them into a single output file (or smaller chunks) that can be efficiently served to the browser.

**Bundlers** like Webpack are tools that package multiple files into one or more bundles, enabling modern frontend workflows. Without bundlers, managing dependencies, assets, and module loading in web projects can become cumbersome and inefficient.

**Why we need bundlers:**

- Minimize the number of HTTP requests by bundling multiple files.
- Enable the use of modern JavaScript (ES6+) features and other languages (e.g., TypeScript) by transpiling them into browser-compatible code.
- Process and optimize assets (CSS, images, fonts) for better performance.
- Facilitate modular coding, improving maintainability and scalability.

**Diagram:**

```
Input Files (JS, CSS, Images, etc.)
       ↓
   Webpack (Loaders & Plugins)
       ↓
Output Bundles (Optimized for Browser)
```
