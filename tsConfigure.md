# tsConfigure 
## 安装包
eslint
js文件ts化
.vue class-component化
### 引入包文件
**@vue/cli-plugin-typescript ^4.5.13**
**@vue/eslint-config-typescript ^5.0.2**：eslint配置依赖集合
**@typescript-eslint/eslint-plugin ^2.33.0**：eslint配置依赖集合
**@typescript-eslint/parser ^2.33.0**：eslint配置依赖集合
**typescript ~3.9.3**
**vue-class-component ^7.2.6**：提供装饰器(官方)
**vue-property-decorator ^9.1.2**：提供装饰器(三方，依赖于 vue-class-component)
**vuex-class ^0.3.2**：让 vuex 更好的支持 typescript
```
yarn add @vue/cli-plugin-typescript @vue/eslint-config-typescript typescript @typescript-eslint/eslint-plugin @typescript-eslint/parser -D 

yarn add vue-class-component vue-property-decorator vuex-class
```

### eslint 新增配置
注意：目前使用的 ``@typescript-eslint/eslint-plugin``、``@typescript-eslint/parser``为 2.33.0, 其他版本在``@vue/typescript/recommended``会冲突。

```
'extends': [
+    '@vue/typescript/recommended'
],
  rules: {
+    '@typescript-eslint/no-unused-vars': 'off',
+    '@typescript-eslint/no-var-requires': 'off',
+    '@typescript-eslint/no-inferrable-types': 'off',
+    '@typescript-eslint/no-explicit-any': 'off',
+    'camelcase': 'off',
+    '@typescript-eslint/camelcase': 0
  }
  parserOptions: {
+    parser: '@typescript-eslint/parser'
-    parser: 'babel-eslint',
-    ecmaFeatures: {
-      legacyDecorators: true
-    }
  },
+ plugins: ['@typescript-eslint'],
```
