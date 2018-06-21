@title[observable]

### Observables.

Observables are **functions** that tie an observer to a producer. That’s it.

---

@title[producers]

### Producers.

A producer is the source of values for your observable.
It could be a web socket, it could be DOM events, it could be an iterator, or something looping over an array.
Basically, it’s anything you’re using to get values and pass them to `observer.next(value)`.

---

@title[cold]

### Cold Observables.

It is when your observable creates the producer.

```
const source = new Observable((observer) => {
  const socket = new WebSocket('ws://someurl');
  socket.addEventListener('message', (e) => observer.next(e));
  return () => socket.close();
});

```

---

@title[hot]

### Hot Observables.

An observable is “hot” if its underlying producer is either created or activated outside of subscription.

```
const socket = new WebSocket('ws://someurl');
const source = new Observable((observer) => {
  socket.addEventListener('message', (e) => observer.next(e));
});

```

---

@title[why_make_it_hot]

### Why make an Observable hot?

There is one big problem with cold observables:


If you’re subscribing to an observable more than once that is creating some scarce resource, like a web socket connection, you don’t want to create that web socket connection over and over.

---

@title[how_to_make_it_hot]

### How to make an Observable hot?

There are several functions in RxJs that you can use to turn a cold observsable into a hot observable.

The one we have been using is `share` that makes a hot, refCounted observable that can be retried on failure, or repeated on success.

---

@title[how_to_make_it_hot_2]

```
const votes = http.get('https://voting.com/api/votes');

votes.subscribe((votes) => console.debug('votes', votes));
votes.subscribe((votes) => refreshDOM(votes));

// Will trigger 2 GET requests

```

---

@title[how_to_make_it_hot_3]

```
const votes = http.get('https://voting.com/api/votes').share(); // <--- We are sharing o multicasting the source.

votes.subscribe((votes) => console.debug('votes', votes));
votes.subscribe((votes) => refreshDOM(votes));

// Will trigger 1 GET requests

```
