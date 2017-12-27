# hey-cli
Webpack scaffolding, hot-dev-server, build.  
Do not need to understand webpack, only need to know how to configure it to use, get rid of cumbersome duplication of webpack configuration.

## 中文文档
此处有[中文文档](https://github.com/heyui/hey-cli/blob/master/README_zh.md)


## Advantage
- Global installation, all development projects are supported, do not need to install and configure each project webpack.    
- Support <code>ES6</code>
- Support Hot Module Replacement
- Support Http Proxy
- Default support <code>vue2.0</code>, support <code>react</code>
- Support the global less parameter definition
- Build UMD mode code
- Only need to configure <code>hey.js</code> configuration file

## Installation

```sh
npm install -g hey-cli
```

## Configuration

Add the hey.js configuration file in the project root directory.  
```js
module.exports = {
  "port": 9002, //Port
  "dist": 'dist', //the root of the build file
  "timestamp": false, //the static folder generated by build with the static[timestamp] named folder
  "react": true, //support react project
  "webpack": { //webpack related configuration
    "console": false, //package compression whether to retain the console, the default is false
    "publicPath": "/", //public path
    "output": {
      // Output what documents, mainly html,
      // Default setting will load the same js file as the html file name for the entrance. 
      // Support for defining common packages.
      "./*.html": {
        // Load js file by default, and html automatically references. 
        //If not configured, the same js file as the html file name is automatically
        "entry":"./src/index.js", 
        "commons": [
          "common"
        ]
      }
    },

    //public package definition
    "commonTrunk": {
      "common": [
        "jquery",
        "vue",
        "vuex",
        "manba",
        "jsoneditor"
      ]
    },

    //define resolve, https://webpack.js.org/configuration/resolve/
    "resolve": {
      "alias": {}
    },

    //define global, https://webpack.js.org/plugins/provide-plugin
    "global": {
      "Vue": "vue",
      "$": "jquery",
      "log": "./js/common/log"
    },

    //define proxy, https://webpack.js.org/configuration/dev-server/#devserver-proxy
    "devServer": {
      "proxy": {
        "/api": {
          "target": "http://yoda:9000"
        }
      },
      historyApiFallback: true
    },
    //define externals, https://webpack.js.org/configuration/externals/
    "externals":{

    },

    //Define the global less parameter definition, you can use the globalVars parameter in any less
    globalVars: './static/css/var.less',
  },

  // The files that are not referenced are copied to the packaged folder when build
  "copy": [
    "./images/**/*",
    "./help/**/*",
    "./template/**/*"
  ]
};
```

### Extended Configuration
You can expand and configure the following properties in the webpack configuration item in hey.js:  
- plugins
- module
- node
- externals
- devServer

Specific use, please refer to [webpack](https://webpack.js.org/) document.

## Example

### Load vue,vue-router

```json
"hey": {
  "port": 9008,
  "timestamp": true,
  "dist": "gen",
  "webpack": {
    "publicPath": "/",
    "output": {
      "./*.html": {
        "entry":"./src/app",
        "common":["common"]
      }
    },
    "commonTrunk": {
      "common":["vue","vue-router"]
    },
    "global": {
      "Vue": "vue"
    },
    "devServer": {
      "historyApiFallback":true
    }
  }
}
```
### External loading vue,vue-router  

```json
"hey": {
  "port": 9008,
  "timestamp": true,
  "dist": "gen",
  "webpack": {
    "publicPath": "/",
    "output": {
      "./*.html": {
        "entry":"src/app"
      }
    },
    "global": {
      "Vue": "vue"
    },
    "devServer": {
      "historyApiFallback":true
    },
    "externals": {
      "Vue": "window.Vue",
      "VueRouter": "window.VueRouter"
    }
  }
}
```

### Common code to build UMD mode
Mainly used to build some of the common code, simple configuration can be used.   
*Because it is a public package packaged into UMD mode, do not use the import mode.*

```js
module.exports = {
  dist: "build",
  webpack: {
    umd: {
      entry: "./src/index.js",
      library: "Validator",
      filename: 'validator.js' //build generation /build/validator.js
    },
    externals: {
      "manba": "manba"  //The dependent package will not be packaged into the source code
    }
  }
};
```

## Deploy

Start webpack server

```sh
hey dev
hey build
```
<!-- 
## Attention

For the Vue project, the project itself needs to install the <code>vue-template-compiler</code> package corresponding to the Vue version.  
If your project is using the latest version of vue, use the following command to perform the installation 

```sh
npm install --save vue
npm install --save-dev vue-template-compiler
```

If your project uses a fixed version of vue, use the following command to perform the installation
```sh
npm install --save vue@version
npm install --save-dev vue-template-compiler@version
``` -->
