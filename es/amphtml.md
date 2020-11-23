---
"$title": Especificación HTML de AMP
order: '8'
formats:
- sitios web
teaser:
  text: |2-

    AMP HTML es un subconjunto de HTML para crear páginas de contenido, como noticias.
    artículos de una manera que garantice cierto rendimiento básico
    caracteristicas.
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/spec/amp-html-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

AMP HTML es un subconjunto de HTML para la creación de páginas de contenido, como artículos de noticias, de manera que garantice determinadas características de rendimiento de referencia.

Al ser un subconjunto de HTML, impone algunas restricciones al conjunto completo de etiquetas y funcionalidades disponibles a través de HTML, pero no requiere el desarrollo de nuevos motores de renderizado: los agentes de usuario existentes pueden renderizar AMP HTML como todos los demás HTML.

[tip type="read-on"] Si lo que más le interesa es lo que está permitido en AMP y lo que no, mire nuestro [video básico sobre las limitaciones de AMP](https://www.youtube.com/watch?v=Gv8A4CktajQ) . [/tip]

Además, los documentos HTML de AMP se pueden cargar en un servidor web y servir como cualquier otro documento HTML; no es necesaria ninguna configuración especial para el servidor. Sin embargo, también están diseñados para ser servidos opcionalmente a través de sistemas de servicio AMP especializados que procesan documentos AMP. Estos documentos les sirven desde su propio origen y se les permite aplicar transformaciones al documento que proporcionan beneficios de rendimiento adicionales. Una lista incompleta de optimizaciones que podría hacer un sistema de servicio de este tipo es:

- Reemplace las referencias de imagen con imágenes del tamaño de la ventana gráfica del espectador.
- Imágenes en línea que son visibles en la parte superior del pliegue.
- Variables CSS en línea.
- Cargue previamente los componentes extendidos.
- Minimiza HTML y CSS.

HTML de AMP utiliza un conjunto de elementos personalizados alojados y administrados de forma centralizada para implementar funciones avanzadas, como galerías de imágenes que se pueden encontrar en un documento HTML de AMP. Si bien permite diseñar el documento utilizando CSS personalizado, no permite JavaScript escrito por el autor más allá de lo que se proporciona a través de los elementos personalizados para alcanzar sus objetivos de rendimiento.

Al utilizar el formato AMP, los productores de contenido hacen que el contenido de los archivos AMP esté disponible para ser rastreado (sujeto a restricciones de robots.txt), almacenado en caché y mostrado por terceros.

## Actuación<a name="performance"></a>

El rendimiento predecible es un objetivo de diseño clave para AMP HTML. Principalmente, nuestro objetivo es reducir el tiempo hasta que el usuario puede consumir / utilizar el contenido de una página. En términos concretos, esto significa que:

- Las solicitudes HTTP necesarias para representar y diseñar completamente el documento deben minimizarse.
- Los recursos como imágenes o anuncios solo deben descargarse si es probable que el usuario los vea.
- Los navegadores deberían poder calcular el espacio que necesita cada recurso en la página sin buscar ese recurso.

## El formato HTML de AMP<a name="the-amp-html-format"></a>

### Documento de muestra<a name="sample-document"></a>

[sourcecode:html]
<!DOCTYPE html>
<html ⚡>
  <head>
    <meta charset="utf-8" />
    <title>Sample document</title>
    <link rel="canonical" href="./regular-html-version.html" />
    <meta
      name="viewport"
      content="width=device-width,minimum-scale=1,initial-scale=1"
    />
    <style amp-custom>
      h1 {
        color: red;
      }
    </style>
    <script type="application/ld+json">
      {
        "@context": "http://schema.org",
        "@type": "NewsArticle",
        "headline": "Article headline",
        "image": ["thumbnail1.jpg"],
        "datePublished": "2015-02-05T08:00:00+08:00"
      }
    </script>
    <script
      async
      custom-element="amp-carousel"
      src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"
    ></script>
    <script
      async
      custom-element="amp-ad"
      src="https://cdn.ampproject.org/v0/amp-ad-0.1.js"
    ></script>
    <style amp-boilerplate>
      body {
        -webkit-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        -moz-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        -ms-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        animation: -amp-start 8s steps(1, end) 0s 1 normal both;
      }
      @-webkit-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-moz-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-ms-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-o-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
    </style>
    <noscript
      ><style amp-boilerplate>
        body {
          -webkit-animation: none;
          -moz-animation: none;
          -ms-animation: none;
          animation: none;
        }
      </style></noscript
    >
    <script async src="https://cdn.ampproject.org/v0.js"></script>
  </head>
  <body>
    <h1>Sample document</h1>
    <p>
      Some text
      <amp-img src="sample.jpg" width="300" height="300"></amp-img>
    </p>
    <amp-ad
      width="300"
      height="250"
      type="a9"
      data-aax_size="300x250"
      data-aax_pubname="test123"
      data-aax_src="302"
    >
    </amp-ad>
  </body>
</html>
[/sourcecode]

### Marcado requerido<a name="required-markup"></a>

Los documentos HTML de AMP DEBEN

- <a name="dctp"></a>comience con el doctype `<!doctype html>` . [🔗](#dctp)
- <a name="ampd"></a>contener una etiqueta `<html ⚡>` nivel superior (también se acepta `<html amp>` ). [🔗](#ampd)
- <a name="crps"></a>contienen etiquetas `<head>` y `<body>` (son opcionales en HTML). [🔗](#crps)
- <a name="canon"></a>contienen una etiqueta `<link rel="canonical" href="$SOME_URL">` dentro de su cabeza que apunta a la versión HTML normal del documento HTML de AMP o a sí mismo si no existe dicha versión HTML. [🔗](#canon)
- <a name="chrs"></a>contienen una etiqueta `<meta charset="utf-8">` como primer elemento secundario de su etiqueta principal. [🔗](#chrs)
- <a name="vprt"></a>contienen una etiqueta `<meta name="viewport" content="width=device-width">` dentro de su etiqueta principal. También se recomienda incluir `minimum-scale=1` y `initial-scale=1` . [🔗](#vprt)
- <a name="scrpt"></a>contienen una etiqueta `<script async src="https://cdn.ampproject.org/v0.js"></script>` dentro de su etiqueta principal. [🔗](#scrpt)
- <a name="boilerplate"></a>contienen el [código repetitivo de AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) ( `head > style[amp-boilerplate]` y `noscript > style[amp-boilerplate]` ) en su etiqueta principal. [🔗](#boilerplate)

### Metadatos<a name="metadata"></a>

Se recomienda que los documentos HTML de AMP estén anotados con metadatos estandarizados: [Open Graph Protocol](http://ogp.me/) , [Twitter Cards](https://dev.twitter.com/cards/overview) , etc.

También recomendamos que los documentos HTML de AMP estén marcados con [schema.org/CreativeWork](https://schema.org/CreativeWork) o cualquiera de sus tipos más específicos, como [schema.org/NewsArticle](https://schema.org/NewsArticle) o [schema.org/BlogPosting](https://schema.org/BlogPosting) .

### Etiquetas HTML<a name="html-tags"></a>

Las etiquetas HTML se pueden utilizar sin cambios en HTML de AMP. Algunas etiquetas tienen etiquetas personalizadas equivalentes (como `<img>` y `<amp-img>` ) y otras etiquetas están estrictamente prohibidas:

<table>
  <tr>
    <th width="30%">Etiqueta</th>
    <th>Estado en HTML de AMP</th>
  </tr>
  <tr>
    <td width="30%">guión</td>
    <td>Prohibido a menos que el tipo sea <code>application/ld+json</code> , <code>application/json</code> o <code>text/plain</code> . (Se pueden agregar otros valores no ejecutables según sea necesario). La excepción es la etiqueta de secuencia de comandos obligatoria para cargar el tiempo de ejecución de AMP y las etiquetas de secuencia de comandos para cargar componentes extendidos.</td>
  </tr>
  <tr>
    <td width="30%">noscript</td>
    <td>Permitido. Se puede usar en cualquier parte del documento. Si se especifica, el contenido dentro del elemento <code>&lt;noscript&gt;</code> se muestra si el usuario deshabilita JavaScript.</td>
  </tr>
  <tr>
    <td width="30%">base</td>
    <td>Prohibido.</td>
  </tr>
  <tr>
    <td width="30%">img</td>
    <td>Reemplazado con <code>amp-img</code> .<br> Tenga en cuenta: <code>&lt;img&gt;</code> es un <a href="https://www.w3.org/TR/html5/syntax.html#void-elements">elemento vacío según HTML5</a> , por lo que no tiene una etiqueta final. Sin embargo, <code>&lt;amp-img&gt;</code> tiene una etiqueta final <code>&lt;/amp-img&gt;</code> .</td>
  </tr>
    <tr>
    <td width="30%">imagen</td>
    <td>Prohibido. Sirva diferentes formatos de imagen mediante el uso del atributo de <a href="https://amp.dev/documentation/guides-and-tutorials/develop/style_and_layout/placeholders?format=websites">reserva</a> o proporcione varios <a href="https://amp.dev/documentation/components/amp-img#attributes"><code>srcset</code> en <code>&lt;amp-img&gt;</code></a> .</td>
  </tr>
  <tr>
    <td width="30%">vídeo</td>
    <td>Reemplazado con <code>amp-video</code> .</td>
  </tr>
  <tr>
    <td width="30%">audio</td>
    <td>Reemplazado con <code>amp-audio</code> .</td>
  </tr>
  <tr>
    <td width="30%">iframe</td>
    <td>Reemplazado con <code>amp-iframe</code> .</td>
  </tr>
    <tr>
    <td width="30%">cuadro</td>
    <td>Prohibido.</td>
  </tr>
  <tr>
    <td width="30%">marco</td>
    <td>Prohibido.</td>
  </tr>
  <tr>
    <td width="30%">objeto</td>
    <td>Prohibido.</td>
  </tr>
  <tr>
    <td width="30%">param</td>
    <td>Prohibido.</td>
  </tr>
  <tr>
    <td width="30%">applet</td>
    <td>Prohibido.</td>
  </tr>
  <tr>
    <td width="30%">empotrar</td>
    <td>Prohibido.</td>
  </tr>
  <tr>
    <td width="30%">formar</td>
    <td>Permitido. Requiere incluir extensión en <a href="https://amp.dev/documentation/components/amp-form">forma de amplificador</a> .</td>
  </tr>
  <tr>
    <td width="30%">elementos de entrada</td>
    <td>Se permite principalmente con la <a href="https://amp.dev/documentation/components/amp-form#inputs-and-fields">excepción de algunos tipos de entrada</a> , a saber, <code>&lt;input type=button&gt;</code> , <code>&lt;button type=image&gt;</code> no son válidos. También se permiten etiquetas relacionadas: <code>&lt;fieldset&gt;</code> , <code>&lt;label&gt;</code>
</td>
  </tr>
  <tr>
    <td width="30%">botón</td>
    <td>Permitido.</td>
  </tr>
  <tr>
    <td width="30%"><code>&lt;a name="cust"&gt;&lt;/a&gt;style</code></td>
    <td>
<a href="#boilerplate">Etiqueta de estilo requerida para amp-boilerplate</a> . Se permite una etiqueta de estilo adicional en la etiqueta de la cabeza con el propósito de personalizar el estilo. Esta etiqueta de estilo debe tener el atributo <code>amp-custom</code> . <a href="#cust">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">enlace</td>
    <td>Se permiten los valores <code>rel</code> registrados en <a href="http://microformats.org/wiki/existing-rel-values">microformats.org</a> . Si falta un valor <code>rel</code> en nuestra lista blanca, <a href="https://github.com/ampproject/amphtml/issues/new">envíe un problema</a> . <code>stylesheet</code> y otros valores como <code>preconnect</code> , <code>prerender</code> y <code>prefetch</code> que tienen efectos secundarios en el navegador no están permitidos. Existe un caso especial para obtener hojas de estilo de proveedores de fuentes de la lista blanca.</td>
  </tr>
  <tr>
    <td width="30%">meta</td>
    <td>El atributo <code>http-equiv</code> puede utilizarse para valores permitidos específicos; consulte la <a href="https://github.com/ampproject/amphtml/blob/master/validator/validator-main.protoascii">especificación del validador AMP</a> para obtener más detalles.</td>
  </tr>
  <tr>
    <td width="30%"><code>&lt;a name="ancr"&gt;&lt;/a&gt;a</code></td>
    <td>El valor del atributo <code>href</code> no debe comenzar con <code>javascript:</code> Si se establece, el <code>target</code> valor de atributo debe ser <code>_blank</code> . De lo contrario permitido. <a href="#ancr">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">svg</td>
    <td>Se permiten la mayoría de los elementos SVG.</td>
  </tr>
</table>

Las implementaciones de validadores deben usar una lista blanca basada en la especificación HTML5 con las etiquetas anteriores eliminadas. Consulte el [anexo de etiquetas de AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-tag-addendum.md) .

### Comentarios<a name="comments"></a>

No se permiten comentarios HTML condicionales.

### Atributos HTML<a name="html-attributes"></a>

Los nombres de atributos que comienzan con `on` (como `onclick` o `onmouseover` ) no están permitidos en AMP HTML. El atributo con el nombre literal `on` está permitido (sin sufijo).

Los atributos relacionados con XML, como xmlns, xml: lang, xml: base y xml: space, no están permitidos en AMP HTML.

Los atributos internos de AMP con el prefijo `i-amp-` no están permitidos en AMP HTML.

### Clases<a name="classes"></a>

Los nombres de clases de AMP internos con el prefijo `-amp-` e `i-amp-` no están permitidos en AMP HTML.

Consulte la [documentación de AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-css-classes.md) para conocer el significado de los nombres de clases con el prefijo `amp-` . El uso de estas clases está permitido y está destinado a permitir la personalización de algunas funciones del tiempo de ejecución y las extensiones de AMP.

Todos los demás nombres de clases creados están permitidos en el marcado HTML de AMP.

### Identificaciones<a name="ids"></a>

Ciertos nombres de ID no están permitidos en AMP HTML, como los ID con el prefijo `-amp-` e `i-amp-` que pueden entrar en conflicto con los ID de AMP internos.

Consulte la documentación de AMP para conocer las extensiones específicas antes de usar `amp-` y `AMP` ID para evitar conflictos con las funciones proporcionadas por estas extensiones, como `amp-access` .

Vea la lista completa de nombres de identificación no permitidos buscando `mandatory-id-attr` [aquí](https://github.com/ampproject/amphtml/blob/master/spec/../validator/validator-main.protoascii) .

### Enlaces<a name="links"></a>

El esquema `javascript:` no está permitido.

### Hojas de estilo<a name="stylesheets"></a>

Las principales etiquetas semánticas y los elementos personalizados de AMP vienen con estilos predeterminados para que la creación de un documento receptivo sea razonablemente fácil. Es posible que en el futuro se agregue una opción para excluirse de los estilos predeterminados.

#### @-reglas<a name="-rules"></a>

Se permiten las siguientes reglas @ en las hojas de estilo:

`@font-face` , `@keyframes` , `@media` , `@page` , `@supports` .

`@import` no se permitirá. Es posible que se agreguen otros en el futuro.

#### Hojas de estilo de autor<a name="author-stylesheets"></a>

Los autores pueden agregar estilos personalizados a un documento usando una sola etiqueta `<style amp-custom>` en el encabezado del documento o estilos en línea.

`@keyframes` rules are allowed in the `<style amp-custom>`. However, if they are too many of them, it's recommended to place them in the additional `<style amp-keyframes>` tag, which must be located at the end of the AMP document. For details, see the [Keyframes stylesheet](#keyframes-stylesheet) section of this document.

#### Selectores<a name="selectors"></a>

Las siguientes restricciones se aplican a los selectores en las hojas de estilo de autor:

##### Nombres de clases y etiquetas<a name="class-and-tag-names"></a>

Class names, IDs, tag names and attributes, in author stylesheets, may not start with the string `-amp-` and `i-amp-`. These are reserved for internal use by the AMP runtime. It follows, that the user's stylesheet may not reference CSS selectors for `-amp-` classes, `i-amp-` IDs and `i-amp-` tags and attributes. These classes, IDs and tag/attribute names are not meant to be customized by authors. Authors, however, can override styles of `amp-` classes and tags for any CSS properties not explicitly forbidden by these components' spec.

Para evitar el uso de selectores de atributos para eludir las limitaciones de los nombres de clase, generalmente no se permite que los selectores de CSS contengan tokens y cadenas que comiencen con `-amp-` e `i-amp-` .

#### Importante<a name="important"></a>

No se permite el uso del calificador `!important` . Este es un requisito necesario para permitir que AMP aplique sus invariantes de tamaño de elementos.

#### Propiedades<a name="properties"></a>

AMP solo permite transiciones y animaciones de propiedades que pueden acelerarse por GPU en navegadores comunes. Actualmente tenemos la lista blanca: `opacity` , `transform` (también `-vendorPrefix-transform` ).

En los siguientes ejemplos, `<property>` debe estar en la lista blanca anterior.

- `transition <property>` (también -vendorPrefix-transición)
- `@keyframes name { from: {<property>: value} to {<property: value>} }` (también `@-vendorPrefix-keyframes` )

#### Talla máxima<a name="maximum-size"></a>

Es un error de validación si la hoja de estilo del autor o los estilos en línea juntos tienen más de 75.000 bytes.

### Hoja de estilo de fotogramas clave<a name="keyframes-stylesheet"></a>

Además de `<style amp-custom>` , los autores también pueden agregar la etiqueta `<style amp-keyframes>` , que está permitida específicamente para animaciones de fotogramas clave.

Las siguientes restricciones se aplican a la etiqueta `<style amp-keyframes>` :

1. Solo se puede colocar como el último elemento secundario del elemento `<body>` del documento.
2. Solo puede contener `@keyframes` , `@media` , `@supports` reglas y su combinación.
3. No puede tener más de 500.000 bytes.

La razón por la que existe la etiqueta `<style amp-keyframes>` es que las reglas de los fotogramas clave suelen ser voluminosas incluso para animaciones moderadamente complicadas, lo que conduce a un análisis CSS lento y una primera pintura con contenido. Pero tales reglas a menudo exceden el límite de tamaño impuesto en `<style amp-custom>` . Poner dichas declaraciones de fotogramas clave al final del documento en `<style amp-keyframes>` les permite superar las limitaciones de tamaño. Y dado que los fotogramas clave no bloquean el renderizado, también evita bloquear la primera pintura con contenido para analizarlos.

Ejemplo:

[sourcecode:html]
<style amp-keyframes>
@keyframes anim1 {}

@media (min-width: 600px) {
  @keyframes anim1 {}
}
</style>
</body>
[/sourcecode]

### Fuentes personalizadas<a name="custom-fonts"></a>

Los autores pueden incluir hojas de estilo para fuentes personalizadas. Los 2 métodos admitidos son etiquetas de enlace que apuntan a proveedores de fuentes de la lista blanca y la inclusión de `@font-face` .

Ejemplo:

[sourcecode:html]
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css?family=Tangerine"
/>
[/sourcecode]

Los proveedores de fuentes pueden aparecer en la lista blanca si admiten integraciones solo de CSS y sirven a través de HTTPS. Actualmente, se permiten los siguientes orígenes para la publicación de fuentes a través de etiquetas de enlace:

- Fonts.com: `https://fast.fonts.net`
- Fuentes de Google: `https://fonts.googleapis.com`
- Fuente impresionante: `https://maxcdn.bootstrapcdn.com, https://use.fontawesome.com`
- [Typekit](https://helpx.adobe.com/typekit/using/google-amp.html) : `https://use.typekit.net/kitId.css` (reemplace `kitId` consecuencia)

NOTA DE LOS IMPLEMENTADORES: Para agregar a esta lista, es necesario realizar un cambio en la regla AMP Cache CSP.

Los autores son libres de incluir todas las fuentes personalizadas a través de una instrucción CSS `@font-face` través de su CSS personalizado. Las fuentes incluidas a través de `@font-face` deben obtenerse mediante el esquema HTTP o HTTPS.

## Tiempo de ejecución de AMP<a name="amp-runtime"></a>

El tiempo de ejecución de AMP es una parte de JavaScript que se ejecuta dentro de cada documento AMP. Proporciona implementaciones para elementos personalizados de AMP, administra la carga de recursos y la priorización y, opcionalmente, incluye un validador de tiempo de ejecución para HTML de AMP para usar durante el desarrollo.

El tiempo de ejecución de AMP se carga a través de la etiqueta obligatoria `<script src="https://cdn.ampproject.org/v0.js"></script>` en el documento AMP `<head>` .

El tiempo de ejecución de AMP se puede colocar en un modo de desarrollo para cualquier página. El modo de desarrollo activará la validación de AMP en la página incorporada, que emitirá el estado de validación y cualquier error a la consola de desarrollador de JavaScript. El modo de desarrollo se puede activar agregando `#development=1` a la URL de la página.

## Recursos<a name="resources"></a>

Los recursos como imágenes, videos, archivos de audio o anuncios deben incluirse en un archivo HTML AMP a través de elementos personalizados como `<amp-img>` . Los llamamos "recursos administrados" porque el tiempo de ejecución de AMP decide cuándo se cargarán y mostrarán al usuario.

No hay garantías particulares en cuanto al comportamiento de carga del tiempo de ejecución de AMP, pero generalmente debe esforzarse por cargar los recursos lo suficientemente rápido, de modo que se carguen en el momento en que el usuario quisiera verlos si es posible. El tiempo de ejecución debe priorizar los recursos actualmente en la ventana gráfica e intentar predecir cambios en la ventana gráfica y precargar los recursos en consecuencia.

El tiempo de ejecución de AMP puede decidir en cualquier momento descargar recursos que no están actualmente en la ventana gráfica o reutilizar los contenedores de recursos, como iframes, para reducir el consumo general de RAM.

## Componentes AMP<a name="amp-components"></a>

HTML de AMP utiliza elementos personalizados denominados "componentes de AMP" para sustituir las etiquetas de carga de recursos integradas, como `<img>` y `<video>` y para implementar funciones con interacciones complejas, como lightboxes de imágenes o carruseles.

Consulte las [especificaciones del componente AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-components.md) para obtener detalles sobre los componentes compatibles.

Hay 2 tipos de componentes AMP compatibles:

1. Incorporado
2. Extendido

Los componentes integrados siempre están disponibles en un documento AMP y tienen un elemento personalizado dedicado, como `<amp-img>` . Los componentes extendidos deben incluirse explícitamente en el documento.

### Atributos comunes<a name="common-attributes"></a>

#### `layout` , `width` , `height` , `media` , `placeholder` , `fallback`<a name="layout-width-height-media-placeholder-fallback"></a>

Estos atributos definen el diseño de un elemento. El objetivo clave aquí es garantizar que el elemento se pueda mostrar y su espacio se pueda reservar correctamente antes de que se haya descargado cualquiera de los recursos JavaScript o remotos.

Consulte el [Sistema de diseño de AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-layout.md) para obtener detalles sobre el sistema de diseño.

#### `on` <a name="on"></a>

El atributo `on` se utiliza para instalar controladores de eventos en elementos. Los eventos que se admiten dependen del elemento.

El valor de la sintaxis es un lenguaje específico de dominio simple de la forma:

[sourcecode:javascript]
eventName:targetId[.methodName[(arg1=value, arg2=value)]]
[/sourcecode]

Ejemplo: `on="tap:fooId.showLightbox"`

Si se omite `methodName` el método predeterminado se ejecuta si está definido para el elemento. Ejemplo: `on="tap:fooId"`

Algunas acciones, si se documentan, pueden aceptar argumentos. Los argumentos se definen entre paréntesis en notación `key=value` . Los valores aceptados son:

- cadenas simples sin comillas: `simple-value` ;
- cadenas entre comillas: `"string value"` o `'string value'` ;
- valores booleanos: `true` o `false` ;
- números: `11` o `1.1` .

Puede escuchar varios eventos en un elemento separando los dos eventos con un punto y coma `;` .

Ejemplo: `on="submit-success:lightbox1;submit-error:lightbox2"`

Más información sobre [acciones y eventos de AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-actions-and-events.md) .

### Componentes extendidos<a name="extended-components"></a>

Los componentes extendidos son componentes que no se envían necesariamente con el tiempo de ejecución de AMP. En su lugar, deben incluirse explícitamente en el documento.

Los componentes extendidos se cargan al incluir una etiqueta `<script>` en el encabezado del documento de esta manera:

[sourcecode:html]
<script
  async
  custom-element="amp-carousel"
  src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"
></script>
[/sourcecode]

La etiqueta `<script>` debe tener un atributo `async` y debe tener un atributo de `custom-element` haga referencia al nombre del elemento.

Las implementaciones en tiempo de ejecución pueden usar el nombre para representar marcadores de posición para estos elementos.

La URL del script debe comenzar con `https://cdn.ampproject.org` y debe seguir un patrón muy estricto de `/v\d+/[az-]+-(latest|\d+|\d+\.\d+)\.js` .

##### URL<a name="url"></a>

La URL para componentes extendidos tiene el formato:

[sourcecode:http]
https://cdn.ampproject.org/$RUNTIME_VERSION/$ELEMENT_NAME-$ELEMENT_VERSION.js
[/sourcecode]

##### Control de versiones<a name="versioning"></a>

Consulta la [política de control de versiones de AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-versioning-policy.md) .

### Plantillas<a name="templates"></a>

Las plantillas representan contenido HTML en función de la plantilla específica del idioma y los datos JSON proporcionados.

Consulte la [especificación de la plantilla AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-templates.md) para obtener detalles sobre las plantillas compatibles.

Las plantillas no se envían con el tiempo de ejecución de AMP y deben descargarse al igual que con los elementos extendidos. Los componentes extendidos se cargan al incluir una etiqueta `<script>` en el encabezado del documento de esta manera:

[sourcecode:html]
<script
  async
  custom-template="amp-mustache"
  src="https://cdn.ampproject.org/v0/amp-mustache-0.2.js"
></script>
[/sourcecode]

La etiqueta `<script>` debe tener un atributo `async` y debe tener un atributo de `custom-template` haga referencia al tipo de plantilla. La URL del script debe comenzar con `https://cdn.ampproject.org` y debe seguir un patrón muy estricto de `/v\d+/[az-]+-(latest|\d+|\d+\.\d+)\.js` .

Las plantillas se declaran en el documento de la siguiente manera:

[sourcecode:html]
<template type="amp-mustache" id="template1">
  Hello {% raw %}{{you}}{% endraw %}!
</template>
[/sourcecode]

El atributo de `type` es obligatorio y debe hacer referencia a un script de `custom-template` declarado.

El atributo `id` es opcional. Los elementos AMP individuales descubren sus propias plantillas. Los escenarios típicos involucrarían un elemento AMP que busca una `<template>` ya sea entre sus hijos o referenciada por ID.

La sintaxis dentro del elemento de plantilla depende del idioma de plantilla específico. Sin embargo, el idioma de la plantilla podría estar restringido dentro de AMP. Por ejemplo, de acuerdo con el elemento "plantilla", todas las producciones deben realizarse sobre un DOM válido y bien formado. Todas las salidas de la plantilla también están sujetas a desinfección para garantizar una salida válida para AMP.

To learn about the syntax and restrictions for an template, visit the [template's documentation](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-templates.md#templates).

##### URL<a name="url-1"></a>

La URL para componentes extendidos tiene el formato:

[sourcecode:http]
https://cdn.ampproject.org/$RUNTIME_VERSION/$TEMPLATE_TYPE-$TEMPLATE_VERSION.js
[/sourcecode]

##### Control de versiones<a name="versioning-1"></a>

Consulte el control de versiones de elementos personalizados para obtener más detalles.

## Seguridad<a name="security"></a>

Los documentos HTML de AMP no deben generar errores cuando se entregan con una política de seguridad de contenido que no incluya las palabras clave `unsafe-inline` y `unsafe-eval` .

El formato HTML de AMP está diseñado para que ese sea siempre el caso.

Todos los elementos de la plantilla de AMP deben pasar por la revisión de seguridad de AMP antes de que puedan enviarse al repositorio de AMP.

## SVG<a name="svg"></a>

Actualmente, se permiten los siguientes elementos SVG:

- conceptos básicos: "g", "glifo", "glyphRef", "imagen", "marcador", "metadatos", "ruta", "color sólido", "svg", "cambiar", "vista"
- formas: "círculo", "elipse", "línea", "polígono", "polilínea", "rect"
- texto: "text", "textPath", "tref", "tspan"
- rendering: "clipPath", "filter", "hkern", "linearGradient", "mask", "pattern", "radialGradient", "vkern"
- especial: "defs" (todos los niños de arriba están permitidos aquí), "símbolo", "uso"
- filter: "feColorMatrix", "feComposite", "feGaussianBlur", "feMerge", "feMergeNode", "feOffset", "ForeignObject"
- ARIA: "desc", "título"

Además de estos atributos:

- "xlink: href": solo se permiten los URI que comienzan con "#"
- "estilo"

## Descubrimiento de documentos AMP<a name="amp-document-discovery"></a>

El mecanismo que se describe a continuación proporciona una forma estandarizada para que el software descubra si existe una versión AMP para un documento canónico.

Si existe un documento AMP que es una representación alternativa de un documento canónico, entonces el documento canónico debe apuntar al documento AMP a través de una etiqueta de `link` con la [relación "amphtml"](http://microformats.org/wiki/existing-rel-values#HTML5_link_type_extensions) .

Ejemplo:

[sourcecode:html]
<link rel="amphtml" href="https://www.example.com/url/to/amp/document.html" />
[/sourcecode]

Se espera que el documento AMP en sí apunte a su documento canónico a través de una etiqueta de `link` con la relación "canónico".

Ejemplo:

[sourcecode:html]
<link
  rel="canonical"
  href="https://www.example.com/url/to/canonical/document.html"
/>
[/sourcecode]

(Si un solo recurso es simultáneamente el AMP *y* el documento canónico, la relación canónica debe apuntar a sí misma; no se requiere una relación "amphtml").

Tenga en cuenta que para una mayor compatibilidad con los sistemas que consumen AMP, debería ser posible leer la relación "amphtml" sin ejecutar JavaScript. (Es decir, la etiqueta debe estar presente en el HTML sin procesar y no inyectarse a través de JavaScript).
