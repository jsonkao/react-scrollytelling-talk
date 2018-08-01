# ReactNYC: HyHNuVaZJ

Tip sheet for my talk at [ReactNYC: HyHNuVaZ](https://www.meetup.com/ReactNYC/events/251214926/). Watch the full talk [here](https://youtu.be/yf5wT2fRgEM?t=1h17m21s).

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
- Scrollytelling is a storytelling technique where content “unfolds” as the user scrolls.
- Snow Fall

Let's get into why scrollytelling?, and why you should consider using it for your next storytelling interactive.

#### Why use scrollytelling?
- To answer this question, I'm gonna use Bran Ferren's quote, "The Web is a Storytelling Medium". I really dig this.
- I just graduated high school, so most of my friends, when I tell them I do web development, they say, ew, I hate JS, why are there 3 equal signs, web development is so boring.
- Obviously, I hope most of the people in this room don't have that view, web development is art, really, it can be beautiful, it can be thoughtful.
- And, interactive web development is not just flashy animations. On the web, we have several different types of content, like text, and images and video, and the web brings them together in a unique medium. Scrolling is just an intuitive way to do that.

#### Sticky Graphic Pattern
- Now, one of the most popular scrollytelling patterns is the sticky graphic pattern, this is where a graphic scrolls into view, "sticks" for a duration of steps, then unsticks when the steps conclude.

I think one of the best examples of the sticky graphic pattern is this article [navigate to article], gender parity in the House of Representatives, by The Pudding. I'm not gonna go through all of it, I just want to show off how sexy it is.
  - So we scroll into this graph, it sticks onto the screen, and as I scroll, different blocks of text go by, we'll call those steps. 
  - As each step enters our viewport, it triggers a corresponding transition in the graphic.
  - As you scroll, the steps take you through the construction of a somewhat complicated graph, basically leading you through this data analysis.
  
Now, I know you guys just wanna see some code, so how do we implement something like this in React?

#### How to implement scrollytelling in React: Implementation details
- How do we do this in React?
- I'm gonna split this into two sections, (1) step triggers, (2) sticky graphic

### Step triggers

#### The Olden Days of Scroll Triggers
- Back in the olden days of 2015, implementing intersection detection with the viewport and its elements involved lots of event handlers and loops calling methods like `getBoundingClientRect()` to build up the needed information for every element affected. Since all your code ran on the main thread, there were bound to be performance problems, especially in a big article like women in the house of representatives.

#### What is the Intersection Observer?
- This is where the intersection observer comes in. This API lets you configure a callback whenever an element (referred to as the target) intersects the viewport or a specified ancestor element (the root). And there's no need to do anything on the main thread to watch for this type of element intersection.

#### Creating an Intersection Observer
- I think everything's best done by example, so let's create an intersection observer really quick. You have your callback, config, and observe call.

### First, the options
We're defining a root, a root margin, and a threshold.

#### `root` and `threshold`
- As I said before, the root is the element used as the viewport to check the visibility of the target. It defaults to the browser viewport, so we left it as null, but you can put any of the target's ancestor elements as the root.
- The target is the blue rectangle, it's the actual element we want to detect intersection on. So just imagine we're scrolling down the page and the target is coming up into the viepwort.
- The threshold is the percentage of the target's visibility in the root. When we set the threshold to 0.5, we're telling the intersection observer to call our callback when 50% of the target is visible.

#### `rootMargin`
- The root margin can be used to grow or shrink each side of the root element's bounding box before computing intersections. Here, we use a negative bottom margin to shrink the viewport's bounding box.
- Even though half of the target is already in our viewport, it won't call the callback function until half of it is above the newly defined bounding box, which is the yellow box.

#### watch
After we've configured our intersection observer, we give it a target element to watch.
- Callback whenever target comes into view.

Now that we're familiar with the intersection observer, let's take a look at sticky positioning.

#### Sticky positioning
- `position: sticky` is the love child of `position: relative` and `position: fixed`. Sticky elements are static in the document until they cross a certain threshold, at which point they become fixed. A threshold is defined by any directional declaration such as: `top: 0;`. In that case, the element would become fixed once we reach the top edge of its parent.

### React Scrollama
So this is the basic tech behind scrollytelling. Using this, you can easily create a scrollytelling interactive in React. So that's what I did yesterday, you can check it out at https://github.com/jsonkao/react-scrollama, it's named scrollama because it's based off of Russel Goldenberg's Scrollama library.
- I'll do a quick [demo](https://jsonkao.github.io/react-scrollama/)

#### Outro
- jasonkao.me
- @jsonkao 
