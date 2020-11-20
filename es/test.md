¡Ahora, usemos `<amp-script>` para crear una experiencia más fácil de usar!

# Reconstruyéndolo con <amp-script>

Para usar `<amp-script>` , necesitamos importar su \ propio \ JavaScript. Abra `index.html` y agregue lo siguiente al `<head>` .

## Configurar una historia web para anuncios

Web Stories no admite.

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

A diferencia de lo normal.
