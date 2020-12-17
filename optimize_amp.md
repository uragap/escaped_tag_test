With AMP there are a few things that you can do to optimize your font loading ([most of them are actually not specific to AMP](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/webfont-optimization)):

- If possible, use [font-display: optional](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display): This will only use the font if it's already in the cache, and falls back to the system font if your custom font has not been loaded yet.
- Optimize your web fonts (for example, serve custom fonts using WOFF2).
- Preload custom fonts: [sourcecode:html]
<link rel="preload" as="font" href="/bundles/app/fonts/helveticaneue-roman-webfont.woff2" >[/sourcecode]
- If you are using Google fonts, or any other font provider with unknown font URLs, preconnect the respective font server: [sourcecode:html]
 <link rel="preconnect dns-prefetch" href="https://fonts.gstatic.com/" crossorigin>
[/sourcecode]

Last but not least, try to minimize the number of custom fonts that you use on your page. If you can, use the system fonts instead of custom fonts because system fonts make your website match the user's operating system, and it helps to avoid loading more resources.
