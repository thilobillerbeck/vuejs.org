---
title: Die Vue Instanz
type: guide
order: 3
---

## Erstellen einer neuen Vue Instanz

Jede Vue Applikation startet mit der Definition einer neuen **Vue Instanz** mithilfe der `Vue` Funktion:

``` js
var vm = new Vue({
  // Optionen
})
```

Auch wenn das [MVVM Pattern](https://en.wikipedia.org/wiki/Model_View_ViewModel) hier keine vollständige Anwendung findet, wurde das Design von Vue teilweise davon inspiriert. Eine Konvention ist daher, dass wir die Variable `vm` (View Model) benutzen, wenn wir eine Vue Instanz referenzieren.

Wenn du eine Vue Instanz erstellst, übergibst du ein **Optionsobjekt**. Ein Großteil dieses Guides erklärt dir, wie du mit diesen Optionen das vom Objekt erwartete Verhalten erreichst. Du kannst die vollständige Liste der Optionen in der [API Referenz](../api/#Options-Data) nachschlagen.

Eine Vue Applikation besteht aus einer **Root Vue Instanz** welche mit `new Vue` erstellt wird und einen Baum aus verschachtelten, wiederverwendbaren Komponenten enthält. Beispielsweise sieht der Abhängigkeitsbaum einer Todo App wie folgt aus:

```
Root Instanz
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

Später reden wir noch genauer über die [Syntax der Komponenten](components.html). Zurzeit musst du nur wissen, dass alle Vue Komponenten auch Vue Instanzen sind und daher das gleiche Optionsobjekt akzeptieren (außer einiger Root-spezifischen Optionen).

## Daten und Methoden

Wenn eine Vue Instanz erstellt wird, lädt sie alle Daten, die sie in ihrem `data` Objekt finden kann, in Vue's **reactivity system**. Wenn sich die Werte der Daten verändern, "reagiert" die View darauf, und updatet sich den Werten entsprechend.

``` js
// Unser "data" Objekt
var data = { a: 1 }

// Dieses Objekt wird nun der Vue Instanz hinzugefügt
var vm = new Vue({
  data: data
})

// Beide referenzieren das gleiche Objekt
vm.a === data.a // => true

// Wenn in einer Instanz ein Wert verändert wird
// spiegelt sich das auch im Quellobjekt wieder
vm.a = 2
data.a // => 2

// ... das gleiche gilt auch rückläufig
data.a = 3
vm.a // => 3
```

Wenn sich diese Daten ändern, wird sich die View neu rendern. Wichtig ist zu wissen, dass die Daten in `data` nur **reactive** sind, wenn sie bereits bei der Erstellung der Instanz existierten. Das heißt, wenn du später Werte hinzufügst, wie:

``` js
vm.b = 'hi'
```

Werden Veränderungen in `b` kein View Update auslösen. Wenn du weißt, dass du später ein weiteres Datum benötigst, es aber zum Zeitpunkt der Instanziierung nicht vorhanden ist, musst du ihm einen initialen Wert zuweisen. Beispielsweise:

``` js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

Neben den Daten, stellt die Vue Instanz eine Hand voll Instanzwerte und -methoden zur Verfügung. Diese beginnen mit einem `$` um sie von Nutzerdaten unterscheiden zu können. Beispielsweise:

``` js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch ist eine Instanzmethode
vm.$watch('a', function (newValue, oldValue) {
  // Dieser Callback wird aufgerufen, wenn sich `vm.a` verändert
})
```

Später kannst du die [API Referenz](../api/#Instance-Properties) aufsuchen, um eine vollständige Liste an Instanzwerten und -methoden abrufen zu können.

## Instance Lifecycle Hooks

Jede Vue Instanz durchläuft mehrere Initialisierungsschirtte wenn sie erstellt wird. Beispielsweise muss sie Datenobservierung einrichten, das Template compilieren, die Instanz ins DOM einhängen und Updates im DOM verarbeiten. Nebenbei ruft sie Funktionen mit dem Namen **lifecycle hooks** auf, welche dem Nutzer die Möglichkeit geben, ihren Code an bestimmten Stellen auszuführen.

Beispielsweise kann der [`created`](../api/#created) Hook genutzt werden, um Code auszuführen, nachdem die Instanz erstellt wurde:

``` js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` zeigt auf die vm Instanz
    console.log('a is: ' + this.a)
  }
})
// => "a ist: 1"
```

Es gibt viele weitere Hooks die an bestimmten Stellen im Lebenszyklus der Instanz aufgerufen werden, so gibt es beispielsweise [`mounted`](../api/#mounted), [`updated`](../api/#updated), und [`destroyed`](../api/#destroyed). Alle lifecycle hooks werden in ihrem `this` Kontext aufgerufen, welcher auf die Vue Instanz zeigt, welche diese Methode ausführt.

<p class="tip">Nutze keine [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in einer options property oder in einem Callback, beispielsweise `created: () => console.log(this.a)` oder `vm.$watch('a', newValue => this.myMethod())`. Da arrow functions an ihr Elternobjekt gebunden sind, zeigt `this` oft nicht auf die erwartete Vue Instanz, was in Fehler wie `Uncaught TypeError: Cannot read property of undefined` oder `Uncaught TypeError: this.myMethod is not a function` resultiert.</p>


## Lebenszyklusdiagramm

Unten siehst du ein Diagramm mit dem instance lifecycle von Vue. Du musst nicht alles davon verstehen, aber im Laufe der Zeit eignet sich das Diagramm als eine nützliche Referenz.

![The Vue Instance Lifecycle](/images/lifecycle.png)
