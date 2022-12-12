1. Start from a basic React app boilerplate.
2. Add Webpack 5.
3. Add the following to the webpack.config.js file:

```
experiments: {
    asyncWebAssembly: true,
    syncWebAssembly: true,
  },
```

and this:

```
devServer: {
    historyApiFallback: true,
    client: {
      overlay: false,
    },
  },
```

4. Inside src/lib create a folder called 'emurgo' and inside that create a file called 'loader.ts' with the following content:

```
type Lib = typeof import('@emurgo/cardano-serialization-lib-browser');

class Module {
  private _wasm: Lib | null = null;

  private async load(): Promise<Lib> {
    if (!this._wasm) {
      this._wasm = await import('@emurgo/cardano-serialization-lib-browser/cardano_serialization_lib');
    }
    return this._wasm;
  }

  async CardanoWasm(): Promise<Lib> {
    return this.load();
  }
}

export const EmurgoModule: Module = new Module();
```

5. Add the following dependency and run 'npm install':

```
"@emurgo/cardano-serialization-lib-browser": "11.1.0",
```

6. Add the following to your App.tsx file, before 'return':

```
  EmurgoModule.CardanoWasm().then((cardano) => {
    console.log(cardano);
  });
```

7. Run 'npm start' and check if a new instance of the loader is visible in the console.

8. Add the following dependency and run 'npm install':

```
"bip39": "^3.0.4"
```

This is the Bitcoin Improvement Proposal (in this case it's number 39) which handles the seedphrase.
