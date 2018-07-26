<p align="center">
  <strong>Repository for my React NYC talk on scrollytelling.</strong>
</p>

Scrollytelling is widely used by graphics reporters, data scientists, and front-end developers, yet it can be complicated to implement, especially in React. In this talk I will give an overview of this storytelling technique, its applications, and some best practices. I will also demonstrate how to create a crude visual essay in React with a simple scrollytelling library I wrote.

Component library: [react-scrollama](https://github.com/jsonkao/react-scrollama)

## Outline

Introduce people to scrollytelling, and explain the implementation in React, subsequently introducing people the Interaction Observer and sticky positioning for further applications.

### Intro
- "Mainly on Scrollytelling"
- Hello everybody, hopefully you're having a good night, I'll try not to put you to sleep.
- This talk is going to mainly be on scrollytelling, my name is Jason Kao, let's get right into it.

#### About me
So a little about me...
- freelance developer at a Canadian tech consultancy doing React & React Native
- incoming Freshman @ Columbia studying CS, political science, stats

### What is scrollytelling?
- [short bio](https://pudding.cool/process/how-to-implement-scrollytelling/)
- Scrollytelling is a storytelling technique in which content “unfolds” as the user scrolls.
- [example: Snow Fall](http://www.nytimes.com/projects/2012/snow-fall/index.html#/?part=descent-begins)
  - It's about an avalanche...that's all I can tell you...but it looks really cool.

Let's get into the purpose of scrollytelling, and why you should consider using it for your next storytelling interactive. If you're not making a storytelling interactive anytime soon, don't worry, this talk is only mainly on storytelling, I promise there'll be other useful stuff.

#### Why use scrollytelling?
- To answer this question, I'm gonna use Bran Ferren's quote, "The Web is a Storytelling Medium". I really dig this.
- I just graduated high school, so most of my friends, when I tell them I do web development, they say, ew, I hate JS, why are there 3 equal signs, web development is so boring. Now that's like an electrical engineer thinking poetry is the cinquain his 2nd grade son wrote.
- No, interactive web development is not just flashy animations. On the web, we have several different types of content, like text, and images and video, and the web brings them together in a unique medium. Scrolling is just an intuitive way to do that.

#### Sticky Graphic Pattern
- Now, one of the most popular scrollytelling patterns is the sticky graphic pattern, this is where a graphic scrolls into view, "sticks" for a duration of steps, then unsticks when the steps conclude.

I think one of the best examples of the sticky graphic pattern is this article [navigate to article], gender parity in the House of Representatives, by The Pudding. I'm not gonna go through all of it, I just want to show off how sexy it is.
- [example: Women in Congress](https://pudding.cool/2018/07/women-in-congress/)
  - So we scroll into this graph, it sticks onto the screen, and as I scroll, different blocks of text go by, we'll call those steps. 
  - As each step enters our viewport, it triggers a corresponding transition in the graphic.
  - For example, [].
  - As you scroll, the steps take you through the construction of a somewhat complicated graph, basically leading you through this data analysis.
  
Now, I know you guys and girls just wanna see some code, so how do we implement something like this in React?

#### How to implement scrollytelling in React: Implementation details
- How do we do this in React?
- I'm gonna split this into two sections, (1) step triggers, (2) sticky graphic

### Step triggers

#### The Olden Days of Scroll Triggers
- Lots of:
  - `window.addEventListener('scroll', onScroll, true)`
  - `el.getBoundingClientRect()`
  - `window.requestAnimationFrame`
- _Background image of the above 3 phrases scattered & multiplied 1000x behind "The Olden Days"_
- Back in the olden days of 2015, implementing intersection detection with the viewport and its elements involved lots of event handlers and loops calling methods like `getBoundingClientRect()` to build up the needed information for every element affected. Since all your code ran on the main thread, there were bound to be performance problems, especially in a big article like women in the house of representatives.

#### What is the Intersection Observer?
- This is where the intersection observer comes in. This API lets you configure a callback whenever an element (referred to as the target) intersects the viewport or a specified ancestor element (the root). And there's no need to do anything on the main thread to watch for this type of element intersection.

#### Creating an Intersection Observer
- I think everything's best done by example, so let's create an intersection observer really quick. You have your callback, config, and observe call.
```js
const options = {
  root: null,
  rootMargin: '0 0 -20% 0',
  threshold: 0.5,
};

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
We're defining a root, a root margin, and a threshold.

#### `root` and `threshold`
![alt](https://i.imgur.com/0EmmrRs.png)

- As I said before, the root is the element used as the viewport to check the visibility of the target. It defaults to the browser viewport, so we left it as null, but you can put any of the target's ancestor elements as the root.
- The target is the blue rectangle, it's the actual element we want to detect intersection on. So just imagine we're scrolling down the page and the target is coming up into the viepwort.
- The threshold is the percentage of the target's visibility in the root. When we set the threshold to 0.5, we're telling the intersection observer to call our callback when 50% of the target is visible.

#### `rootMargin`
![alt](https://i.imgur.com/17W7TPA.png)

- The root margin can be used to grow or shrink each side of the root element's bounding box before computing intersections. Here, we use a negative bottom margin to shrink the viewport's bounding box.
- Even though half of the target is already in our viewport, it won't call the callback function until half of it is above the newly defined bounding box, which is the yellow box.

#### watch
After we've configured our intersection observer, we give it a target element to watch.
```js
const observer = new IntersectionObserver(callback, options);
observer.observe(targetEl);
```
- Callback whenever target comes into view.

Now that we're familiar with the intersection observer, let's take a look at sticky positioning.

#### Sticky positioning
- [👪 graphic]
- `position: sticky` is the love child of `position: relative` and `position: fixed`. Sticky elements are static in the document until they cross a certain threshold, at which point they become fixed. A threshold is defined by any directional declaration such as: `top: 0;`. In that case, the element would become fixed once we reach the top edge of its parent.

### React Scrollama
So this is the basic tech behind scrollytelling. Using this, you can easily create a scrollytelling interactive in React. So that's what I did yesterday, you can check it out at https://github.com/jsonkao/react-scrollama, it's named scrollama because it's based off of Russel Goldenberg's Scrollama library.
- I'll do a quick [demo](https://jsonkao.github.io/react-scrollama/)

#### Outro
- jasonkao.me
- @jsonkao 

## Good Examples

[NYT Trump Climate Change](https://www.nytimes.com/interactive/2016/12/08/us/trump-climate-change.html)
