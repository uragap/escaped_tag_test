Ahora, usemos `<amp-script>` para crear una experiencia más fácil de usar.

# Reconstruyéndolo con &lt; amp-script &gt;

Para usar `<amp-script>` , necesitamos importar su propio JavaScript. Abra `index.html` y agregue lo siguiente al `<head>` .

## Configurar una historia web para anuncios

Web Stories no admite.

[sourcecode:html]
&lt;amp-story&gt;
  &lt;amp-story-auto-ads&gt;
    &lt;script type="application/json"&gt;
      {
        "ad-attributes": {
          // ad server configuration
        }
      }
    &lt;/script&gt;
  &lt;/amp-story-auto-ads&gt;
  &lt;amp-story-page&gt;
  ...
&lt;/amp-story&gt;
[/sourcecode]

A diferencia de lo normal.
