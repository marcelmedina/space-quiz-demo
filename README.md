# 🚀 Space Exploration Quiz

A 10-question space exploration quiz in a single, self-contained `index.html`. It has no build step, no server and no dependencies.

## Features

- 10 multiple-choice questions about missions, planets and astronauts
- Mission-control status bar with question counter, score readout and a status light
- Progress bar that fills as you answer
- Animated feedback: the correct answer turns green with a pop, a wrong answer turns red and shakes
- Results screen with an emoji and title that change with your score, plus "Play again"
- Light and dark themes that follow your system setting (`prefers-color-scheme`)
- Keyboard play: answer with <kbd>A</kbd>–<kbd>D</kbd> or <kbd>1</kbd>–<kbd>4</kbd>
- Accessible: plain-language screen-reader text, live updates, and animations turned off when your system is set to reduce motion

## Run it

Open `index.html` in any modern browser. That's it.

## Customize

Edit the `QUESTIONS` array in `index.html`. Each entry is:

```js
{ q: "Question text", a: ["Choice A", "Choice B", "Choice C", "Choice D"], c: 1 } // c = index of the correct choice
```

The results thresholds use the fraction of correct answers, so they still work if you change the number of questions.
