
### Basics of Webpack: Loaders, Plugins, Dev/Prod Modes

#### 1. **Loaders**
Loaders in Webpack transform files during the bundling process. They enable Webpack to process non-JavaScript files like CSS, images, and TypeScript. Each loader specifies a transformation rule.

**Example of Loaders in `webpack.config.js`:**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/, // Matches .css files
        use: ['style-loader', 'css-loader'], // Use these loaders for transformation
      },
      {
        test: /\.(png|jpg|gif)$/, // Matches image files
        use: ['file-loader'], // Use file-loader to handle these assets
      },
    ],
  },
};
```

#### 2. **Plugins**
Plugins extend Webpack's functionality by enabling tasks like optimization, resource management, and environment definition.

**Example of Plugins in `webpack.config.js`:**
```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html', // Injects scripts into the HTML template
    }),
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css', // Extracts CSS into separate files
    }),
  ],
};
```

#### 3. **Development Mode**
Development mode optimizes for debugging and faster rebuilds:
- Includes inline source maps.
- Enables Hot Module Replacement (HMR).
- Outputs non-minified code.

**Setting Development Mode:**
```js
module.exports = {
  mode: 'development',
};
```

#### 4. **Production Mode**
Production mode optimizes for performance:
- Minifies JavaScript using tools like Terser.
- Extracts CSS into separate files.
- Optimizes assets for caching.

**Setting Production Mode:**
```js
module.exports = {
  mode: 'production',
};
```

#### 5. **Combining Loaders, Plugins, and Modes**
Webpack's power lies in combining loaders and plugins, tailored for development or production environments.

**Example Config:**
```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: process.env.NODE_ENV || 'development',
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          process.env.NODE_ENV === 'production' ? MiniCssExtractPlugin.loader : 'style-loader',
          'css-loader',
        ],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
    new MiniCssExtractPlugin(),
  ],
};
```
