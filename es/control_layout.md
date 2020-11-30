---
"$title": Consultas de diseño y medios
"$order": '1'
description: "AMP admite consultas de medios y amp; consultas de elementos, además viene con una forma poderosa e integrada de controlar el diseño de elementos individuales. El atributo de diseño hace que trabajar con ..."
formats:
- sitios web
- correo electrónico
- anuncios
- cuentos
author: Meggin
contributors:
- pbakaus
---

AMP admite tanto **consultas de medios como consultas de** **elementos** , además viene con una forma poderosa e integrada de controlar el **diseño** de elementos individuales. El atributo de `layout` hace que trabajar con un diseño totalmente receptivo y crearlo sea mucho más fácil que si usara CSS solo.

## Imágenes receptivas, simplificadas

Cree imágenes receptivas especificando el `width` y la `height` , estableciendo el diseño en `responsive` e indicando con [`srcset`](art_direction.md) qué recurso de imagen usar según los diferentes tamaños de pantalla:

[sourcecode:html]
<amp-img
    src="/img/narrow.jpg"
    srcset="/img/wide.jpg 640w,
           /img/narrow.jpg 320w"
    width="1698"
    height="2911"
    layout="responsive"
    alt="an image">
</amp-img>
[/sourcecode]

Este elemento [`amp-img`](../../../../documentation/components/reference/amp-img.md) se ajusta automáticamente al ancho de su elemento contenedor, y su altura se establece automáticamente en la relación de aspecto determinada por el ancho y alto dados. Pruébelo cambiando el tamaño de esta ventana del navegador:

<amp-img src="/static/img/background.jpg" width="1920" height="1080" layout="responsive"></amp-img>

[tip type="tip"] **SUGERENCIA:** vea nuestras demostraciones en vivo en paralelo de [`amp-img`](../../../../documentation/components/reference/amp-img.md) : [demostraciones en vivo en AMP por ejemplo](../../../../documentation/examples/documentation/amp-img.html?format=websites) . [/tip]

## El atributo de diseño<a name="the-layout-attribute"></a>

El atributo de `layout` le brinda un control fácil por elemento sobre cómo debe mostrarse su elemento en la pantalla. Muchas de estas cosas son posibles con CSS puro, pero son mucho más difíciles y requieren una gran cantidad de hacks. En su lugar, utilice el atributo de `layout` .

### Valores admitidos para el atributo de `layout`

Los siguientes valores se pueden utilizar en el atributo de `layout` :

<table>
  <thead>
    <tr>
      <th data-th="Layout type" class="col-thirty">Tipo de diseño</th>
      <th data-th="Width/height required" class="col-twenty">Anchura/<br> altura requerida</th>
      <th data-th="Behavior">Comportamiento</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Layout type"><code>nodisplay</code></td>
      <td data-th="Description">No</td>
      <td data-th="Behavior">Elemento no mostrado. Este diseño se puede aplicar a todos los elementos de AMP. El componente ocupa cero espacio en la pantalla como si su estilo de visualización no fuera ninguno. Se supone que el elemento puede mostrarse a sí mismo en la acción del usuario, por ejemplo, <a href="../../../../documentation/components/reference/amp-lightbox.md"><code>amp-lightbox</code></a> .</td>
    </tr>
    <tr>
      <td data-th="Layout type"><code>fixed</code></td>
      <td data-th="Description">si</td>
      <td data-th="Behavior">El elemento tiene un ancho y una altura fijos sin capacidad de respuesta compatible. Las únicas excepciones son los elementos <a href="../../../../documentation/components/reference/amp-pixel.md"><code>amp-pixel</code></a> y <a href="../../../../documentation/components/reference/amp-audio.md"><code>amp-audio</code></a> .</td>
    </tr>
    <tr>
      <td data-th="Layout type"><code>responsive</code></td>
      <td data-th="Description">si</td>
      <td data-th="Behavior">El tamaño del elemento se ajusta al ancho de su elemento contenedor y cambia su altura automáticamente a la relación de aspecto dada por los atributos de ancho y alto. Este diseño funciona muy bien para la mayoría de los elementos AMP, incluidos <a href="../../../../documentation/components/reference/amp-img.md"><code>amp-img</code></a> , <a href="../../../../documentation/components/reference/amp-video.md"><code>amp-video</code></a> . El espacio disponible depende del elemento principal y también se puede personalizar con CSS de <code>max-width</code> .<p> <strong>Nota</strong> : Los elementos con <code>"layout=responsive"</code> no tienen un tamaño intrínseco. El tamaño del elemento se determina a partir de su elemento contenedor. Para garantizar que se muestre su elemento AMP, debe especificar un ancho y un alto para el elemento contenedor. No especifique <code>"display:table"</code> en el elemento contenedor, ya que esto anula la visualización del elemento AMP, lo que hace que el elemento AMP sea invisible.</p>
</td>
    </tr>
    <tr>
      <td data-th="Layout type"><code>fixed-height</code></td>
      <td data-th="Description">Solo altura</td>
      <td data-th="Behavior">El elemento ocupa el espacio disponible pero mantiene la altura sin cambios. Este diseño funciona bien para elementos como <a href="../../../../documentation/components/reference/amp-carousel.md"><code>amp-carousel</code></a> que involucran contenido colocado horizontalmente. El atributo de <code>width</code> no debe estar presente o debe ser igual a <code>auto</code> .</td>
    </tr>
    <tr>
      <td data-th="Layout type"><code>fill</code></td>
      <td data-th="Description">No</td>
      <td data-th="Behavior">Element ocupa el espacio disponible, tanto en ancho como en alto. En otras palabras, el diseño de un elemento de relleno coincide con su padre. Para que un elemento llene su contenedor principal, asegúrese de que el contenedor principal especifique `posición: relativa` o` posición: absoluta`.</td>
    </tr>
    <tr>
      <td data-th="Layout type"><code>container</code></td>
      <td data-th="Description">No</td>
      <td data-th="Behavior">Element permite que sus hijos definan su tamaño, al igual que un <code>div</code> HTML normal. Se supone que el componente no tiene un diseño específico en sí mismo, sino que solo actúa como un contenedor. Sus hijos se rinden inmediatamente.</td>
    </tr>
    <tr>
      <td data-th="Layout type"><code>flex-item</code></td>
      <td data-th="Description">No</td>
      <td data-th="Behavior">Element and other elements in its parent take the parent container's remaining space when the parent is a flexible container (i.e., <code>display:flex</code>). The element size is determined by the parent element and the number of other elements inside the parent according to the <code>display:flex</code> CSS layout.</td>
    </tr>
    <tr>
      <td data-th="Layout type"><code>intrinsic</code></td>
      <td data-th="Description">si</td>
      <td data-th="Behavior">El elemento toma el espacio disponible y cambia su altura automáticamente a la relación de aspecto dada por los atributos de <code>width</code> y <code>height</code> <em>hasta</em> que alcanza el tamaño natural del elemento o alcanza una restricción CSS (por ejemplo, ancho máximo). Los atributos de ancho y alto deben estar presentes. Este diseño funciona muy bien para la mayoría de los elementos AMP, incluidos <a href="../../../../documentation/components/reference/amp-img.md"><code>amp-img</code></a> , <a href="../../../../documentation/components/reference/amp-carousel.md"><code>amp-carousel</code></a> , etc. El espacio disponible depende del elemento principal y también se puede personalizar usando CSS de <code>max-width</code> . Este diseño se diferencia del <code>responsive</code> por tener una altura y un ancho intrínsecos. Esto es más evidente dentro de un elemento flotante donde un diseño <code>responsive</code> representará 0x0 y un diseño <code>intrinsic</code> se inflará al más pequeño de su tamaño natural o cualquier restricción CSS.</td>
    </tr>
  </tbody>
</table>

[tip type="tip"] **SUGERENCIA:** visita la página [Demostración de diseños de AMP](../../../../documentation/guides-and-tutorials/learn/amp-html-layout/layouts_demonstrated.html) para ver cómo responden los distintos diseños al cambio de tamaño de la pantalla. [/tip]

### ¿Qué pasa si el ancho y el alto no están definidos?<a name="what-if-width-and-height-are-undefined"></a>

En algunos casos, si no se especifica el `width` o el `height` , el tiempo de ejecución de AMP puede predeterminar estos valores de la siguiente manera:

- [`amp-pixel`](../../../../documentation/components/reference/amp-pixel.md) : tanto el ancho como el alto están predeterminados en 0.
- [`amp-audio`](../../../../documentation/components/reference/amp-audio.md) : el ancho y alto predeterminados se infieren del navegador.

### ¿Qué pasa si no se especifica el atributo de <code>layout</code> ?<a name="what-if-the-layout-attribute-isnt-specified"></a>

Si no se especifica el atributo de <code>layout</code> , AMP intenta inferir o adivinar el valor apropiado:

<table>
  <thead>
    <tr>
      <th data-th="Rule">Regla</th>
      <th data-th="Inferred layout" class="col-thirty">Diseño inferido</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Rule">
<code>height</code> está presente y el <code>width</code> está ausente o es igual a <code>auto</code>
</td>
      <td data-th="Inferred layout"><code>fixed-height</code></td>
    </tr>
    <tr>
      <td data-th="Rule">
<code>width</code> or <code>height</code> attributes are present along with the <code>sizes</code> attribute</td>
      <td data-th="Inferred layout"><code>responsive</code></td>
    </tr>
    <tr>
      <td data-th="Rule">Están presentes los atributos de <code>width</code> y <code>height</code>
</td>
      <td data-th="Inferred layout"><code>fixed</code></td>
    </tr>
    <tr>
      <td data-th="Rule">
<code>width</code> y el <code>height</code> no están presentes</td>
      <td data-th="Inferred layout"><code>container</code></td>
    </tr>
  </tbody>
</table>

## Usar consultas de medios

### Consultas de medios CSS

Use [`@media`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media) para controlar cómo se ve y se comporta el diseño de la página, como lo haría en cualquier otro sitio web. Cuando la ventana del navegador cambia de tamaño u orientación, las consultas de medios se vuelven a evaluar y los elementos se ocultan y se muestran en función de los nuevos resultados.

[tip type="read-on"] SIGUE **LEER:** Obtén más información sobre cómo controlar el diseño aplicando consultas de medios en [Usar consultas de medios CSS para la capacidad de respuesta](https://developers.google.com/web/fundamentals/design-and-ui/responsive/fundamentals/use-media-queries?hl=en) . [/tip]

### Consultas de elementos de medios<a name="element-media-queries"></a>

Una característica adicional para el diseño receptivo disponible en AMP es el atributo de `media` . Este atributo se puede utilizar en todos los elementos de AMP; funciona de manera similar a las consultas de medios en su hoja de estilo global, pero solo afecta el elemento específico en una sola página.

Por ejemplo, aquí tenemos 2 imágenes con consultas de medios mutuamente excluyentes.

[sourcecode:html]
<amp-img
    media="(min-width: 650px)"
    src="wide.jpg"
    width="527"
    height="355"
    layout="responsive">
</amp-img>
[/sourcecode]

Dependiendo del ancho de la pantalla, se buscará y renderizará uno u otro.

[sourcecode:html]
<amp-img
    media="(max-width: 649px)"
    src="narrow.jpg"
    width="466"
    height="193"
    layout="responsive">
</amp-img>
[/sourcecode]
