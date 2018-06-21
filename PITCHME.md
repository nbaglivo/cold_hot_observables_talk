@title[observable]

Observables are **functions** that tie an observer to a producer. That’s it.

---

@title[producers]

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
