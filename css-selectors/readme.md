# CSS Selectors

While it is possible to only use HTML elements as selectors, you'll find that you need more than simple selectors to make a great design when building complex web pages. For example, you may have two forms on the page, and you'd like the two forms to look different from each other. CSS allows for various selectors to be used, making it easier to customize all the different parts of your page.

In this lesson, you'll learn different types of selectors when styling your web pages. There are many selectors to learn, and this lesson serves as an introduction to some of the most common that are used. Finally, you'll learn some best practices when writing your CSS so it is maintainable.

## Learning Objectives

By the end of this lesson, you should be able to:

- Describe specificity in regards to how CSS applies styles to particular elements.
- Describe the purpose of the id and class attributes and be able to explain the difference between the two.
- Apply styles to specific elements, classes, or IDs, incorporating descendent selectors.
- Use grouping to apply styles to multiple elements, classes, or IDs.
- Style links by using pseudo-classes.

---

In the code below, you'll notice several sections, two forms, anchor tags in two places, and more.

If you only use the elements as selectors, you don't have a lot of flexibility in how the same elements are styled throughout the page. This lesson will show several ways to target elements within a web page.

<details><summary>Sample index.html</summary>

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="styles.css" />
    <title>Movie Quotes</title>
  </head>
  <body>
    <header>
      <h1>My Favorite Movie Quotes</h1>
      <nav>
        <ul>
          <li><a href="#">home</a></li>
          <li><a href="#">about</a></li>
        </ul>
      </nav>
    </header>
    <main>
      <h2>This year's favorites!</h2>
      <section>
        <h3>Star Wars</h3>
        <ul>
          <li>This is the way.</li>
          <li>May the force be with you.</li>
        </ul>
      </section>
      <section>
        <h3>Jaws</h3>
        <ul>
          <li>You're going to need a bigger boat.</li>
          <li>Just when you thought it was safe to go back in the water.</li>
        </ul>
      </section>
      <section>
        <h3>Wizard of Oz</h3>
        <ul>
          <li>Toto, I've a feeling we're not in Kansas any more.</li>
          <li>There is no place like home.</li>
        </ul>
      </section>
    </main>
    <aside>
      <h2>Add your quote!</h2>
      <form>
        <label for="movie">Movie name</label>
        <input type="text" name="move" id="movie" />
        <label for="quote">Quote</label>
        <input type="text" name="move" id="quote" />
        <input type="submit" />
      </form>
    </aside>
    <footer>
      <h3>Contact me:</h3>
      <section>
        <form>
          <label for="your-name">Your name</label>
          <input type="text" name="your-name" id="your-name" />
          <label for="message">Message</label>
          <input type="text" name="message" id="message" />
          <input type="submit" />
        </form>
      </section>
      <section>
        <h4><a href="http://imdb.com">Go to IMDB</a></h4>
      </section>
    </footer>
  </body>
</html>
```

</details>

## CSS Selector Specificity

Cascading stylesheets follow three rules to apply styles.

Using the following HTML block, let's look at some examples.

```html
<body>
  <h1>Demo</h1>
  <aside>
    <p>x</p>
    <p>y</p>
    <p>z</p>
  </aside>

  <section>
    <p>a</p>
    <p>b</p>
    <p>c</p>
  </section>
</body>
```

If the rule has the same specificity, the last rule is applied. In the following code block, the salmon color will be applied. All `p` elements will have the color salmon.

```css
p {
  color: orchid;
}

p {
  color: salmon;
}
```

If the rule is more targeted, it will get applied. In this case, the body style cascades down. However, the `p` tag style is applied directly to the `p` and does not need to cascade down. Therefore the p tags will have the color of salmon. However, the h1 will still have the color of orchid.

```css
body {
  color: orchid;
}

p {
  color: salmon;
}
```

Declarations have specificity scores to determine which competing rule should be applied. If the declaration has a higher specificity score, it will be applied. Elements have a specificity score of 1 point each.

In the following case, all of the `p` elements inside the section tag will be salmon because the specificity is 2 (using two elements, each element gets 1 point). However, the `p` elements inside the aside will be orchid.

```css
p {
  color: orchid;
}

section p {
  color: salmon;
}
```

The specificity can be calculated by the three types of specificity: `id` (100 points), `class` (10 points), and `element` (1 point).

`id`s are attributes designed only to be used once on a webpage. Their usage should only be for forms or situations where you can make a case for an element being the only one of its type on the page and that there is a need to target it and only it.

`class` is an attribute that can be applied to many elements on a webpage. `class` should be used on similar items. For example, you may use them on all the `sections` inside a `main` tag to distinguish them from `section` tags elsewhere on the web page.

Notice when an element has an `id`, the HTML element does not have a `#`. However, when you select an element by id, you must use the `#` to inform CSS that you are selecting an element by id. The following p element with an id of `my-quote` will be orchid in color.

```html
<p id="my-quote">Wow</p>
```

```css
#my-quote {
  color: orchid;
}

p {
  color: salmon;
}
```

`class` has an in-between score of 10 points. In the following example, the section tag has a class of `my-quotes`. To select it with CSS, you must begin the selector's name with a `.`. However, the `.` is not used when assigning the class name.

```html
<section class="my-quotes">Wow</p>
```

```css
.my-quote {
  color: orchid;
}

section {
  color: salmon;
}
```

[W3 schools has an in-depth write-up](https://www.w3schools.com/css/css_specificity.asp), and you can also find [CSS Specificity Calulators](https://polypane.app/css-specificity-calculator/#selector=section%20%3E%20p) online to get a better understanding.

## Combinations

It is also possible to combine multiple selectors. To do so, write the selectors without including a space. For example, the `p` selector is combined with the class selector `.frankenstein` in the CSS below. The declaration block will only apply to the `p` elements with the `frankenstein` class.

```html
<img
  class="frankenstein"
  src="./images/frankenstein-poster.jpg"
  alt="Image of the Frankenstein movie poster."
/>
<p class="frankenstein movie-quote">"It's alive! It's alive!"</p>
```

```css
p.frankenstein {
  text-decoration: green;
}
```

It is also possible to combine classes. For example, the CSS below will only apply to elements with the `movie-quote` and `frankenstein` classes.

```css
.movie-quote.frankenstein {
  text-transform: uppercase;
}
```

### Descendants

You can create styles for elements inside a "container" element. For example, the following CSS will only apply to the `p` element that is _inside of_ a `div` with a class of `active`. Therefore, it will not apply to the `frankenstein` quote.

```html
<div class="active">
  <p class="jaws movie-quote">"You're gonna need a bigger boat."</p>
</div>
<p class="frankenstein movie-quote">"It's alive! It's alive!"</p>
```

```css
.active p {
  background: black;
  color: white;
}
```

In the case above, the `p` element _does not_ need to be a direct descendent of an element with the `active` class. It can be nested anywhere inside of the `active` element. For example, the CSS rule would still apply if the HTML looked like the below example.

If you want to target direct descendants, you can use the `>` character. Notice if you add the `>` symbol, the `p` tags no longer change. You would need to remove the `div` with the class of `quote` so that the `p` tag is a direct descendent of `active`.

```css
.active > p {
  background: black;
  color: white;
}
```

```html
<div class="active">
  <div class="quote">
    <p class="jaws movie-quote">"You're gonna need a bigger boat."</p>
  </div>
  <p class="attribution">- From the movie "Jaws"</p>
</div>
<p class="frankenstein movie-quote">"It's alive! It's alive!"</p>
```

Using descendent selectors is quite powerful and can be a great tool when organizing your CSS code. However, you should try to use them for small groups of HTML elements, for example, just a form and the elements within it or a nav bar and the elements in it. You shouldn't use it from the body down two or three elements because if you reorganize your code, it will cause your CSS to break. Using descendants from a high-level general component is an example of brittle/fragile code (where a small change breaks the code).

## Grouping

It is also possible to apply the same declaration block to multiple selectors. To do so, you can separate each selector by a comma.

```css
h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: Impact, serif;
}
```

Grouping does not impact specificity and can be used with other selectors, like classes or IDs.

```css
p.movie-quote,
.frankenstein {
  text-decoration: underline;
}
```

## Pseudo-classes

In addition to the classes you define, the browser has special classes, called "pseudo-classes", that it assigns to particular elements. Many of these pseudo-classes relate to the element's state: for example, the element has been clicked or is hovered over by a mouse pointer.

The most common pseudo-classes you will interact with are links, as detailed below.

### Links

Links, or `a` elements, can be styled using regular CSS.

```css
a {
  color: black;
  font-weight: bold;
  text-decoration: none;
}
```

However, links also have different states. For example, a link looks different before, during, and after it's been clicked. It can also look different when hovered on or selected by a keyboard. To modify the CSS for these states, you can use pseudo-classes, typically attached to the `a` element and prefixed with a `:` symbol.

For example, the CSS below will style links so that they are bold and black without an underline before, during, and after they are clicked. However, when hovered over, they will turn pink.

```css
a,
a:active,
a:visited {
  color: black;
  font-weight: bold;
  text-decoration: none;
}

a:hover {
  color: hotpink;
}
```

Pseudo-classes are an interesting topic and can allow for some pretty specific styles. For more information on pseudo-links, you can read the article below.

- [MDN: Styling links](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_text/Styling_links)
