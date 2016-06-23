# plover-module-resolver


[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][coveralls-image]][coveralls-url]


【核心模块】 plover模块加载和解析器


## Usage
```js
const path = require('path');
const ModuleResolver = require('plover-module-resolver');

const options = {
  development: true,
  applicationRoot: path.resolve('./'),
  libModulesDir: path.resolve('./node_modules'),
  modulesDir: path.resolve('./modules')
};

const moduleResolver = new ModuleResolver(options);

console.log(moduleResolver.list());  // 模块列表
```

`plover-module-resolver`先加载`libModulesDir`目录下的plover库模块，然后再加载`modulesDir`目录中的plover应用模块。

**Note**

某些情况下需要对库模块进行自定义，此时在`modulesDir`中定义同名模块，`plover-module-resolver`将会覆盖`libModulesDir`中模块。


## Options
### development
是否为开发模式，默认为非开发模式。

### applicationRoot
应用根目录。

### libModulesDir
plover库模块目录，默认为`applicationRoot`应用根目录下的`node_modules`目录。

在`package.json`中定义了`plover`信息的模块为plover库模块。

```json
{
  "name": "plover-xxx",
  "version": "0.1.0",
  "description": "xxx",
  "plover": {
    "plugin": "lib/plugin.js"
  }
}
```

### modulesDir
plover应用模块目录，默认为`applicationRoot`应用根目录下的`modules`目录。


## Methods
### list
返回模块列表。

### loadModule
加载模块信息。

```js
moduleResolver.loadModule(path.resolve('./tempModules/test'));
```

##### parameters
| name | description |
|:-----|:------------|
| path | 模块根目录 |
| options | 配置可选项 |

 - options配置可选项：

| name | description |
|:-----|:------------|
| namespace | 匿名模块的名字空间 |
| ensure | 是否校验模块`package.json`中必须存在`plover`配置 |
| silent | 当解析模块不存在时，是否忽略，若为`false`，系统将抛出异常 |

### pushModule
添加模块信息。

```js
moduleResolver.pushModule({
  name: 'test',
  version: '0.0.1',
  path: path.resolve('./tempModules/test'),
  view: {
    template: 'views/view.art',
    js: 'js/view.js',
    css: 'css/view.css'
  },
  // ...
});
```

##### parameters
| name | description |
|:-----|:------------|
| info | 模块信息 |

### resolve
返回指定名称的模块信息。在开发模式中每次都重新加载模块信息（隔间时间为3秒）, 在生产环境时直接从缓冲中获取。

##### parameters
| name | description |
|:-----|:------------|
| name | 模块名称 |

### vertify
验证模块依赖是否兼容, 如果有问题则会抛出Error。若在`package.json`的`plover`中定义了`dep`依赖模块信息，此时将会校验plover模块依赖的版本是否兼容。


[npm-image]: https://img.shields.io/npm/v/plover-module-resolver.svg?style=flat-square
[npm-url]: https://www.npmjs.com/package/plover-module-resolver
[travis-image]: https://img.shields.io/travis/plover-modules/plover-module-resolver/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/plover-modules/plover-module-resolver
[coveralls-image]: https://img.shields.io/codecov/c/github/plover-modules/plover-module-resolver.svg?style=flat-square
[coveralls-url]: https://codecov.io/github/plover-modules/plover-module-resolver?branch=master