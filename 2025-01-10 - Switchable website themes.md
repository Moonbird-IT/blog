# Switchable website themes (light and dark mode)

As more and more websites and apps offer choosing a light or dark (or even auto-switch based on the user's OS), I also wanted to have a look at how to best integrate this into a web application. Below are my notes about techniques and common problems and how to handle these.

## Implementing

### Checking current user choice

If you want to check if the user is preferring light or dark mode, run the following JS on first page load:

```
if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    // dark mode
}
```
Make sure to save the setting in a cookie/localStorage setting and pass it to the server on reload to prevent flickering (see "Common issues").

### Build on SCSS variables

Instead of creating a set of CSS styles for dark and light, set up light and dark SCSS variable definition files and have one base file using the variables. 

**light theme**

```
$TextColor: #222222;
$BackgroundColor: #FFFFFF;
```

**dark theme**

```
$TextColor: #EEEEFF;
$BackgroundColor: #222222;
```

**page template**

```
body {
  background-color: $BackgroundColor;
  color: $TextColor;
}
```

## Common issues

### Bad contrasts

If you simply switch your colors in your code, readability might not be ideal, so double-check each rule properly. To automatically set a fitting contrast color, have a look at `color-contrast`, which is now [available in most modern browsers](https://caniuse.com/?search=color-contrast).

**color-constrast() example**

```
body {
  color: color-contrast(#004 vs black, white);
}
```
This makes more sense in case colors are stored in variables, e.g. as part of an SCSS file.

### Flickering page
If your web page is always loading a default style on initialization and handling the user's configured theme, you'll run into the situation that the page will flicker on load, e.g. it will be all-white on load and will then switch to dark mode once the JS is executed.

To prevent this behavior, send the preferred theme to the server as part of the request, and then pre-configure the theme to be used based on the submitted value:

```
http://my.cool.site/?theme=dark
```
leads to

```
<html>
...
<body class="dark">...</body>
</html>
```

## References

Most of the ideas originate from the wonderful _"How To Dark Mode and Beyond"_ on the [Syntax.fm podcast](https://syntax.fm/show/690/how-to-dark-mode-and-beyond).

Further listen (still on my to-listen-to list) is last year's episode _"Effortless Light and Dark Mode × Theme Styling"_ on the [Syntax.fm podcast](https://syntax.fm/show/810/effortless-light-and-dark-mode-theme-styling).

Further links:
- How to switch dark/light mode in browser tools [Stack Overflow](https://stackoverflow.com/questions/57606960/how-can-i-emulate-prefers-color-scheme-media-query-in-chrome)