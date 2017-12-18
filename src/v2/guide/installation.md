---
title: Installation
type: guide
order: 1
vue_version: 2.5.11
gz_size: "30.59"
---

### Kompatiblitätshinweis

Vue unterstützt **keinen** Internet Explorer 8 (und ältere Versionen), da es von diesem nicht unterstützte ECMAScript 5 Features benutzt. VueJS unterstützt allerdings alle [ECMAScript 5 kompatiblen Browser](http://caniuse.com/#feat=es5).

### Release Notes

Letzte stabile Version: {{vue_version}}

Detaillierte Release Notes für jede Version sind auf [GitHub](https://github.com/vuejs/vue/releases) zu finden.

## Vue Devtools

Zum arbeiten mit Vue empfehlen wir die Installation der [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools) für deinen Browser. Diese erlauben dir ein einfacheres Inspizieren und Debuggen deiner Applikationen.

## Implementation mittels `<script>` Tag
Lade dir einfach Vue herunter und binde es mit einem `<script>` Tag ein. `Vue` wird automatisch als globale Variable eingetragen.

<p class="tip">Nutze zum Entwickeln nicht die komprimierte Version, sie zeigt dir keine Hinweise an.</p>

<div id="downloads">
<a class="button" href="/js/vue.js" download>Development Version</a><span class="light info">Mit allen Hinweisen und Debug Modus</span>

<a class="button" href="/js/vue.min.js" download>Production Version</a><span class="light info">Hinweise entfernt, {{gz_size}}KB min+gzip</span>
</div>

### CDN

Empfohlen: [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue), liefert immer die aktuellste Version die auf npm zu finden ist. Der Quellcode ist unter [https://cdn.jsdelivr.net/npm/vue/](https://cdn.jsdelivr.net/npm/vue/) einsehbar.

Auch verfügbar unter [unpkg](https://unpkg.com/vue) und [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) (cdnjs braucht manchaml etwas Zeit um zu synchronisieren, daher kann es auch manchmal ältere Versionen enthalten).

## NPM

NPM ist die empfohlene Installationsmethode für größere Vue Projekte. Es lässt sich gut in "module bundlers" wie [Webpack](https://webpack.js.org/) oder [Browserify](http://browserify.org/) integrieren. Vue bietet ebenfalls Tools um [Single File Components](single-file-components.html) zu erstellen.

``` bash
# installiert das aktuellste 'stable' release
$ npm install vue
```

## CLI
Vue.js bietet auch ein [offizieles CLI](https://github.com/vuejs/vue-cli) um in wenigen Minuten 'Single Page Applications' mithilfe eines 'batteries included' Setups für einen modernen Workflow aufzusetzen. Danach unterstützt das Projekt schon 'hot realod', 'lint on save' und Builds für Produktivsysteme.

``` bash
# installiere vue-cli
$ npm install --global vue-cli
# erstelle ein neuese Projekt mit dem "webpack" Template
$ vue init webpack my-project
# installiere die Abhängigkeiten und starte das Projekt
$ cd my-project
$ npm install
$ npm run dev
```

<p class="tip">
Die Vue.js CLI setzt Grundkenntnisse in node.js und entsprechenden Build Tools vorraus. Wenn du keine Erfahrung mit Vue.js oder Front-End Build Tools hast, empfehlen wir dir <a href="./">unseren Guide</a> ohne Build Tools durchzuarbeiten, bevor du das CLI benutzt.</p>

## Erklärung der verschiedenen Build Versionen

Im [`dist/` Ordner des NPM Pakets](https://cdn.jsdelivr.net/npm/vue/dist/) sind verschiedene Builds von Vue.js zu finden. Hier eine Übersicht der Unterschiede:

| | UMD | CommonJS | ES Module |
| --- | --- | --- | --- |
| **Full** | vue.js | vue.common.js | vue.esm.js |
| **Runtime-only** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js |
| **Full (production)** | vue.min.js | - | - |
| **Runtime-only (production)** | vue.runtime.min.js | - | - |

### Begriffserklärung

- **Full**: Builds welche sowohl Compiler als auch Runtime enthalten

- **Compiler**: Code, welcher Vue.js Templates in Javascript render Funktionen umwandelt

- **Runtime**: Code, welcher Vue Instanzen erstellt, den DOM-Tree rendert und Objekte einhängt. Im Prinzip alles ohne den Compiler.

- **[UMD](https://github.com/umdjs/umd)**: UMD Builds können direkt per `<script>` Tag eingebaut werden. Der Standardbuild auf jsDelivr [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue) enthält sowohl Runtime als auch Compiler.

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS Builds sind dafür da, um mit älteren Bundlern wie [browserify](http://browserify.org/) oder [webpack 1](https://webpack.github.io) genutzt zu werden. Die Standarddatei für solche Bundler (`pkg.main`) enthält nur die Runtime.

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: ES module Builds sind dafür da, um mit modernen Bundlern wie [webpack 2](https://webpack.js.org) oder [rollup](https://rollupjs.org/) genutzt zu werden. Die Standarddatei für solche Bundler (`pkg.module`) enthält nur die Runtime.


### Runtime + Compiler vs. Runtime-only

Wenn Templates compiliert werden müssen (also Strings an die template Funktion übergeben oder Elemente in andere durch Vue definierte eingehängt werden müssen) muss der Compiler und damit der Full Build genutzt werden:

``` js
// hier wird der Compiler gebraucht
new Vue({
  template: '<div>{{ hi }}</div>'
})

// hier wird er nicht gebraucht
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```
Wenn der `vue-loader` or `vueify` genutzt werden, werden Templates in `*.vue` Dateien bereits zur Compilezeit  in Javascript umgewandelt. Daher wird der Compiler im finalen Bundle nicht benötigt und somit kann ein Runtime only Build genutzt werden.

Da der Runtime-only Build auch 30% kleiner als der Full Build ist, sollte er immer bevorzugt genutzt werden. Will man dennoch den Full Build benutzen, so muss man einen Alias im Bundler anlegen:

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' für webpack 1
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': 'vue/dist/vue.esm.js'
    })
  ]
})
```

#### Browserify

Füge folgendes in die `package.json` ein:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

### Development vs. Production Version
Development/pdocution Versionsn sind hard-gedcodete UML Builds: Die unkomprimierten Dateien sind für die Entwicklung und die komprimierten für production Umgebungen.

CommonJS und ES Module Builds are intended for bundlers, sind für die Nutzung mit Bundlern gedacht. Daher werden hierfür keine komprimierten Versionen angeboten, man muss dies also selbst tun

CommonJS und ES Module Builds lesen auch `process.env.NODE_ENV` um zu entscheiden, in welchem Modus Vue arbeitet. Das asutauschen von `process.env.NODE_ENV` mit String Literalen erlaubt ebenfalls Tools wie UglifyJS "development only" Blöcke zu filtern und so die finale Packetgröße zu verkleinern.

#### Webpack

Nutzung mit Webpack's [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

Nutzung mit Rollup's [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

Füge eineb globale [envify](https://github.com/hughsk/envify) Transform zu deinem Bundle hinzu.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Siehe auch [Production Deployment Tips](deployment.html).

### CSP Umgebungen

Manche Umgebungen, wie bspw. Google Chrome Apps, erzwingen eine Content Security Policy (CSP), welche die Nutzung von `new Function()` zur Evaluierung von Ausdrücken verhindert. Da der Full Build of diesem Feature beruht um Templates zu compilieren, ist er in diesen umgebungen nicht nutzbar.

Der Runtime-only Build ist allerdings komplett mit CSPs kompatibel. Wenn Runtime-only Builds zusammen mit [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) oder [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple) genutzt werden, so werden die Templates bereits zur Compilezeit in `render` Funktionen umgewandelt welche in CSP Umgebungen genutzt werden dürfen.

## Dev Build

**Wichtig**: Die im `/dist` Ordner der GitHub Repository liegenden Builds werden nur zum Release aktualisiert, daher müssen sie selbst kompiliert werden, wenn man die mit dem Source Code äquivalente Version nutzen möchte:

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

In Bower sind nur UMD Module vorhanden.

``` bash
# Letzte 'stable' Version
$ bower install vue
```

## AMD Module Loaders

Alle UMD Builds können direkt als AMD Modul verwendet werden.