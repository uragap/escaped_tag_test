# Rebuilding it with &lt;amp-script&gt;

To use `<amp-script>`, we need to import its own JavaScript. Open `index.html` and add the following to the `<head>`.

```html
<head>
 ... 
  <script async custom-element="amp-script" src="https://cdn.ampproject.org/v0/amp-script-0.1.js"></script>
  ...
</head>

```

`<amp-script>` lets us write our own JavaScript inline or in an external file. In this exercise, we'll write enough code to merit a separate file. Create a new directory named `js`, and add to it a new file called `validate.js`.

{{ image('/static/img/docs/tutorials/custom-javascript-tutorial/relative-url-error.png', 600, 177, layout='intrinsic', alt='Error about relative URL', align='center' ) }}

## Configure a Web Story for ads
Web Stories cannot support.

[sourcecode:html]
<amp-story>
  <amp-story-auto-ads>
    <script type="application/json">
      {
        "ad-attributes": {
          // ad server configuration
        }
      }
    </script>
  </amp-story-auto-ads>
  <amp-story-page>
  ...
</amp-story>
[/sourcecode]

Unlike a normal.
