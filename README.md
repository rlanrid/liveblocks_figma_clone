# setup
`npm install fabric uuid @liveblocks/client @liveblocks/react`

```js
npx create-next-app@latest ./
Need to install the following packages:
create-next-app@14.2.2
Ok to proceed? (y) y
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to customize the default import alias (@/*)? ... No / Yes
```


## 트러블 슈팅

```js
node:internal/modules/esm/resolve:215
  const resolvedOption = FSLegacyMainResolve(packageJsonUrlString, packageConfig.main, baseStringified);
                         ^

Error: Cannot find package 'C:\Users\(사용자 계정)\AppData\Local\npm-cache\_npx\125ee17d583c4e03\node_modules\wcwidth\package.json' imported from C:\Users\(사용자 계정)\AppData\Local\npm-cache\_npx\125ee17d583c4e03\node_modules\ora\index.js
    at legacyMainResolve (node:internal/modules/esm/resolve:215:26)        
    at packageResolve (node:internal/modules/esm/resolve:841:14)
    at moduleResolve (node:internal/modules/esm/resolve:927:18)
    at defaultResolve (node:internal/modules/esm/resolve:1157:11)
    at ModuleLoader.defaultResolve (node:internal/modules/esm/loader:390:12    at ModuleLoader.resolve (node:internal/modules/esm/loader:359:25)        
    at ModuleLoader.getModuleJob (node:internal/modules/esm/loader:234:38)   
    at ModuleWrap.<anonymous> (node:internal/modules/esm/module_job:87:39)   
    at link (node:internal/modules/esm/module_job:86:36) {
  code: 'ERR_MODULE_NOT_FOUND'
}

Node.js v20.12.2
```   
해결: `npx clear-npx-cache`#   l i v e b l o c k s _ f i g m a _ c l o n e  
 