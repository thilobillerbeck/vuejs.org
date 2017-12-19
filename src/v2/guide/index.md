---
title: Einführung
type: guide
order: 2
---

## Was ist Vue.js?

Vue (ausgesproche /vju:/, wie engl. **view**) ist ein progressives Framework um User-Interfaces zu erstellen. Anders als andere große Frameworks wurde Vue von Grund auf für gute Adaptivität ausgelegt. Die Grundbibliothek ist nur auf den "View Layer" ausgelegt, leicht zu erlenern und gut in ein Projekt mit anderen Bibliotheken zu integrieren. Auf der anderen Seite kann man mit Vue auch größere Single-Page Applikationen mithilfe von [modernem Tooling](single-file-components.html) und [unterstützten Biliotheken](https://github.com/vuejs/awesome-vue#components--libraries) realisieren.

Wenn du bereites ein erfahrener Frontend Entwickler bist und Vue gerne mit anderen Bibliotheken/Frameworks vergleichen möchtest, dann lese hier einen [Vergleich mit anderen Frameworks](comparison.html).

## Getting Started

<p class="tip">Der offizielle Guide setzt erweiterte Grundkenntnisse in HTML, CSS und Javascript vorraus. Wenn du komplett neu in der Frontend Entwicklung bist, ist es vielleicht keine gute Idee mit einem Framework anzufangen. Lerne die Grundlagen und komme dann zurück! Erfahrungen mit anderen Frameworks sind hilfreich, aber nicht notwendig.</p>

Der einfachste Weg um Vue.js auszuprobieren ist es, das [JSFiddle Hello World Beispiel](https://jsfiddle.net/chrisvfritz/50wL7mdz/) auszuprobieren. Öffne es doch einfach in einem neuen Tab und arbeite dort die hier gezeigten Beispiele durch. Du kannst auch eine <a href="https://gist.githubusercontent.com/chrisvfritz/7f8d7d63000b48493c336e48b3db3e52/raw/ed60c4e5d5c6fec48b0921edaed0cb60be30e87c/index.html" target="_blank" download="index.html"><code>index.html</code> Datei</a> erstellen und Vue mit:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

einbinden.

Die [Installationsseite](installation.html) erklärt die verschiedenen Optionen um Vue zu installieren. Bitte bedenke, dass wir dir **nicht empfehlen** mit der `vue-cli` zu beginnen, besonders dann nicht, wenn du dich mit node.js basierten Build Tools nicht auskennst.

## deklaratives Rendering

Im Kern ist Vue.js ein System, welches dir ermöglicht deklarativ Daten mit einer einfachen Syntax im DOM zu rendern:

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hallo Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hallo Vue!'
  }
})
</script>
{% endraw %}

Super, du hast gerade deine erste Vue App erstelt! Es sieht so aus, als hätten wir ein String Template gerendert, doch unter der Haube macht Vue viel mehr. Daten und DOM sind nun miteinander verbunden und alles ist **reactive**. Wie können wir das herausfinden? Öffne einfach mal die Browserkonsole und setze `app.message` auf einen anderen Wert. Das obige Beispiel sollte sich mit dem Verändern aktualisieren.

Abseits von Textinterpolation können wir auch wie folgt Attribute an ein Element binden:

``` html
<div id="app-2">
  <span v-bind:title="message">
    Hover mit der Maus ein paar Sekunden über mich, um einen dynamisch angebundenen Titel zu sehen.
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Die Seite wurde um ' + new Date().toLocaleString() + ' geladen'
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Hover mit der Maus ein paar Sekunden über mich, um einen dynamisch angebundenen Titel zu sehen.
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Die Seite wurde um ' + new Date().toLocaleString() + ' geladen'
  }
})
</script>
{% endraw %}

Hier machen wir mit einem neuen Element Bekanntschaft. Das hier genutzte `v-bind` Attribut wird auch **directive** genannt. Directives fangen mit `v-` an um zu zeigen, dass sie ein spezielle Vue Attribut sind. Wie du vielleicht schon erraten hast, haben diese Attribute bestimmte datenabhängige Fähigkeiten im gerenderten DOM. Hier macht das Attribut nichts anderes als zu sagen: "Gleiche das `title` Attribut des Elements immer mit dem `message` Wert in der Vue Instanz ab.". 

Wenn du nun wieder die JavaScript Konsole öffnest und `app2.message = 'irgendeine Nachricht'` eingibst, so aktualisiert sich wieder das `title` Attribut im obigen Beispiel.

## Bedingungen und Schleifen

Es ist sehr leicht, die Sichtbarkeit eines Elements zu steuern:

``` html
<div id="app-3">
  <span v-if="seen">Jetzt siehst du mich</span>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Jetzt siehst du mich</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Versuch direkt einmal `app3.seen = false` in die Konsole einzugeben, die Nachricht sollte verschwinden.

Diese Beispiel zeigt sehr gut, dass wir nicht nur Daten, sondern auch die Struktur, im DOM verändern können. Vue bringt sogar ein sehr mächtiges Übergangseffektsystem mit, welches automatisch [Übergangseffekte](transitions.html) anwendet, wenn Elemente hinzugefügt/aktualisiert/entfernt werden.

Es gibt noch einige andere Directives, jedes mit seiner eigenen Funktionalität. Beispielsweise das `v-for` Directive, welches genutzt werden kann um eine Liste von Elementen anhand eines Arrays auszugeben:

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Lerne JavaScript' },
      { text: 'Lerne Vue' },
      { text: 'Erschaffe etwas großartiges' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Lerne JavaScript' },
      { text: 'Lerne Vue' },
      { text: 'Erschaffe etwas großartiges' }
    ]
  }
})
</script>
{% endraw %}

Gebe nun in die Konsole `app4.todos.push({ text: 'Neues Element' })` ein. In der Liste sollte nun ein neues Element auftauchen.

## Nutzereingaben verarbeiten

Damit Nutzer mit deiner App interagieren können, kannst du das `v-on' Directive benutzen, um Listeners zu Elementen hinzuzufügen. Diese führen dann Methoden aus, die in der Vue Instanz definiert wurden:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Nachricht umdrehen</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hallo Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Nachricht umdrehen</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hallo Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

In diesem Fall updaten wir den State unserer App, ohne das DOM manuell zu verändern. Die gesammte DOM Manipulation wird von Vue übernommen. So kannst du dich mit deinem Code auf die Logik deiner App konzentrieren.

Vue bietet dir auch ein `v-model` Directive, welches einfach ein bidirektoinales Binding zwischen Eingabefeld und App State erstellt:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hallo Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hallo Vue!'
  }
})
</script>
{% endraw %}

## Kombinieren von Komponenten

Das Komponentensystem ist ein weiteres Grundkonzept von Vue, da es uns danke Abstraktion ermöglicht, große Applikationen mit kleinen, eigenständigen und universell nutzbaren Komponenten zu entwickeln. Wenn man genauer drüber nachdenkt, so kann man fast jede Applikation auf einen Bauem aus Komponenten abbilden:

![Component Tree](/images/components.png)

Eine Komponente in Vue ist im Wesentlichen eine Vue Instanz mit einigen vordefinierten Werten. Das Einfügen einer Komponente in Vue ist einfach:

``` js
// erstelle eine neue Komponente mit dem Namen 'todo-item'
Vue.component('todo-item', {
  template: '<li>Das ist eine Aufgabe</li>'
})
```

Nun kann man die Komponente in dem Template einer anderen Komponente verwenden:

``` html
<ol>
  <!-- Erstelle eine Instanz der 'todo-item' Komponente -->
  <todo-item></todo-item>
</ol>
```

Hier würde alerdings immer der gleiche Text für jedes 'todo-item' gerendert werden. Wir sollten in der Lage sein, Daten vom Parent Objekt in den Scope des Child Objekts zu übergeben. Modifizieren wir nun die Komponente so, dass sie eine [prop](components.html#Props) (engl. property => dt. Attribut) akzeptiert:

``` js
Vue.component('todo-item', {
  // Die todo-item Komponente kann nun
  // eine "prop" akzeptieren. Diese ist äuqivalent
  // zu eine Attribut. Die "prop" hier heißt todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Nun können wir mit `v-bind` Daten an jede einzelne Komponente übergeben:

``` html
<div id="app-7">
  <ol>
    <!--
      Nun können wir jedem todo-item ein todo Objekt geben
      welches es repräsentieren soll. So bleibt der Inhalt
      dynamisch. Wir müssen außerdem jeder Komponente einen "Key"
      übergeben, was später noch erklärt wird.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Gemuese' },
      { id: 1, text: 'Kaese' },
      { id: 2, text: 'Was auch immer Menschen sonst so essen' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item" :key="item.id"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Gemuese' },
      { id: 1, text: 'Kaese' },
      { id: 2, text: 'Was auch immer Menschen sonst so essen' }
    ]
  }
})
</script>
{% endraw %}

Das hier ist ein sehr gekünsteltes Beispiel, aber wir haben unsere App in zwei Komponenten unterteilen können. Die Child Komponente ist dank des props Interface saube von der Parent Komponente getrennt. Wir haben nun die Möglichkeit, unser `<todo-item>` mit komplexer Templatestruktur oder Logik auszustatten ohne die Parent Komponente zu beeinträchtigen.

In großen Applikationen ist es essentiell alles in Komponenten zu unterteilen um einen störungsfreien Entwicklungsablauf zu gewährleisten. [Später](components.html) gehen wir in sachen Komponenten noch etwas mehr ins Detail gehen. Aber hier ist ein Beispiel, wie ein App-Template mit Komponenten aussehen könnte:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Verwandtschaft mit Custom Elements

Wie dir vielleicht aufgefallen ist haben Vue Komponenten eine große Ähnlichkeit mit **Custom Elements**, welche Teil der 
[Web Components Spec](https://www.w3.org/wiki/WebComponents/) sind. Das resultiert daraus, dass die Syntax von Vue Komponenten lose nach der Spezifikation definiert wurde. Beispielswiese implementieren Vue Komponenten die [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) und das spezielle `is` Attribut. Trotzdem gibt es einige gravierende Unterschiede:

1. Die Web Components Spezifikation ist immernoch in der Entwicklung, und wird daher noch nicht von jedem Browser nativ unterstützt. Vue Komponenten brauchen keine Polyfills um konsistent in allen Browsern (IE9 oder neuer) zu funktionieren. Wenn gewünscht, können Vue Komponenten auch in ein natives Custom Element gepackt werden.

2. Vue Komponenten bieten einige Features welche in Custom Elements nicht implementiert sind. Bekannte Vertreter sind hier "cross-component data flow", "custom event communication" und "build tool integrations".

## Bereit für mehr?

Wir haben hier die Basics von Vue.js's Grundbibiliothek erklärt - Der Rest des Guides beschäftig sich mit diesen und weiteren fortgerschirtteneren Features im Detail. Lese dir daher am besten alles durch!
