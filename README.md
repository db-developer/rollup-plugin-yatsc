# rollup-plugin-yatsc

Yet another [Rollup](https://github.com/rollup/rollup) plugin for transpiling Typescript.  

This project is a fork of [rollup-plugin-tsc](https://github.com/tsne/rollup-plugin-tsc) v 1.1.15 (MIT).
Thanks to tsne (Thomas Nitsche) for the work on his project on which the adjustments for this plugin are based.

rollup-plugin-yatsc aims to be callable by grunt and provides the option to pass in tsconfig by pojo as well as by filename.

## Installation
```
npm install --save-dev rollup-plugin-yatsc
```

## Usage
```javascript
// rollup.config.js
import yatsc from "rollup-plugin-yatsc";

export pojodefault {
	input: "src/main.ts",

	plugins: [
		yatsc({
			// put your tsconfig here
		}),
	]
};

export filedefault {
	input: "src/main.ts",

	plugins: [
		yatsc("build/tsconfig.es5.json"),
	]
};
```
To configure the Typescript compiler, a [tsconfig](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) has to be passed to the plugin. This can either be by providing a plain old javascript object or by specifying a `path/to/tsconfig.json`.

```javascript
// gruntfile.js
const yatsc = require( "rollup-plugin-yatsc" );

module.exports = function( grunt ) {
  let pkgjson = grunt.file.readJSON( "package.json" );
  let scope   = pkgjson.name.slice( 0, pkgjson.name.indexOf("/")).replace( "@", "" );
  let pkgname = pkgjson.name.slice( pkgjson.name.indexOf("/") + 1 );
  let pkglobl = scope ? `${ scope }.${ pkgname }` : pkgname;

  grunt.initConfig({
    rollup: {
      esm5: {
        src   : "build/public-api.ts",
        dest  : `dist/${ pkgname }/esm5/${ pkglobl }.js`,
        options: {
          plugins   : [ yatsc( `${ BUILD }/tsconfig.esm5.json` )],
          name      : `${ pkglobl }`,
          format    : "esm",
          sourcemap : "inline"
        }
      }
    }, // ...
  });

  grunt.loadNpmTasks( "grunt-rollup" );

  grunt.registerTask( "default", [ "rollup" ]);
};

```
