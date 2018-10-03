class: center, middle, inverse, hero

# A tasty 🥓 15-min intro to Functional Reactive Programming

---

class: center, middle, inverse

# "Everyone sucks at managing state"

http://www.sprynthesis.com/page2

???

Turns out state is really really hard to get right. Every time you need to restart your computer or an app is probably because state went bad. You fix it by blowing away the old state and starting fresh.

---

class: center, middle, inverse

<img src="https://media.giphy.com/media/F7yLXA5fJ5sLC/giphy.gif" class="full-width" alt="State is hard!" />

???

In fact state is so hard we had to build debuggers: that’s right, people had to build programs to help other programmers figure out the state in their programs.

---

class: center, middle, inverse

FRP can be described as a way to build programs by modelling state as values which react to events over time.

---

class: center, middle, inverse

In FRP we don't store our state in variables, rather in immutable containers called event streams.

---

class: center, middle, inverse

Instead of describing execution steps that the computer must take, we describe the relationship between values, and the FP engine automatically derives execution from that.

---

class: center, middle, inverse

This is a huge win as thinking in terms of dependencies is much simpler than thinking in terms of sequences.

---

class: center, middle, inverse

# Live Coding Demo 😱

---

class: center, middle, inverse

# Annotated Source

https://github.com/fknussel/bacon-dnd

---

class: center, middle, inverse, hero, contact-details

## .secondary[Slides:] https://talks.fknussel.com/frp-intro

## .secondary[Email:] fknussel@gmail.com

## .secondary[Twitter/GitHub:] @fknussel
