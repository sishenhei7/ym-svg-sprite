## yi-svg-sprite
[![NPM version][version-img]][versions-img] [![Build status][ci-img]][ci-url] [![CodeClimate score][codeclimate-img]][codeclimate-url] [![Documentation score][docs-coverage-img]][docs-coverage-url] [![Dependencies status][deps-img]][deps-url] [![Dev dependencies status][dev-deps-img]][dev-deps-url] [![NPM downloads][downloads-img]][npm-url]

`yi-svg-sprite` is used to build svg sprite in vue

## Getting Started

### install

To begin, you'll need to install svg-sprite-loader and yi-svg-sprite:

```
npm i svg-sprite-loader --save-dev
npm i yi-svg-sprite
```

### vue.config.js

Then add these settings to your vue.config.js file(if it does not exist, create one):

```
const path = require("path");

module.exports = {
  chainWebpack: config => {
    const resolve = dir => path.join(__dirname, dir);
    const svgRule = config.module.rule('svg');
    svgRule.uses.clear();

    svgRule
      .test(/\.svg$/)
      .oneOf('normal')
      .exclude
        .add(resolve('src/assets/sprite'))
        .end()
      .use('file-loader')
        .loader('file-loader')
        .end()
      .end()
      .oneOf('sprite')
      .include
        .add(resolve('src/assets/sprite'))
        .end()
      .use('svg-sprite-loader')
        .loader('svg-sprite-loader')
        .options({
          symbolId: 'sprite-[name]'
        });
  }
};
```

#### tips
All your svg files should be placed under `src/assets/sprite` folder.

### main.js

Finally, import yi-svg-sprite in your main.js:

```
import SvgSprite from 'yi-svg-sprite';

Vue.use(SvgSprite);
const requireAll = requireContext => requireContext.keys().map(requireContext);
const req = require.context('./assets/sprite', false, /\.svg$/);
requireAll(req);
```

#### tips
You can modify `src/assets/sprite` in both vue.config.js and main.js to change the default sprite file.


## examples

You can use YiSvg as a vue component anywhere like this:

```
<yi-svg svgId="haha" className="app-svg"/>
```

#### tips
`svgId` is required, it is the name of your svg file.

## ToDo

- implement `svgo` plugin.
- change it to vue-cli3 plugin.
