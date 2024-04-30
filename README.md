# Interesting and Useful CSS Resets

## Adrian Roselli - [Font Size Blog](https://adrianroselli.com/2024/03/the-ultimate-ideal-bestest-base-font-size-that-everyone-is-keeping-a-secret-especially-chet.html)

### Global `font-size`

```css
:root {
  font-size: 100%;
}
```

> This approach has the advantage of always inheriting the user’s preferred font size. The one they choose in their browser or on their system.

### Forms

```css
select, textarea, input, button {
  font: inherit;
}
```

> Add this to your CSS to honor the user’s preferences in forms

### Print

```css
@media print {
  body {
    font-size: 8pt;
  }
}
```

> When it comes to print styles, the text may be too large for your audience (regardless of if or how you set it). If so, you can set the base font in the appropriate point size and all your other relative font sizes will work from that.

### Additional Considerations

User provided "fix" for iOS Safari font issues:

```css
body {
  /* iOS reset to user OS preferences */
  font: -apple-system-body;
}

@supports (font: -apple-system-body) and (not (-webkit-touch-callout: default)) {
  /* Makes sure desktop Safari doesn't pick up broken iOS issues */
  :root {
    font-size: 100%;
  }
}
```

## Adrian Roselli - [Text Boxen Blog](https://adrianroselli.com/2019/09/under-engineered-text-boxen.html)

### Basic Styles

```css
textarea,
input {
  font: inherit;
  letter-spacing: inherit;
  word-spacing: inherit;
}
```

> A more robust style might look like this - this short block of code is deceptively complex, or not...

**[Code Pen Example](https://codepen.io/aardrian/pen/yLBzOEx/)**

An alternative:

```css
textarea,
input:not([type="checkbox"]):not([type="file"]):not([type="image"]):not([type="radio"]):not([type="range"]) {
  font: inherit;
  letter-spacing: inherit;
  word-spacing: inherit;
  /* optional styles */
  border: 0.1em solid;
  padding: 0 0.2em;
}
```

### Focused

```css
textarea:focus,
input:not([type="checkbox"]):not([type="file"]):not([type="image"]):not([type="radio"]):not([type="range"]):focus {
  outline: 0.15em solid #00f;
  box-shadow: 0 0 0.2em #00f;
}
```

> I opt to use an obvious blue outline, with good contrast to white, and set a box shadow as well to make it even more obvious. I tend not to apply this to hover styles.

### Read-Only

```css
textarea[readonly],
input:not([type="checkbox"]):not([type="file"]):not([type="image"]):not([type="radio"]):not([type="range"])[readonly] {
  border-color: rgba(0, 0, 0, 0.42);
  border-left: none;
  border-top: none;
  border-right: none;
}
```

> My first suggestion is to not use the readonly attribute on a field. But if you do, consider a style that is simple and implies that this thing is not like the others.

### Required

```css
textarea[required],
input:not([type="checkbox"]):not([type="file"]):not([type="image"]):not([type="radio"]):not([type="range"])[required] {
  border-left-width: 0.3em;
}
```

> As long as your label indicates a field is required (along with the `required` attribute), you don’t need to style the field any differently. A few years ago, however, I experimented with a visual style to reinforce the label and it tested well with users _for that system_.

### Errored

```css
textarea[aria-invalid="true"],
textarea[aria-invalid="spelling"],
textarea[aria-invalid="grammar"],
input:not([type="checkbox"]):not([type="file"]):not([type="image"]):not([type="radio"]):not([type="range"])[aria-invalid] {
  background: linear-gradient(135deg, rgba(255,0,0,1) 0, rgba(255,0,0,1) .4em, rgba(255,255,255,1) .4em);
}
```

> A red border, alone, will always be insufficient (from both contrast and color-alone WCAG failures) and massive drop shadows can muddy the overall page. In testing with users, too much effort to draw attention to errors creates noise, requiring multiple passes for users to address them all. Instead, I found that an indicator in the corner of the field did the trick.

### Internationalization Styles

```css
*[dir="rtl"] textarea[aria-invalid="true"],
*[dir="rtl"] textarea[aria-invalid="spelling"],
*[dir="rtl"] textarea[aria-invalid="grammar"],
*[dir="rtl"] input:not([type="checkbox"]):not([type="file"]):not([type="image"]):not([type="radio"]):not([type="range"])[aria-invalid] {
  background: linear-gradient(225deg, rgba(255,0,0,1) 0, rgba(255,0,0,1) .4em, rgba(255,255,255,1) .4em);
}

*[dir="rtl"] textarea[required],
*[dir="rtl"] input:not([type="checkbox"]):not([type="file"]):not([type="image"]):not([type="radio"]):not([type="range"])[required] {
  border-left-width: 0.1em;
  border-right-width: 0.3em;
}
```

> Conveniently, since we are using native text fields, we only need to adjust styles we made that lean on expectations from left-to-right languages, namely the required and error styles that position themselves based on where the user starts reading.
