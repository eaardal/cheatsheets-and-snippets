### Lag Node+TS prosjekt

1. `mkdir <project> && cd ./project`
2. `npm init --y`
   1. Sett `"type": "module"` i package.json.
3. `npm install typescript ts-node @types/ts-node`
4. `npx tsc --init`
   1. Sett `target` til `ESNext` (tsconfig.json)
   2. Sett `module` til `ESNext` (tsconfig.json)
   3. Sett `moduleResolution` til `node16` eller `nodeNext` (tsconfig.json)
   4. Sett `esModuleInterop` til `true` (tsconfig.json)
5. I package.json, lag et script for å kjøre appen: `"main": "ts-node-esm <entryfile.ts>"`
6. Kjøre appen: `npm run main` eller det scriptet heter.

### Pakke en executable for et Node+TS prosjekt

1. `npm install pkg`
2. I tsconfig.json, sett `module` til `CommonJS` og `outDir` til `dist`. Se at `includes` og `excludes` er riktig (at ikke node_modules blir med).
3. I package.json, sett `"bin": "dist/main.js"` eller pek mot den bygde entrypoint-filen.
4. I package.json, se til at `main` peker på riktig bygd entrypoint-fil.
5. I package.json, lag et build script: `"build": "rm -rf ./dist && tsc`.
6. I package.json, lag et package script:

```
"sort-bin": "mkdir -p ./bin/linux && mkdir -p ./bin/macos && mkdir -p ./bin/win && mv ./bin/builderbuilder-linux ./bin/linux/builderbuilder && mv ./bin/builderbuilder-macos ./bin/macos/builderbuilder && mv ./bin/builderbuilder-win.exe ./bin/win/builderbuilder.exe",
"package": "rm -rf ./bin && pkg --target node16-linux,node16-macos,node16-win --out-path ./bin --no-bytecode . && npm run sort-bin",
"put-on-path": "cp ./bin/macos/builderbuilder ~/dev/apps/builderbuilder"
```

1. Kjør `npm run build` for å bygge TS til JS.
2. Kjør `npm run package` for å bygge executables med Node som kjører den bygde JS koden.
