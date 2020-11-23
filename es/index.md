---
"$title": Sistema de diseño AMP HTML
order: '1'
formats:
- sitios web
- correo electrónico
- cuentos
- anuncios
teaser:
  text: " Visión general"
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/spec/amp-html-layout.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2015 The AMP HTML Authors. All Rights Reserved.

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

## Visión general<a name="overview"></a>

El objetivo principal del sistema de diseño es garantizar que los elementos AMP puedan expresar su diseño para que el tiempo de ejecución pueda inferir el tamaño de los elementos antes de que se hayan completado los recursos remotos, como JavaScript y llamadas de datos. Esto es importante, ya que reduce significativamente el jank de renderizado y desplazamiento.

Teniendo esto en cuenta, el sistema de diseño AMP está diseñado para admitir diseños pocos pero flexibles que brinden buenas garantías de rendimiento. Este sistema se basa en un conjunto de atributos como `layout` , `width` , `height` , `sizes` y `heights` para expresar las necesidades de diseño y tamaño del elemento.

## Comportamiento<a name="behavior"></a>

A non-container AMP element (i.e., `layout != container`) starts up in the unresolved/unbuilt mode in which all of its children are hidden except for a placeholder (see `placeholder` attribute). The JavaScript and data payload necessary to fully construct the element may still be downloading and initializing, but the AMP runtime already knows how to size and lay out the element only relying on CSS classes and `layout`, `width`, `height` and `media` attributes. In most cases, a `placeholder`, if specified, is sized and positioned to take all of the element's space.

El `placeholder` se oculta tan pronto como se crea el elemento y se completa su primer diseño. En este punto, se espera que el elemento tenga todos sus elementos secundarios correctamente construidos y colocados y listos para mostrarse y aceptar la entrada de un lector. Este es el comportamiento predeterminado. Cada elemento puede anular, por ejemplo, ocultar el `placeholder` más rápido o mantenerlo por más tiempo.

The element is sized and displayed based on the `layout`, `width`, `height` and `media` attributes by the runtime. All of the layout rules are implemented via CSS internally. The element is said to "define size" if its size is inferable via CSS styles and does not change based on its children: available immediately or inserted dynamically. This does not mean that this element's size cannot change. The layout could be fully responsive as is the case with `responsive`, `fixed-height`, `fill` and `flex-item` layouts. It simply means that the size does not change without an explicit user action, e.g. during rendering or scrolling or post download.

Si el elemento se ha configurado incorrectamente, en PROD no se renderizará en absoluto y en el modo DEV el tiempo de ejecución renderizará el elemento en estado de error. Los posibles errores incluyen valores no válidos o no admitidos de los atributos de `layout` , `width` y `height` .

## Atributos de diseño<a name="layout-attributes"></a>

### `width` y `height`<a name="width-and-height"></a>

Según el valor del atributo de `layout` , los elementos del componente AMP deben tener un atributo de `width` y `height` que contenga un valor de píxel entero. El comportamiento de disposición real está determinado por el atributo de `layout` como se describe a continuación.

En algunos casos, si no se especifica el `width` o el `height` , el tiempo de ejecución de AMP puede predeterminar estos valores de la siguiente manera:

- `amp-pixel` : tanto el `width` como el `height` están predeterminados en 0.
- `amp-audio` : el `width` y `height` predeterminados se infieren del navegador.

### `layout` <a name="layout"></a>

AMP proporciona un conjunto de diseños que especifican cómo se comporta un componente AMP en el diseño del documento. Puede especificar un diseño para un componente agregando el atributo de `layout` con uno de los valores especificados en la tabla siguiente.

**Ejemplo** : una imagen receptiva simple, donde el ancho y el alto se utilizan para determinar la relación de aspecto.

[sourcecode:html]
<amp-img
  src="/img/amp.jpg"
  width="1080"
  height="610"
  layout="responsive"
  alt="an image"
></amp-img>
[/sourcecode]

Valores admitidos para el atributo de `layout` :

<table>
  <thead>
    <tr>
      <th width="30%">Valor</th>
      <th>Comportamiento y requisitos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>No presente</td>
      <td>Si no se especifica ningún valor, el diseño del componente se infiere de la siguiente manera:<ul>
<li> Si la <code>height</code> está presente y el <code>width</code> está ausente o está configurado en <code>auto</code> , se asume un diseño de <code>fixed-height</code> .</li>
<li> Si el <code>width</code> y el <code>height</code> están presentes junto con un atributo de <code>sizes</code> o <code>heights</code> , se asume un diseño <code>responsive</code> .</li>
<li> Si hay <code>width</code> y <code>height</code> , se asume un diseño <code>fixed</code> .</li>
<li> si no hay <code>width</code> y <code>height</code> , se asume un diseño de <code>container</code> .</li>
</ul>
</td>
    </tr>
    <tr>
      <td><code>container</code></td>
      <td>El elemento permite que sus hijos definan su tamaño, al igual que un <code>div</code> HTML normal. Se supone que el componente no tiene un diseño específico en sí mismo, sino que solo actúa como contenedor; sus hijos se rinden inmediatamente.</td>
    </tr>
    <tr>
      <td><code>fill</code></td>
      <td>El elemento ocupa el espacio disponible, tanto ancho como alto. En otras palabras, el diseño y el tamaño de un elemento de <code>fill</code> coincide con su padre. Para que un elemento llene su contenedor principal, especifique el diseño de "relleno" y asegúrese de que el contenedor principal especifique la <code>position:relative</code> o <code>position:absolute</code> .</td>
    </tr>
    <tr>
      <td><code>fixed</code></td>
      <td>El elemento tiene un ancho y una altura fijos sin capacidad de respuesta compatible. Los atributos de <code>width</code> y <code>height</code> deben estar presentes. Las únicas excepciones son los componentes <code>amp-pixel</code> y <code>amp-audio</code> .</td>
    </tr>
    <tr>
      <td><code>fixed-height</code></td>
      <td>El elemento ocupa el espacio disponible, pero mantiene la altura sin cambios. Este diseño funciona bien para elementos como <code>amp-carousel</code> que involucran contenido colocado horizontalmente. El atributo de <code>height</code> debe estar presente. El atributo de <code>width</code> no debe estar presente o debe ser igual a <code>auto</code> .</td>
    </tr>
    <tr>
      <td><code>flex-item</code></td>
      <td>El elemento y otros elementos en su padre con tipo de diseño <code>flex-item</code> toman el espacio restante del contenedor padre cuando el padre es un contenedor flexible (es decir, <code>display: flex</code> ). Los atributos de <code>width</code> y <code>height</code> no son obligatorios.</td>
    </tr>
    <tr>
      <td><code>intrinsic</code></td>
      <td>El elemento toma el espacio disponible y cambia su altura automáticamente a la relación de aspecto dada por los atributos de <code>width</code> y <code>height</code> <em>hasta</em> que alcanza el tamaño del elemento definido por los atributos `ancho` y` alto` pasados al <code>amp-img</code> , o alcanza una restricción CSS, como "max-width". Los atributos de ancho y alto deben estar presentes. Este diseño funciona muy bien para la mayoría de los elementos AMP, incluidos <code>amp-img</code> , <code>amp-carousel</code> , etc. El espacio disponible depende del elemento principal y también se puede personalizar usando CSS de <code>max-width</code> . Este diseño se diferencia del <code>responsive</code> por tener una altura y un ancho intrínsecos. Esto es más evidente dentro de un elemento flotante donde un diseño <code>responsive</code> representará 0x0 y un diseño <code>intrinsic</code> se inflará al más pequeño de su tamaño natural o cualquier restricción CSS.</td>
    </tr>
    <tr>
      <td><code>nodisplay</code></td>
      <td>The element isn't displayed, and takes up zero space on the screen as if its display style was <code>none</code>. This layout can be applied to every AMP element.  It’s assumed that the element can display itself on user action (e.g., <code>amp-lightbox</code>). The <code>width</code> and <code>height</code> attributes are not required.</td>
    </tr>
    <tr>
      <td><code>responsive</code></td>
      <td>El elemento toma el espacio disponible y redimensiona su altura automáticamente a la relación de aspecto dada por los atributos de <code>width</code> y <code>height</code> . Este diseño funciona muy bien para la mayoría de los elementos AMP, incluidos <code>amp-img</code> , <code>amp-video</code> , etc. El espacio disponible depende del elemento principal y también se puede personalizar utilizando CSS de <code>max-width</code> . Los atributos de <code>width</code> y <code>height</code> deben estar presentes.<p> <strong>Nota</strong> : Los elementos con <code>"layout=responsive"</code> no tienen un tamaño intrínseco. El tamaño del elemento se determina a partir de su elemento contenedor. Para garantizar que se muestre su elemento AMP, debe especificar un ancho y un alto para el elemento contenedor. No especifique <code>"display:table"</code> en el elemento contenedor, ya que esto anula la visualización del elemento AMP, lo que hace que el elemento AMP sea invisible.</p>
</td>
    </tr>
  </tbody>
</table>

### `sizes` <a name="sizes"></a>

Todos los elementos AMP que admiten el diseño `responsive` también admiten el atributo `sizes` . El valor de este atributo es una expresión de tamaños como se describe en los [tamaños de img](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) , pero se extiende a todos los elementos, no solo a las imágenes. En resumen, el atributo `sizes` describe cómo se calcula el ancho del elemento en función de las condiciones del medio.

Cuando el `sizes` se especifica atributo junto con `width` y `height` , la `layout` está por defecto en `responsive` .

**Ejemplo** : uso del atributo `sizes`

En el siguiente ejemplo, si la ventana gráfica es más ancha que `320px` , la imagen tendrá 320 px de ancho; de lo contrario, tendrá 100 vw de ancho (100% del ancho de la ventana gráfica).

[sourcecode:html]
<amp-img
  src="https://acme.org/image1.png"
  width="400"
  height="300"
  layout="responsive"
  sizes="(min-width: 320px) 320px, 100vw"
>
</amp-img>
[/sourcecode]

### `disable-inline-width` <a name="disable-inline-width"></a>

El atributo de `sizes` por sí solo establecerá un estilo de `width` línea en el elemento. Al emparejar `disable-inline-width` con `sizes` , el elemento AMP propagará el valor de los `sizes` a la etiqueta subyacente del elemento, al igual que con el `img` anidado dentro de un `amp-img` , **sin** establecer el `width` línea como los `sizes` normalmente lo hacen por sí mismos en AMP .

**Ejemplo** : uso del atributo `disable-inline-width`

En el siguiente ejemplo, el ancho del elemento `<amp-img>` no se ve afectado y los `sizes` solo se usan para seleccionar una de las fuentes del `srcset` .

[sourcecode:html]
<amp-img
  src="https://acme.org/image1.png"
  width="400"
  height="300"
  layout="responsive"
  sizes="(min-width: 320px) 320px, 100vw"
  disable-inline-width
>
</amp-img>
[/sourcecode]

### `heights` <a name="heights"></a>

Todos los elementos de AMP que admiten el diseño `responsive` también admiten el atributo de `heights` . El valor de este atributo es una expresión de tamaños basada en expresiones de medios similar al [atributo img tamaños](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) , pero con dos diferencias clave:

1. Se aplica a la altura, no al ancho del elemento.
2. Se permiten valores porcentuales, por ejemplo, `86%` . Si se usa un valor porcentual, indica el porcentaje del ancho del elemento.

Cuando el `heights` se especifica atributo junto con `width` y `height` , la `layout` está por defecto en `responsive` .

**Ejemplo** : uso del atributo `heights`

In the following example, the height of the image will default to 80% of the width, but if the viewport is wider than `500px`, the height is capped at `200px`. Because the `heights` attribute is specified along with `width` and `height`, the layout defaults to `responsive`.

[sourcecode:html]
<amp-img
  src="https://acme.org/image1.png"
  width="320"
  height="256"
  heights="(min-width:500px) 200px, 80%"
>
</amp-img>
[/sourcecode]

### `media` <a name="media"></a>

La mayoría de los elementos AMP admiten el atributo `media` . El valor de los `media` es una consulta de medios. Si la consulta no coincide, el elemento no se representa en absoluto y sus recursos y potencialmente sus recursos secundarios no se recuperarán. Si la ventana del navegador cambia de tamaño u orientación, las consultas de medios se vuelven a evaluar y los elementos se ocultan y se muestran en función de los nuevos resultados.

**Ejemplo** : uso del atributo `media`

En el siguiente ejemplo, tenemos 2 imágenes con consultas de medios mutuamente excluyentes. Dependiendo del ancho de la pantalla, se buscará y renderizará una de las dos imágenes. El atributo de `media` está disponible en todos los elementos de AMP, por lo que se puede usar con elementos que no son de imagen, como anuncios.

[sourcecode:html]
<amp-img
  media="(min-width: 650px)"
  src="wide.jpg"
  width="466"
  height="355"
  layout="responsive"
></amp-img>
<amp-img
  media="(max-width: 649px)"
  src="narrow.jpg"
  width="527"
  height="193"
  layout="responsive"
></amp-img>
[/sourcecode]

### `placeholder` <a name="placeholder"></a>

El atributo de `placeholder` se puede establecer en cualquier elemento HTML, no solo en elementos AMP. El atributo de `placeholder` indica que el elemento marcado con este atributo actúa como marcador de posición para el elemento principal de AMP. Si se especifica, un elemento de marcador de posición debe ser un elemento secundario directo del elemento AMP. De forma predeterminada, el marcador de posición se muestra inmediatamente para el elemento AMP, incluso si los recursos del elemento AMP no se han descargado o inicializado. Una vez que está listo, el elemento AMP generalmente oculta su marcador de posición y muestra el contenido. El comportamiento exacto con respecto al marcador de posición depende de la implementación del elemento.

[sourcecode:html]
<amp-anim src="animated.gif" width="466" height="355" layout="responsive">
  <amp-img placeholder src="preview.png" layout="fill"></amp-img>
</amp-anim>
[/sourcecode]

### `fallback` <a name="fallback"></a>

El atributo de `fallback` se puede establecer en cualquier elemento HTML, no solo en los elementos AMP. Una reserva es una convención que permite que el elemento comunique al lector que el navegador no es compatible con el elemento. Si se especifica, un elemento de reserva debe ser un elemento secundario directo del elemento AMP. El comportamiento exacto con respecto a la reserva depende de la implementación del elemento.

[sourcecode:html]
<amp-anim src="animated.gif" width="466" height="355" layout="responsive">
  <div fallback>Cannot play animated images on this device.</div>
</amp-anim>
[/sourcecode]

### `noloading` <a name="noloading"></a>

El atributo `noloading` indica si el "indicador de carga" debe desactivarse para este elemento. Muchos elementos AMP están permitidos para mostrar un "indicador de carga", que es una animación básica que muestra que el elemento aún no se ha cargado por completo. Los elementos pueden optar por no participar en este comportamiento agregando este atributo.

## (tl; dr) Resumen de requisitos y comportamientos de diseño<a name="tldr-summary-of-layout-requirements--behaviors"></a>

La siguiente tabla describe los parámetros aceptables, las clases de CSS y los estilos utilizados para el atributo de `layout` . Tenga en cuenta que:

1. Cualquier clase de CSS marcada con el prefijo `-amp-` y los elementos con el prefijo `i-amp-` se consideran internos de AMP y no se permite su uso en las hojas de estilo del usuario. Se muestran aquí simplemente con fines informativos.
2. Aunque el `width` y el `height` se especifican en la tabla según sea necesario, las reglas predeterminadas pueden aplicarse como es el caso de `amp-pixel` y `amp-audio` .

<table>
  <thead>
    <tr>
      <th width="21%">Diseño</th>
      <th width="20%">Anchura/<br> ¿Altura requerida?</th>
      <th width="20%">¿Define el tamaño?</th>
      <th width="20%">Elementos adicionales</th>
      <th width="19%">CSS "visualización"</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>container</code></td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>fill</code></td>
      <td>No</td>
      <td>Sí, el tamaño de los padres.</td>
      <td>No</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>fixed</code></td>
      <td>si</td>
      <td>Sí, especificado por <code>width</code> y <code>height</code> .</td>
      <td>No</td>
      <td><code>inline-block</code></td>
    </tr>
    <tr>
      <td><code>fixed-height</code></td>
      <td>solo <code>height</code> ; <code>width</code> puede ser <code>auto</code>
</td>
      <td>Sí, especificado por el contenedor principal y la <code>height</code> .</td>
      <td>No</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>flex-item</code></td>
      <td>No</td>
      <td>No</td>
      <td>Sí, según el contenedor principal.</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>intrinsic</code></td>
      <td>si</td>
      <td>Sí, según el contenedor principal y la relación de aspecto <code>width:height</code> .</td>
      <td>Sí, <code>i-amphtml-sizer</code> .</td>
      <td>
<code>block</code> (se comporta como un <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/Replaced_element" rel="nofollow">elemento reemplazado</a> )</td>
    </tr>
    <tr>
      <td><code>nodisplay</code></td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td><code>none</code></td>
    </tr>
    <tr>
      <td><code>responsive</code></td>
      <td>si</td>
      <td>Sí, según el contenedor principal y la relación de aspecto <code>width:height</code> .</td>
      <td>Sí, <code>i-amphtml-sizer</code> .</td>
      <td><code>block</code></td>
    </tr>
  </tbody>
</table>
