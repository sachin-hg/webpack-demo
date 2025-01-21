
### Common Loaders Explained

Loaders are a core feature of Webpack that transform files during the bundling process. They allow Webpack to handle different types of files (e.g., CSS, images, TypeScript) by converting them into JavaScript modules. Below are some commonly used loaders:

#### 1. **Babel Loader**
Transforms modern JavaScript (ES6+) or TypeScript into browser-compatible JavaScript.

**Example:**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
          },
        },
      },
    ],
  },
};
```

#### 2. **Terser Loader**
Used for minifying JavaScript files to optimize performance in production.

**Note:** Terser is typically used as a plugin (`TerserWebpackPlugin`) instead of a loader.

#### 3. **CSS Loader**
Processes `@import` and `url()` in CSS files, resolving them into dependencies.

**Example:**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

#### 4. **Style Loader**
Injects CSS into the DOM by adding `<style>` tags.

**Example:**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

#### 5. **File Loader**
Handles importing files (e.g., images, fonts) and emits them to the output directory.

**Example:**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: ['file-loader'],
      },
    ],
  },
};
```

#### 6. **URL Loader**
Similar to `file-loader`, but inlines files as Base64 strings if they are smaller than a specified limit.

**Example:**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: {
          loader: 'url-loader',
          options: {
            limit: 8192, // Files smaller than 8KB will be inlined
          },
        },
      },
    ],
  },
};
```

#### 7. **Asset Modules**
A newer Webpack feature (introduced in Webpack 5) that replaces `file-loader` and `url-loader`.

**Example:**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        type: 'asset/resource', // Emits files similar to file-loader
      },
      {
        test: /\.svg$/,
        type: 'asset/inline', // Inlines files similar to url-loader
      },
    ],
  },
};
```

---

### Key Takeaways:
- Loaders allow Webpack to handle non-JavaScript files by converting them into JavaScript modules.
- Choose loaders based on your project's requirements, such as handling styles, images, or modern JavaScript.
- Use `asset modules` for simplified handling of static assets in Webpack 5.
