At this point, the user can add a comment using the
```
[sourcecode:html]
<form amp-access="loggedIn" amp-access-hide method="post" action-xhr="<%host%>/samples_templates/comment_section/submit-comment-xhr" target="_top">
[/sourcecode]
We specify a POST method and a XHR action, as non XHR actions are not allowed with POST methods in AMP.
```
Because this is a demo, we are not persisting comments, so it’s only possible to add one comment at the time
