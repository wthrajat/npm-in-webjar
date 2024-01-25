# NPM in WebJar [![Generic badge](https://img.shields.io/badge/NPM%20in-WebJar-pink.svg)](https://shields.io/) [![Maven](https://badgen.net/badge/icon/maven?icon=maven&label)](https://https://maven.apache.org/) [![TypeScript](https://img.shields.io/badge/--3178C6?logo=typescript&logoColor=ffffff)](https://www.typescriptlang.org/) [![GitHub license](https://img.shields.io/github/license/Naereen/StrapDown.js.svg)](https://github.com/Naereen/StrapDown.js/blob/master/LICENSE) [![Npm](https://badgen.net/badge/icon/npm?icon=npm&label)](https://https://npmjs.com/)

### How to use
```sh
git clone https://github.com/wthrajat/npm-in-webjar.git && cd npm-in-webjar && mvn clean install
```


### Development

The directory `src/main/resources` is now an NPM project - like the one you probaby have worked on usually.

### Version upgrade

1. Edit `src/main/resources/package.json` for the deps update
2. `cd src/main/resources` and run `npm install`
3. This will update `src/main/resources/package-lock.json`
> NOTE: Without this step, the updated version is not taken into account during the maven build.

### Example of `src/main/resources/main.ts`

```typescript
    export default function helloWorld(name : string) {
    console.log("Hello " + name + "!");
}
    export default helloWorld;
```

### Using the output bundle

```typescript
  require.config({
    paths: {
      'output': '$services.webjars.url('domain.org.name:npm-in-webjar', 'output.js')'
    }
  });

  require(['output'], function (output) {
        console.log('Your output bundle has been loaded successfully!');
        name = "Taylor Swift";
        bundle.helloWorld(name);
     // Prints: Hello Taylor Swift!
});
```

## How is the build done

1. At first, maven copy `src/main/resources` to `target/webjar`
2. Then `npm ci` is run inside `target/webjar` installing the dependencies by following `package-lock.json`
3. Then `npm run build` is run inside `target/webjar`, producing the artifacts of `target/webjar/dist`
4. At last, the content of `target/webjar/dist` is copied to the relevant directory to be included in the WebJar packaging.