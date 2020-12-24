---
"$title": Cree un widget de interfaz de usuario con JavaScript personalizado
"$order": '101'
formats:
- sitios web
tutorial: cierto
author:
- morsssss
- CrystalOnScript
description: Para las experiencias web que requieren una gran cantidad de personalización, AMP ha creado amp-script, un componente que permite el uso de JavaScript arbitrario en su página AMP sin afectar el rendimiento de la página.
---

En este tutorial, aprenderá a usar `<amp-script>` , un componente que permite a los desarrolladores escribir JavaScript personalizado en AMP. Utilizará esto para crear un widget que verifique el contenido de un campo de entrada de contraseña y solo permita que se envíe cuando se cumplan ciertos requisitos. AMP ya proporciona esta funcionalidad con `<amp-form>` , pero `<amp-script>` le permitirá crear una experiencia personalizada.

## Lo que necesitarás

- Un navegador web moderno
- Conocimientos básicos de HTML, CSS y JavaScript.
- Ya sea:
    - un servidor web local y un editor de código como [SublimeText](https://www.sublimetext.com) o [VSCode](https://code.visualstudio.com/)
    - *o* [CodePen](https://codepen.io/) , [Glitch](https://glitch.com/) o un patio de juegos en línea similar

## Antecedentes

AMP tiene como objetivo hacer que los sitios web sean más rápidos y estables para los usuarios. Un exceso de JavaScript puede hacer que una página web sea lenta. Pero a veces es necesario crear funciones que los componentes de AMP no proporcionan. En tales casos, puede utilizar el componente [`<amp-script>`](../../../documentation/components/reference/amp-script.md) para escribir JavaScript personalizado.

¡Empecemos!

# Empezando

Para obtener el código de inicio, descargue o clone[este repositorio de github](https://github.com/ampproject/samples/tree/master/amp-script-tutorial) . Una vez hecho esto, `cd` al directorio que ha creado. Verás dos directorios: `starter_code` y `finished_code` . `finished_code` contiene lo que creará durante este tutorial. Así que no veamos eso todavía. En su lugar, `cd` en `starter_code` . Este contiene una página web que implementa nuestro formulario usando [`<amp-form>`](../../../documentation/components/reference/amp-form.md) solo, sin la ayuda de `<amp-script>` .

Para hacer este ejercicio, necesitará ejecutar un servidor web en su computadora. Si ya está haciendo esto, ¡estará listo! Si es así, dependiendo de su configuración, podrá acceder a la página web de inicio escribiendo en su navegador una URL como `http://localhost/amp-script-tutorial/starter_code/index.html` .

Alternativamente, puede configurar un servidor local rápido usando algo como [serve](https://www.npmjs.com/package/serve) , un servidor de contenido estático basado en [Node.js.](https://nodejs.org/) Si no ha instalado Node.js, descárguelo [aquí](https://nodejs.org/) . Una vez que Node esté instalado, escriba `npx serve` en su línea de comando. A continuación, puede acceder a su sitio web aquí:

`http://localhost:5000/`

También puede utilizar un área de juegos en línea como [Glitch](https://glitch.com/) o [CodePen](https://codepen.io/) . <a href="itch%5D(https://glitch.com/~grove-thankful-ragdoll" target="_blank">Esto</a> contiene el mismo código que el repositorio de github, ¡y puede comenzar allí si lo desea!

Una vez que haya hecho esto, verá nuestra página web de inicio:

{{image (&#39;/ static / img / docs / tutorials / custom-javascript-tutorial / starter-form.jpg&#39;, 600, 325, layout = &#39;intrinsic&#39;, alt = &#39;Formulario web con entradas de correo electrónico y contraseña&#39;, alinear = &#39;centro&#39;)}}

Abra `starter_code/index.html` en su editor de código favorito. Eche un vistazo al HTML de este formulario. Observe que la contraseña `<input>` contiene este atributo:

```html
on="tap:rules.show; input-debounced:rules.show"
```

Esto le dice a AMP que muestre las reglas `<div>` cuando el usuario toca o hace clic en la contraseña `<input>` , y también después de ingresar cualquier carácter allí. Preferiríamos usar el evento de `focus` , que también cubriría el caso en el que el usuario ingresa la entrada. Al menos en el momento en que se escribe este tutorial, AMP no transmite este evento, por lo que no tenemos esta opción. No se preocupe. ¡Estamos a punto de arreglar eso con `<amp-script>` !

La contraseña `<input>` contiene otro atributo interesante:

```html
pattern="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^a-z\d]).{8,}$"
```

Esta expresión regular combina un conjunto de expresiones regulares más pequeñas, cada una de las cuales expresa una de nuestras reglas de validación. AMP [no permitirá que se envíe el formulario](../../../documentation/components/reference/amp-form.md#verification) hasta que el contenido de la entrada coincida. Si el usuario lo intenta, verá un mensaje de error que proporciona algunos detalles:

{{image (&#39;/ static / img / docs / tutorials / custom-javascript-tutorial / starter-form-error.jpg&#39;, 600, 442, layout = &#39;intrinsic&#39;, alt = &#39;Formulario web que muestra un mensaje de error&#39;, align = &#39;centro&#39;)}}

[tip type="note"] Dado que el código que le proporcionamos no incluye un servicio web que maneje el envío de formularios, enviar el formulario no servirá de nada. ¡Por supuesto, puede agregar esta función a su propio código! [/tip]

Esta experiencia es aceptable, pero lamentablemente AMP no puede explicar cuál de nuestras reglas de verificación falló. No puede saberlo, ya que tuvimos que aplastar las reglas en una sola expresión regular.

¡Ahora, usemos `<amp-script>` para crear una experiencia más fácil de usar!

# Reconstruyéndolo con &lt;amp-script&gt;

Para usar `<amp-script>` , necesitamos importar su propio JavaScript. Abra `index.html` y agregue lo siguiente al `<head>` .

```html
&lt;head&gt;
 ...
  &lt;script async custom-element="amp-script" src="https://cdn.ampproject.org/v0/amp-script-0.1.js"&gt;&lt;/script&gt;
  ...
&lt;/head&gt;

```

`<amp-script>` nos permite escribir nuestro propio JavaScript en línea o en un archivo externo. En este ejercicio, escribiremos suficiente código para ameritar un archivo separado. Cree un nuevo directorio llamado `js` y agréguele un nuevo archivo llamado `validate.js` .

`<amp-script>` permite que JavaScript manipule sus hijos DOM, los elementos que encierra el componente. Copia esos hijos DOM en un DOM virtual y le da acceso a su código a este DOM virtual. En este ejercicio, queremos que nuestro JavaScript controle nuestro `<form>` y su contenido. Entonces, envolveremos el `<form>` en un componente `<amp-script>` , como este:

```html
&lt;amp-script src="js/validate.js" layout="fixed" sandbox="allow-forms" height="500" width="750"&gt;
  &lt;form method="post" action-xhr="#" target="_top" class="card"&gt;
    ...
  &lt;/form&gt;
&lt;/amp-script&gt;
```

Nuestro `<amp-script>` incluye el atributo `sandbox="allow-forms"` . Eso le dice a AMP que está bien que el script modifique el contenido del formulario.

Dado que AMP tiene como objetivo garantizar una experiencia de usuario rápida y visualmente estable, no permitirá que nuestro JavaScript realice cambios sin restricciones en el DOM en ningún momento. Su JavaScript puede realizar más cambios si el tamaño del componente `<amp-script>` no puede cambiar. También permite cambios más sustanciales después de la interacción del usuario. Puede encontrar detalles en [la documentación de referencia](../../../documentation/components/reference/amp-script.md) . Para este tutorial, basta con saber que hemos especificado un tipo de `layout` que no es `container` y que hemos utilizado atributos HTML para bloquear el tamaño del componente. Esto significa que cualquier manipulación del DOM está restringida a un área determinada de la página.

Si está utilizando la [extensión de Chrome del validador AMP](https://chrome.google.com/webstore/detail/amp-validator/nmoffdblmcmgeicmolmhobpoocbbmknc) , ahora verá un mensaje de error:

{{image (&#39;/ static / img / docs / tutorials / custom-javascript-tutorial /rative-url-error.png&#39;, 600, 177, layout = &#39;intrinsic&#39;, alt = &#39;Error sobre URL relativa&#39;, align = &#39;centro&#39;)}}

[tip type="note"] Si no tiene esta extensión, agregue `#development=1` a su URL y AMP generará errores de validación en su consola. [/tip]

¿Qué significa esto? Si su `<amp-script>` carga su JavaScript desde un archivo externo, AMP requiere que especifique una URL absoluta. Podríamos arreglar esto usando `http://localhost/js/validate.js` . Pero AMP también requiere el uso de [HTTPS](https://developers.google.com/web/fundamentals/security/encrypt-in-transit/why-https) . Por lo tanto, obtendríamos un error de validación y la configuración de SSL en nuestro servidor web local está fuera del alcance de este tutorial. Si quieres hacerlo, puedes seguir las instrucciones de [esta publicación](https://timonweb.com/posts/running-expressjs-server-over-https/) .

A continuación, podemos eliminar el atributo de `pattern` y su expresión regular de nuestro formulario: ¡ya no lo necesitaremos!

También vamos a eliminar el atributo `on` que se usa actualmente para decirle a AMP que muestre nuestras reglas de contraseña. Como se presagió anteriormente, en su lugar usaremos `<amp-script>` para capturar el evento de `focus` del navegador.

```html
pattern="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^a-z\d]).{8,}$"
on="tap:rules.show; input-debounced:rules.show"
```

Ahora asegurémonos de que nuestro `<amp-script>` esté funcionando. Abra el archivo `validate.js` que creó y agregue un mensaje de depuración:

```js
console.log("Hello, amp-script!");
```

Vaya a su navegador, abra la consola y vuelva a cargar la página. ¡Asegúrate de ver tu mensaje!

{{image (&#39;/ static / img / docs / tutorials / custom-javascript-tutorial / hello-amp-script.png&#39;, 600, 22, layout = &#39;intrinsic&#39;, alt = &#39;Mensaje de saludo amp-script en la consola&#39; , alinear = &#39;centro&#39;)}}

## ¿Dónde está mi JavaScript?

`<amp-script>` ejecuta su JavaScript en un Web Worker. Los Web Workers no pueden acceder al DOM directamente, por lo que `<amp-script>` le da al trabajador acceso a una copia virtual del DOM, que mantiene sincronizado con el DOM real. `<amp-script>` proporciona emulaciones de muchas API DOM comunes, casi todas las cuales puede usar en su JavaScript de la forma habitual.

Si en algún momento necesita depurar su script, puede establecer puntos de interrupción en JavaScript en un Web Worker de la misma manera que lo hace con cualquier JavaScript. Solo necesita saber dónde encontrarlo.

En Chrome DevTools, abra la pestaña "Fuentes". En la parte inferior verá una cadena hexadecimal larga como la que se muestra a continuación. Expanda eso, luego expanda el área "sin dominio" y verá su secuencia de comandos:

{{image (&#39;/ static / img / docs / tutorials / custom-javascript-tutorial / script-in-sources.png&#39;, 303, 277, layout = &#39;intrinsic&#39;, alt = &#39;amp-script JavaScript en el panel de fuentes de DevTools &#39;, align =&#39; centro &#39;)}}

# Añadiendo nuestro JavaScript

Ahora que sabemos que nuestro `<amp-script>` está funcionando, ¡escribamos algo de JavaScript!

Lo primero que queremos hacer es tomar los elementos DOM con los que trabajaremos y guardarlos en globales. Nuestro código utilizará la entrada de la contraseña, el botón de envío y el área que muestra las reglas de la contraseña. Agregue estas tres declaraciones para `validate.js` :

```js
const passwordBox = document.getElementById("passwordBox");
const submitButton = document.getElementById("submitButton");
const rulesArea = document.getElementById("rules");
```

Tenga en cuenta que podemos usar métodos de API DOM regulares como `getElementById()` . Aunque nuestro código se ejecuta en un trabajador y los trabajadores carecen de acceso directo al DOM, `<amp-script>` proporciona una copia virtual del DOM y emula algunas API comunes, enumeradas [aquí](https://github.com/ampproject/worker-dom/blob/main/web_compat_table.md) . Estas API nos brindan suficientes herramientas para cubrir la mayoría de los casos de uso. Pero es importante tener en cuenta que solo se admite un subconjunto de la API DOM. De lo contrario, el JavaScript incluido con `<amp-script>` sería enorme, ¡anulando los beneficios de rendimiento de AMP!

Necesitamos agregar estas identificaciones a dos de los elementos. Abra `index.html` , busque la contraseña `<input>` y el `<button>` enviar, y agregue las identificaciones. Agregue un atributo `disabled` al `<button>` enviar también, para evitar que el usuario haga clic en él hasta que lo deseemos.

```html
&lt;input type=password
       id="passwordBox"

...

&lt;button type="submit" id="submitButton" tabindex="3" disabled&gt;Submit&lt;/button&gt;
```

Vuelve a cargar la página. Puede verificar que estos valores globales se establecieron correctamente al verificar en la Consola, tal como lo haría con JavaScript no trabajador:

{{image (&#39;/ static / img / docs / tutorials / custom-javascript-tutorial / global-set.png&#39;, 563, 38, layout = &#39;intrinsic&#39;, alt = &#39;El mensaje de la consola que muestra submitButton está configurado&#39;, align = &#39;centro&#39;)}}

También agregaremos identificaciones a cada `<li>` en `<div id="rules">` . Cada uno de estos contiene una regla individual cuyo color queremos controlar. Y eliminaremos cada instancia de `class="invalid"` . ¡Nuestro nuevo JavaScript lo agregará cuando sea necesario!

```html
&lt;ul&gt;
  &lt;li id="lower"&gt;Lowercase letter&lt;/li&gt;
  &lt;li id="upper"&gt;Capital letter&lt;/li&gt;
  &lt;li id="digit"&gt;Digit&lt;/li&gt;
  &lt;li id="special"&gt;Special character (@$!%*?&amp;)&lt;/li&gt;
  &lt;li id="eight"&gt;At least 8 characters long&lt;/li&gt;
&lt;/ul&gt;
```

## Implementando nuestras comprobaciones de contraseña en JavaScript

A continuación, descomprimiremos las expresiones regulares de nuestro atributo de `pattern` . Cada expresión regular representa una de nuestras reglas. Agreguemos un mapa de objetos al final de `validate.js` que asocie cada regla con el criterio que verifica.

```js
const checkRegexes = {
  lower: /[a-z]/,
  upper: /[A-Z]/,
  digit: /\d/,
  special: /[^a-zA-Z\d]/i,
  eight: /.{8}/
};
```

Con esos globales configurados, estamos listos para escribir la lógica que verifica la contraseña y ajusta la interfaz de usuario en consecuencia. Pondremos nuestra lógica dentro de una función llamada `initCheckPassword` que toma un solo argumento: el elemento DOM de la contraseña `<input>` . Este enfoque oculta convenientemente el elemento DOM en un cierre.

```js
function initCheckPassword(element) {

}
```

A continuación, `initCheckPassword` con las funciones y asignaciones de escucha de eventos que necesitaremos. En primer lugar, agregue una pequeña función que haga que una regla individual `<li>` vuelva verde si la regla se aprueba, y otra que la vuelva roja cuando falle.

```js
function initCheckPassword(el) {
  const checkPass = (el) =&gt; {
    el.classList.remove("invalid");
    el.classList.add("valid");
  };

  const checkFail = (el) =&gt; {
    el.classList.remove("valid");
    el.classList.add("invalid");
  };
};
```

Hagamos que esas clases `valid` e `invalid` realmente se conviertan en texto verde o rojo. Vuelva a `index.html` y agregue estas dos reglas a la etiqueta `<style amp-custom>` :

```css
li.valid {
  color: #2d7b1f;
}

li.invalid {
  color:#c11136;
}
```

Ahora estamos listos para agregar la lógica que verifica el contenido de la contraseña `<input>` con nuestras reglas. Agregue una nueva función llamada `checkPassword()` a `initCheckPassword()` , justo antes de la llave de cierre:

```js
const checkPassword = () =&gt; {
  const password = element.value;
  let failed = false;

  for (const check in checkRegexes) {
    let li = document.getElementById(check);

    if (password.match(checkRegexes[check])) {
      checkPass(li);
    } else {
      checkFail(li);
      failed = true;
    }
  }

  if (!failed) {
    submitButton.removeAttribute("disabled");
  }
};
```

Esta función hace lo siguiente:

1. Toma el contenido de la contraseña `<input>` .
2. Crea un indicador llamado `failed` , inicializado en `false` .
3. Repite cada una de nuestras expresiones regulares y las prueba con la contraseña:
    - Si la contraseña falla en una prueba, llame a `checkFail()` para que la regla correspondiente se vuelva roja. Además, el conjunto `failed` en `true` .
    - Si la contraseña pasa una prueba, llame a `checkPass()` para que la regla correspondiente se vuelva verde.
4. Finalmente, si no falla ninguna regla, la contraseña es válida y habilitamos el botón Enviar.

Todo lo que necesitamos ahora son un par de oyentes de eventos. ¿Recuerda que no pudimos utilizar el evento de `focus` en AMP? En `<amp-script>` , podemos. Siempre que la contraseña `<input>` reciba el evento de `focus` , mostraremos las reglas. Y siempre que el usuario presione una tecla en esa entrada, llamaremos `checkPassword()` .

Agregue estos dos detectores de eventos al final de `initCheckPassword()` , directamente antes de la llave de cierre:

```js
element.addEventListener("focus", () =&gt; rulesArea.removeAttribute("hidden"));
element.addEventListener("keyup", checkPassword);
```

Finalmente, al final de `validate.js` , agregue una línea que inicialice `initCheckPassword` con el elemento DOM de contraseña `<input>` :

```js
initCheckPassword(passwordBox);
```

¡Nuestra lógica ahora está completa! Cuando la contraseña coincida con todos nuestros criterios, todas las reglas serán verdes y nuestro botón de envío estará habilitado. Ahora debería poder tener una interacción como esta:

<figure class="alignment-wrapper margin-">
  <amp-video width="762" height="564" layout="responsive" autoplay loop noaudio>
    <source src="/static/img/docs/tutorials/custom-javascript-tutorial/finished-project.mp4" type="video/mp4">
    <source src="/static/img/docs/tutorials/custom-javascript-tutorial/finished-project.webm" type="video/webm">
  </amp-video>
</figure>

Si se atasca, siempre puede consultar el código de trabajo en el directorio `finished_code` .

# ¡Felicidades!

Ha aprendido a usar `<amp-script>` para escribir su propio JavaScript en AMP. ¡Ha logrado mejorar el componente `<amp-form>` con su propia lógica personalizada y funciones de interfaz de usuario! ¡Siéntete libre de agregar más funciones a tu nueva página! Y, para obtener más información sobre `<amp-script>` , consulte [la documentación de referencia](../../../documentation/components/reference/amp-script.md) .
