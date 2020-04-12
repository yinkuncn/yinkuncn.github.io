### test title

inside

code 1

```js
// 增加 less 支持
const lessRegex = /\.less$/;
const lessModuleRegex = /\.module\.less$/;

            {
              test: lessRegex,
              exclude: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                },
                'less-loader'
              ),
              // Don't consider CSS imports dead code even if the
              // containing package claims to have no side effects.
              // Remove this when webpack adds a warning or an error for this.
              // See https://github.com/webpack/webpack/issues/6571
              sideEffects: true,
            },
            // Adds support for CSS Modules, but using LASS
            // using the extension .module.scss or .module.less
            {
              test: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                  modules: true,
                  getLocalIdent: getCSSModuleLocalIdent,
                },
                'less-loader'
              ),
            },
```

