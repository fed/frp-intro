# Speaker Notes

> Credits: based on Blake Haswell’s talk on FRP

Turns out state is really really hard to get right. Everytime you need to restart your computer or an app is probably because state went bad. You fix it by blowing away the old state and starting fresh. In fact state is so hard we had to build debuggers: yeah that’s right, ppl had to build programs to help other programmers figure out the state in their programs.

"I understand WHY it crashed. I just don’t understand how the user could have possibly gotten it in that state."
-- Programmer adds another state check.

FRP can be described as a way to build reactive programs by modelling state as values which react to events over time.

Today in JavaScript land there's an entire ecosystem of libraries built upon these ideas, and the community has mostly settled on a combination of React and Redux. Redux is a great library but lacks some of the powers of FRP libs like Reactive Extensions (RxJS) and BaconJS. So tonight I wanna give you a small taste of what those libraries are.

In FRP we don't store our state in variables, rather in immutable containers called event streams.

Instead of describing execution steps that the computer must take, we describe the relationship between values, and the FP engine automatically derives execution from that. This is a huge win as thinking in terms of dependencies is much simpler than thinking in terms of sequences.

```javascript
const Bacon = window.Bacon;

const pointerDownStream = Bacon.fromEvent(window, 'pointerdown');
const pointerMoveStream = Bacon.fromEvent(window, 'pointermove');
const pointerUpStream = Bacon.fromEvent(window, 'pointerup');

pointerDownStream.log();
```

If you think of a drag and drop implementation, there's really three inputs that you care about: `pointerdown`, `pointermove` and `pointerup`.

So the first thing we are gonna do is create streams representing these events.

`Bacon.fromEvent` takes in an observable, in this case `window` and an event name.

Every time the `pointerdown` event is fired on the window, that event will go into the stream.

```javascript
const Bacon = window.Bacon;

const pointerDownStream = Bacon.fromEvent(window, 'pointerdown');
const pointerMoveStream = Bacon.fromEvent(window, 'pointermove');
const pointerUpStream = Bacon.fromEvent(window, 'pointerup');

const cardOffsetStream = pointerDownStream
 .filter(pointerDownEvent => pointerDownEvent.target.matches('.board__card'))
 .log();
```

What we really want is our `pointerdown` events where we moused down over a card.

```javascript
const Bacon = window.Bacon;

const pointerDownStream = Bacon.fromEvent(window, 'pointerdown');
const pointerMoveStream = Bacon.fromEvent(window, 'pointermove');
const pointerUpStream = Bacon.fromEvent(window, 'pointerup');

const cardOffsetStream = pointerDownStream
 .filter(pointerDownEvent => pointerDownEvent.target.matches('.board__card'))
 .flatMap(pointerDownEvent => pointerMoveStream.takeUntil(pointerUpStream))
 .log();
```

Once we have the `pointerdown` events on cards, we want to map that to a stream of `pointermove` events.

And we wanna take values from that `pointermove` stream until the next `pointerup` event.

What `flatMap` does is it takes a function whose argument is the event from the stream you are mapping from, in this case it's a `pointerdown` event, and what it needs to return is a stream, in this case we'll be returning the `pointermove` event.

But we don't want all the `pointermove` events, we just wanna take them until the next `pointerup` event.

```javascript
const Bacon = window.Bacon;

const pointerDownStream = Bacon.fromEvent(window, 'pointerdown');
const pointerMoveStream = Bacon.fromEvent(window, 'pointermove');
const pointerUpStream = Bacon.fromEvent(window, 'pointerup');

const cardOffsetStream = pointerDownStream
 .filter(pointerDownEvent => pointerDownEvent.target.matches('.board__card'))
 .flatMap(pointerDownEvent => {
   const cardId = event.target.dataset.id;

   return pointerMoveStream.takeUntil(pointerUpStream).map(pointerMoveEvent => ({
     cardId,
     offsetX: pointerMoveEvent.clientX - pointerDownEvent.clientX,
     offsetY: pointerMoveEvent.clientY - pointerDownEvent.clientY
   }));
 })
 .log();
```

What we really wanna know is which issue is being moved.

Here we get the card that we moused down on.

We map it into a stream of objects that contain a card ID.

We also need to figure out how far we've moved it.
