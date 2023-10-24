# CSS Box Model

## Learning Objectives

By the end of this lesson, you should be able to:

- Describe the CSS Box Model.
- Differentiate between display property values and how they may affect the box model.
- Use the position property to break the normal flow of the page.

---

## Guiding questions

- What is the box model?

- What parts make up the box model?

- What are some different values for the `display` property?

- What are the default `display` values for the following common elements?

- `p`
- `img`
- `button`

- What is the effect of the CSS declaration `margin: 0 auto;`?

- Describe how the following CSS would affect an HTML page using technical language. Feel free to look up the properties and values.

```html
<section>
  <article>
    <p>Very informative. So article</p>
    <p><a>here</a> click</p>
  </article>

  <article>
    <img src="./doge.png" alt="An image of the 'Doge' meme" />
    <p>What ever happened to <span>Doge</span>?</p>
  </article>
</section>
```

```css
section {
  width: 80%;
  margin: 16px auto;
}
```

- Using the same HTML above, describe how the following CSS would modify the HTML. Then, answer the questions that follow.

```css
article {
  display: inline-block;
  width: 50%;
  padding: 10px;
  margin: 4px;
  border: 5px solid purple;
}

p span {
  color: peachpuff;
}
```

- Do the articles appear next to each other, or does one appear below the other? Why?

- What would one article's total width be if the `section` width is 1000px?

- How could you constrain the width of the articles to be precisely `500px`, regardless of margin or padding properties?

- By default, does the anchor tag (i.e., `a`) have a `display` value of `inline`, `inline-block`, or `block`? Describe how an anchor tag would look if the `display` value was set to any previously mentioned values.

- If you wanted to make the anchor tag appear as a big button, could you style it with CSS, or would you need to use a `button` tag?
