<p align="center">
  <strong>Repository for my React NYC talk on scrollytelling.</strong>
</p>

Scrollytelling is widely used by graphics reporters, data scientists, and front-end developers, yet it can be complicated to implement, especially in React. In this talk I will give an overview of this storytelling technique, its applications, and some best practices. I will also demonstrate how to create a crude visual essay in React with a simple scrollytelling library I wrote.

Component library: [react-scrollama](https://github.com/jsonkao/react-scrollama)

## Outline

Introduce people to scrollytelling, and explain the implementation in React, subsequently introducing people the Interaction Observer and sticky positioning for further applications.

#### Intro
- "Mainly Scrollytelling"

#### About me
- freelance developer at a Canadian tech consultancy doing React Native
- incoming Freshman @ Columbia studying CS, political science, stats

### What is scrollytelling?
- [short bio](https://pudding.cool/process/how-to-implement-scrollytelling/)
- Scrollytelling is a storytelling technique in which content “unfolds” as the user scrolls.
- [a few examples]

#### Why use scrollytelling?

#### How to implement scrollytelling in React: Implementation details
> One of the biggest implementation pains with scrollytelling is the sticky graphic pattern, whereby the graphic scrolls into view, becomes “stuck” for a duration of steps, then exits and “unsticks” when the steps conclude.

### Why the Intersection Observer?

#### The Olden Days
- Lots of:
  - `window.addEventListener('scroll', onScroll, true)`
  - `el.getBoundingClientRect()`
  - `window.requestAnimationFrame`
- _Background image of the above 3 phrases scattered & multiplied 1000x behind "The Olden Days"_
- Implementing intersection detection in the olden days involved lots of event handlerse and loops calling methods like `getBoundingClientRect()` to build up the needed information for every element affected. Since all this code runs on the main thread, even one of these can cause performance problems. When a site is loaded with these tests, things can get pretty ugly.

#### What is the Intersection Observer?
- The Intersection Observer API lets you configure a callback whenever a target element intersects the viewport or a specified element (the root). There's now no need for doing anything on the main thread to watch for this type of element intersection.

#### Creating an Intersection Observer
```js
const options = {
  root: null,
  rootMargin: '0 0 -20% 0',
  threshold: 0.5,
}

const observer = new IntersectionObserver(callback, options);
```

#### `root` and `threshold`
![alt](https://i.imgur.com/0EmmrRs.png)

- Root is the element used as the viewport to check the visibility of the target. It defaults to the browser viewport, so we'll leave it as null.
- The target is the blue rectangle, we can imagine we're scrolling down the page and therefore the target is coming up into the viepwort.
- The threshold is the percentage of the target's visibility in the root. When we set the threshold to 0.5, we told the intersection observer to call our callback when 50% of the target was visible.

#### `rootMargin`
![alt](https://i.imgur.com/rMrEcHT.png)

- The root margin can be used to grow or shrink each side of the root element's bounding box before computing intersections. Here, we use a negative bottom margin to shrink the viewport's box.

#### Sticky positioning
- [what is sticky?](https://pudding.cool/process/scrollytelling-sticky/)
- [live coding]
- The shameless love child 👪 of relative and fixed

#### Outro
- jasonkao.me
- @jsonkao 

## Good Examples

[NYT Trump Climate Change](https://www.nytimes.com/interactive/2016/12/08/us/trump-climate-change.html)
