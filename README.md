
# Introduction

<p style="font-size:1.2em">Typescript configuration for various host and modules. Scenarios are set in different branches.</p>

## Vocabulary
<p style="font-size:1.2em">There are few very important terms and concepts to understand</p>

### Host

<p style="font-size:1.2em">The system that ultimately consumes the output code to direct its module loading behavior. In another words it’s the system outside of TypeScript that TypeScript’s module analysis tries to model.</p>

* <p style="font-size:1.2em">When the output code generated by tsc or some 3rd party transpiler runs directly in runtime like nodejs then nodejs is host.</p>  
* <p style="font-size:1.2em">When there is no output code and a runtime consumes ts code directly runtime is still a host.</p>
* <p style="font-size:1.2em">When bundler input ts files and output bundle file then bundler is host.</p>
* <p style="font-size:1.2em">When loading module in web browser, typescript need to model 2 aspects web server and module loading in browser. Browser js engine decide which module format is supported and web server decide which module to load when imported inside another module.</p>

### Host Rules
1. <p style="font-size:1.2em">valid output module format</p>
2. <p style="font-size:1.2em">import in output module resolves successfully</p>
3. <p style="font-size:1.2em">assign correct type to imported names</p>

### Module Formats

<p style="font-size:1.2em">For question 1 is what kind of module host expects so that typescript output the file format to match that expectation, 
e.g. nodejs before v12 only supports cjs module. After node 12 esm and cjs modules are supported but we need both package.json and file extension to find what module will be output. Similarly browser supports esm modules after release of ES 2015.</p>

<p style="font-size:1.2em">module options passed to compiler will determins following</p>

* <p style="font-size:1.2em">format of output js module</p>
* <p style="font-size:1.2em">module kind detection</p>
* <p style="font-size:1.2em">how different kind of modules are allowed to import</p>
* <p style="font-size:1.2em">whether features like ```import.meta``` and top level ```await``` available 
</p>

|Format|Description|
|:---:|:---|
|node16|<p style="font-size:1.2em">reflect module system of node js 16+ which supports cjs and esm with particular interoparability and detection rules <span style="color:green">RECOMMENDED</span>|
|nodeNext|currently same as node 16 but its moving target to reflect node latest module loading <span style="color:green">RECOMMENDED</span>|
|es2015|Reflect ES2015 (module import and export added in this version initially)|
|es2020|Reflect ES2020 (```import.meta``` and ```import * as ns from "M1"``` support added in ES2015)
|es2022|Reflect ES2022 (top level ```await``` is added in ES2020)
|esNext|Currently same as ES2022, but its moving target reflect latest changes ES module loading
|common,system,amd,umd|Each emit everything in module system named and assumes everything can be successfully imported into that module system. <span style="color:red">NOT RECOMMENDED</span> in future projects.|


# Module Format Detection
<p style="font-size:1.2em">Node.js understands both ES modules and CJS modules, but the format of each file is determined by its file extension and the type field of the first package.json file found in a search of the file’s directory and all ancestor directories:</p>

| Input File  | Output File  |  Module Kind | package.json  | Reason  | Content |
|---|---|---|---|---|---|
|   package.json|   |   |   |   | {}|
|   /main.mts 	| /main.mjs 	 | esm  |   | file extension  | |
|  /utils.cts |  /utils.cjs | cjs  |   | file extension   | |
|   /example.ts| /example.ts  | cjs  |   | no type:"module" in package.json  | |
|   /node_modules/pkg/package.json|   |   |   |   | { "type": "module" }|
|  /node_modules/pkg/index.d.ts 	 |   | esm  |   | type module in package.json  | |
| /node_modules/pkg/index.d.cts  |   |  cjs |   |  file extension | |








# Scenarios 

## Branch basic-version



## ES Modules in NodeJS

### tsconfig.json

```
{
  "compilerOptions": {
    /* Language and Environment */
    "target": "es2016",                                  

    /* Modules */
    "module": "node16",                                

    /* Emit */
    "outDir": "./dist",                                   

    /* Interop Constraints */
    "esModuleInterop": true,                             
    "forceConsistentCasingInFileNames": true,            

    /* Type Checking */
    "strict": true,                                      

    /* Completeness */
    "skipLibCheck": true                                 
  }
}


```

## Source Files

### index.mts
```
import { greeting } from "./greeting.mjs";

console.log(greeting('shoaib'));
```

### greeting.mts
```
src/greeting.mts src/index.mts
```



# References

* [Typescript Theory Link](https://www.typescriptlang.org/docs/handbook/modules/theory.html#module-resolution)

* [module reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html#the-module-compiler-option)