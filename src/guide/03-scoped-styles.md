---
title: Scoped styles
---

One of Svelte's key tenets is that components should be self-contained and reusable in different contexts. Because of that, it has a mechanism for *scoping* your CSS, so that you don't accidentally clobber other selectors on the page.

### Adding styles

Your component template can have a `<style>` tag, like so:

```html
<div class='foo'>
	Big red Comic Sans
</div>

<style>
	.foo {
		color: red;
		font-size: 2em;
		font-family: 'Comic Sans MS';
	}
</style>
```


### How it works

Open the example above in the REPL and inspect the element to see what has happened – Svelte has added a `svelte-[uniqueid]` attribute to the element, and transformed the CSS selector accordingly. Since no other element on the page can share that selector, anything else on the page with `class="foo"` will be unaffected by our styles.

This is vastly simpler than achieving the same effect via [Shadow DOM](http://caniuse.com/#search=shadow%20dom) and works everywhere without polyfills.

> Svelte will dynamically add a `<style>` tag containing your scoped styles to the document head at runtime. Dynamically adding styles may be impossible if your site has a [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP). If that's the case, you can use scoped styles by [server-rendering your CSS](#rendering-css) and using the `css: false` compiler option (or `--no-css` with the CLI).


### Cascading rules

The usual cascading mechanism still applies – any global `.foo` styles would still be applied, and if our template had [nested components](#nested-components) with `class="foo"` elements, they would inherit our styles.

> Scoped styles are *not* dynamic – they are shared between all instances of a component. In other words you can't use `{{tags}}` inside your CSS.
