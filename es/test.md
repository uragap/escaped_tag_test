# Reconstruyéndolo con &lt;amp-script&gt;

Para usar `<amp-script>` , necesitamos importar su propio JavaScript. Abra `index.html` y agregue lo siguiente al `<head>` .

```html
<head>
 ...
  <script async custom-element="amp-script" src="https://cdn.ampproject.org/v0/amp-script-0.1.js"></script>
  ...
</head>
```

`<amp-script>` nos permite escribir nuestro propio JavaScript en línea o en un archivo externo. En este ejercicio, escribiremos suficiente código para ameritar un archivo separado. Cree un nuevo directorio llamado `js` y agréguele un nuevo archivo llamado `validate.js` .

{{image ('/ static / img / docs / tutorials / custom-javascript-tutorial /rative-url-error.png', 600, 177, layout = 'intrinsic', alt = 'Error sobre URL relativa', align = 'centro')}}

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
