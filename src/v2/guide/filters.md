---
title: Filter
type: guide
order: 305
---

Vue.js ermöglicht dir die Definition von Filtern, die zur Textformatierung nach einem allgemeinen Muster verwendet werden können. Filter können an zwei Orten eingesetzt werden: in **"Mustache"-Interpolationen und `v-bind`-Ausdrücken** (letztere werden ab Version 2.1.0 unterstützt). Filter werden einem JavaScript-Ausdruck hintangestellt und von diesem durch ein "Pipe"-Symbol abgetrennt:

``` html
<!-- in mustaches -->
{{ message | capitalize }}

<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
```

Du kannst einen Filter lokal in den options einer Komponente definieren:

``` js
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

oder global:

``` js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})
```

Hier ein Anwendungsbeispiel zu unserem `capitalize`-Filter:

{% raw %}
<div id="example_1" class="demo">
  <input type="text" v-model="message">
  <p>{{ message | capitalize }}</p>
</div>
<script>
  new Vue({
    el: '#example_1',
    data: function () {
      return {
        message: 'john'
      }
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
  })
</script>
{% endraw %}

Filterfunktionen erhalten als erstes Argument immer den Ausdruckswert (d. h. das Ergebnis der vorausgehenden Kette). Im obigen Beispiel wird die `capitalize`-Filterfunktion als erstes Argument also den Wert von `message` erhalten.

Filter können verkettet werden:

``` html
{{ message | filterA | filterB }}
```

In diesem Fall empfängt `filterA` (das gemäß seiner Definition nur ein Argument hat) den Wert von `message`. Anschließend wird die `filterB`-Funktion aufgerufen, wobei als einziges Argument das Ergebnis von `filterA` übergeben wird.

Filter sind JavaScript-Funktionen und können somit parametrisiert werden:

``` html
{{ message | filterA('arg1', arg2) }}
```

Hier ist `filterA` definiert als eine Funktion mit drei Argumenten: Der Wert von `message` wird als erstes Argument übergeben, die Zeichenkette `'arg1'` als zweites, und der Wert des Ausdrucks `arg2` als drittes.
