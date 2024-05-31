# Introduction

Typescript configuration for various host and modules. Scenarios are set in different branches.


# Module Resolution Algorithm

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

