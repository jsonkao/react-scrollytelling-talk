<p align="center">
  <strong>Repository for my React NYC talk on scrollytelling.</strong>
</p>

Scrollytelling is widely used by graphics reporters, data scientists, and front-end developers, yet it can be complicated to implement, especially in React. In this talk I will give an overview of this storytelling technique, its applications, and some best practices. I will also demonstrate how to create a crude visual essay in React with a simple scrollytelling library I wrote.

Component library: [react-scrollama](https://github.com/jsonkao/react-scrollama)

## Outline

Introduce people to scrollytelling, and explain the implementation in React, subsequently introducing people the Interaction Observer and sticky positioning for further applications.

### Intro
- "Mainly on Scrollytelling"

#### About me
- freelance developer at a Canadian tech consultancy doing React Native
- incoming Freshman @ Columbia studying CS, political science, stats

### What is scrollytelling?
- [short bio](https://pudding.cool/process/how-to-implement-scrollytelling/)
- Scrollytelling is a storytelling technique in which content “unfolds” as the user scrolls.
- [a few examples]

Let's get into the purpose of scrollytelling, and it's not just for interactive storytelling with flashy animations.

#### Why use scrollytelling?
- "The Web is a Storytelling Medium" —Bran Ferren, I really dig this.
- I just graduated high school, so most of my friends, when I tell them I do web development, they say, ew, I hate CSS/JS, web development is so boring, there's no interesting problem solving in front-end. Now that's like an electrical engineer thinking poetry is the cinquain his elementary 1st grade daughter wrote.
- We have these different types of content, like text, and images and video, and the web brings them together in a unique medium, and scrolling is just an intuitive way to do that.

#### Sticky Graphic Pattern
- Now, one of the most popular scrollytelling patterns is the sticky graphic pattern, in which a graphic scrolls into view, "sticks" for a duration of steps, then unsticks when the steps conclude.
- [example: Snow Fall](http://www.nytimes.com/projects/2012/snow-fall/index.html#/?part=descent-begins)
- [example: Women in Congress](https://pudding.cool/2018/07/women-in-congress/)

#### How to implement scrollytelling in React: Implementation details
- How do we do this in React?
- (1) step triggers, (2) sticky graphic

### Step triggers: the Intersection Observer

#### The Olden Days of Scroll Triggers
- Lots of:
  - `window.addEventListener('scroll', onScroll, true)`
  - `el.getBoundingClientRect()`
  - `window.requestAnimationFrame`
- _Background image of the above 3 phrases scattered & multiplied 1000x behind "The Olden Days"_
- Implementing intersection detection in the olden days involved lots of event handlers and loops calling methods like `getBoundingClientRect()` to build up the needed information for every element affected. Since all this code runs on the main thread, even one of these can cause performance problems.

#### What is the Intersection Observer?
- The Intersection Observer API lets you configure a callback whenever an element (referred to as the target) intersects the viewport or a specified element (the root). And there's no need to do anything on the main thread to watch for this type of element intersection.

#### Creating an Intersection Observer
```js
const options = {
  root: null,
  rootMargin: '0 0 -20% 0',
  threshold: 0.5,
}

const observer = new IntersectionObserver(callback, options);
observer.observe(targetEl);
```

### First, the options
```js
const options = {
  root: null,
  rootMargin: '0 0 -20% 0',
  threshold: 0.5,
}
```
#### `root` and `threshold`
![alt](https://i.imgur.com/0EmmrRs.png)

- Root is the element used as the viewport to check the visibility of the target. It defaults to the browser viewport, so we'll leave it as null, but you can put any ancestor element to the target.
- The target is the blue rectangle, we can imagine we're scrolling down the page and therefore the target is coming up into the viepwort.
- The threshold is the percentage of the target's visibility in the root. When we set the threshold to 0.5, we're telling the intersection observer to call our callback when 50% of the target is visible.

#### `rootMargin`
![alt](https://i.imgur.com/rMrEcHT.png)

- The root margin can be used to grow or shrink each side of the root element's bounding box before computing intersections. Here, we use a negative bottom margin to shrink the viewport's box.
- Even though half of the target is already in our viewport, it won't call the callback function until half of it is above the newly defined bounding box.

#### watch
After we've configured our intersection observer, we give it a target element to watch.
```js
const observer = new IntersectionObserver(callback, options);
observer.observe(targetEl);
```
- Callback whenever target comes into view.

#### Sticky positioning
- [what is sticky?](https://pudding.cool/process/scrollytelling-sticky/)
- [live coding]
- The shameless love child 👪 of relative and fixed

### React Scrollama
- [demo](https://jsonkao.github.io/react-scrollama/)

#### Outro
- jasonkao.me
- @jsonkao 

## Good Examples

[NYT Trump Climate Change](https://www.nytimes.com/interactive/2016/12/08/us/trump-climate-change.html)
