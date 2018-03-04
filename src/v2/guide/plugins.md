---
title: Plugins
type: guide
order: 304
---

## Ein Plugin schreiben

Für gewöhnlich fügen Plugins globale Funktionalitäten zu Vue hinzu. Der Zweck und Einsatzbereich von Plugins lässt sich nicht strikt abgrenzen, jedoch lassen sie sich typischerweise den folgenden Arten zuordnen:

1. Fügt globale Methoden oder Eigenschaften hinzu; z.B. [vue-custom-element](https://github.com/karol-f/vue-custom-element)

2. Fügt ein oder mehrere globale Assets hinzu: Direktiven/Filter/Transitionen usw.; z.B. [vue-touch](https://github.com/vuejs/vue-touch)

3. Fügt über ein globales Mixin Komponentenoptionen hinzu; z.B. [vue-router](https://github.com/vuejs/vue-router)

4. Fügt Vue-Instanzmethoden hinzu, indem sie an `Vue.prototype` angehängt werden.

5. Eine Bibliothek mit einer eigenen API, die gleichzeitig aber noch eine Kombination der obigen Punkte umsetzt; z.B. [vue-router](https://github.com/vuejs/vue-router)

Ein Vue.js-Plugin sollte eine `install`-Methode aufweisen. Diese Methode wird von Vue mit dem `Vue`-Konstruktor als erstem Argument aufgerufen, zusammen mit optionalen `options`:

``` js
MyPlugin.install = function (Vue, options) {
  // 1. Füge globale Methoden oder Eigenschaften hinzu
  Vue.myGlobalMethod = function () {
    // beliebige Programmlogik ...
  }

  // 2. Füge ein oder mehrere globale Assets hinzu
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // beliebige Programmlogik ...
    }
    ...
  })

  // 3. Füge Komponentenoptionen hinzu
  Vue.mixin({
    created: function () {
      // beliebige Programmlogik ...
    }
    ...
  })

  // 4. Füge eine Instanzmethode hinzu
  Vue.prototype.$myMethod = function (methodOptions) {
    // beliebige Programmlogik ...
  }
}
```

## Ein Plugin verwenden

Verwende ein Plugin durch Aufrufen der globalen Methode `Vue.use()`:

``` js
// ruft `MyPlugin.install(Vue)` auf
Vue.use(MyPlugin)
```

So kannst Du `options` übergeben:

``` js
Vue.use(MyPlugin, { someOption: true })
```

`Vue.use` hindert Dich automatisch daran, dasselbe Plugin mehr als einmal zu verwenden. Mehrfache Aufrufe mit demselben Plugin installieren dieses nur ein einziges Mal.

Gewisse offizielle Plugins von Vue.js wie beispielsweise `vue-router` rufen `Vue.use()` automatisch auf, wenn `Vue` als globale Variable verfügbar ist. In einer Modulumgebung wie etwa CommonJS musst Du `Vue.use()` allerdings immer explizit aufrufen:

``` js
// Bei der Verwendung von CommonJS via Browserify oder Webpack
var Vue = require('vue')
var VueRouter = require('vue-router')

// Vergiss den folgenden Aufruf nicht!
Vue.use(VueRouter)
```

Sieh Dir [awesome-vue](https://github.com/vuejs/awesome-vue#components--libraries) an, eine riesige Sammlung von Plugins und Bibliotheken, die von der Community beigesteuert werden.
