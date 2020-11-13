Now, let's use `<amp-script>` to build a more user-friendly experience!

# Rebuilding it with &lt;amp-script&gt;

To use `<amp-script>`, we need to import its own JavaScript. Open `index.html` and add the following to the `<head>`.

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
