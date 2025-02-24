### Jest debugging

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Jest All",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": ["--runInBand"],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Jest Current File",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": [
        "${fileBasenameNoExtension}",
        "--config",
        "jest.config.js",
        "--runInBand"
      ],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Jest Watch All",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": [
        "${fileBasenameNoExtension}",
        "--config",
        "jest.config.js",
        "--runInBand",
        "--watch"
      ],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    }
  ]
}
```

### Arc Browser debugging

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Arc Browser",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000", // Update to the URL you want to debug
      "webRoot": "${workspaceFolder}",
      "runtimeExecutable": "/Applications/Arc.app/Contents/MacOS/Arc",
      "userDataDir": "${workspaceFolder}/.vscode/arc-profile",
      "sourceMaps": true,
      "skipFiles": ["<node_internals>/**"]
    },
    {
      "name": "Attach to Arc Browser",
      "type": "chrome",
      "request": "attach",
      "port": 9222,
      "webRoot": "${workspaceFolder}",
      "sourceMaps": true,
      "skipFiles": ["<node_internals>/**"]
    }
  ]
}
```

### Arc Browser debugging setup steps

Oppsett for å debugge via Arc Browser:

VS Code launch.json:

- Legg til `Launch Arc Browser` og `Attach to Arc Browser` targets i launch.json

```json
{
  "type": "chrome",
  "request": "launch",
  "name": "Launch Arc Browser",
  "url": "http://localhost:3000", // Update to the URL you want to debug
  "webRoot": "${workspaceFolder}",
  "runtimeExecutable": "/Applications/Arc.app/Contents/MacOS/Arc",
  "userDataDir": "${workspaceFolder}/.vscode/arc-profile",
  "sourceMaps": true,
  "skipFiles": ["<node_internals>/**"]
},
{
  "type": "chrome",
  "request": "attach",
  "name": "Attach to Arc Browser",
  "port": 9222,
  "webRoot": "${workspaceFolder}",
  "sourceMaps": true,
  "skipFiles": ["<node_internals>/**"]
},
```

MacOS Automator:

For å kunne attache en debugger må Arc startes med remote debugging port skrudd på. Vi kan bruke Automator for å lage en ny executable med dette flagget bakt inn.

Merk at dette kun er nødvendig hvis du vil bruke `Attach to Arc Browser`. Hvis du bare vil bruke `Launch Arc Browser` er ikke dette nødvendig.

- Lag nytt dokument av typen Application.
- Søk opp “Run Shell Script” automatisering og legg den til dokumentet.
- Som bash script: `/Applications/Arc.app/Contents/MacOS/Arc --remote-debugging-port=9222`
- Lagre filen som “Arc Debug” eller hva du vil.

Starte en debug sesjon (attach):

- Hvis du har Arc åpen, lukk den helt slik at den lille dotten forsvinner under appen på taskbar (høyreklikk på Arc i taskbar og Quit)
- Bruk den nye Arc Debug appen til å starte Arc på nytt med remote debugging port åpnet.
- I VS Code, gå til `Run and Debug` og velg `Attach to Arc Browser`, trykk Play.

Starte en debug sesjon (launch):

- Hvis du har Arc åpen, lukk den helt slik at den lille dotten forsvinner under appen på taskbar (høyreklikk på Arc i taskbar og Quit)
- I VS Code, gå til `Run and Debug` og velg `Launch Arc Browser`, trykk Play.

### NextJS serverside debugging

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Attach to Next.js (serverside debugging only)",
      "type": "node",
      "request": "attach",
      "skipFiles": ["<node_internals>/**"],
      "port": 9230
    }
  ]
}
```

### Flutter debugging

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "app_arat dev",
      "request": "launch",
      "type": "dart",
      "program": "lib/main_dev.dart",
      "args": ["--flavor", "dev"]
    }
  ]
}
```
