---
title: svelte/reactivity
---

Svelte provides reactive versions of various built-ins like `SvelteMap`, `SvelteSet` and `SvelteURL`. These can be imported from `svelte/reactivity` and used just like their native counterparts.

```svelte
<script>
	import { SvelteURL } from 'svelte/reactivity';

	const url = new SvelteURL('https://example.com/path');
</script>

<!-- changes to these... -->
<input bind:value={url.protocol} />
<input bind:value={url.hostname} />
<input bind:value={url.pathname} />

<hr />

<!-- will update `href` and vice versa -->
<input bind:value={url.href} />
```



```js
// @noErrors
import { createSubscriber } from 'svelte/reactivity';
```

## createSubscriber

Returns a function that, when invoked in a reactive context, calls the `start` function once,
and calls the `stop` function returned from `start` when all reactive contexts it's called in
are destroyed. This is useful for creating a notifier that starts and stops when the
"subscriber" count goes from 0 to 1 and back to 0. Call the `update` function passed to the
`start` function to notify subscribers of an update.

Usage example (reimplementing `MediaQuery`):

```js
// @errors: 7031
import { createSubscriber, on } from 'svelte/reactivity';

export class MediaQuery {
	#query;
	#subscribe = createSubscriber((update) => {
		// add an event listener to update all subscribers when the match changes
		return on(this.#query, 'change', update);
	});

	get current() {
		// If the `current` property is accessed in a reactive context, start a new
		// subscription if there isn't one already. The subscription will under the
		// hood ensure that whatever reactive context reads `current` will rerun when
		// the match changes
		this.#subscribe();
		// Return the current state of the query
		return this.#query.matches;
	}

	constructor(query) {
		this.#query = window.matchMedia(`(${query})`);
	}
}
```

<div class="ts-block">

```dts
function createSubscriber(
	start: (update: () => void) => (() => void) | void
): () => void;
```

</div>



## MediaQuery

Creates a media query and provides a `current` property that reflects whether or not it matches.

<div class="ts-block">

```dts
class MediaQuery {/*…*/}
```

<div class="ts-block-property">

```dts
constructor(query: string, matches?: boolean | undefined);
```

<div class="ts-block-property-details">

<div class="ts-block-property-bullets">

- `query` A media query string
- `matches` Fallback value for the server

</div>

</div>
</div>

<div class="ts-block-property">

```dts
get current(): boolean;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
#private;
```

<div class="ts-block-property-details"></div>
</div></div>

## SvelteDate

<div class="ts-block">

```dts
class SvelteDate extends Date {/*…*/}
```

<div class="ts-block-property">

```dts
constructor(...params: any[]);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
#private;
```

<div class="ts-block-property-details"></div>
</div></div>

## SvelteMap

<div class="ts-block">

```dts
class SvelteMap<K, V> extends Map<K, V> {/*…*/}
```

<div class="ts-block-property">

```dts
constructor(value?: Iterable<readonly [K, V]> | null | undefined);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
set(key: K, value: V): this;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
#private;
```

<div class="ts-block-property-details"></div>
</div></div>

## SvelteSet

<div class="ts-block">

```dts
class SvelteSet<T> extends Set<T> {/*…*/}
```

<div class="ts-block-property">

```dts
constructor(value?: Iterable<T> | null | undefined);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
add(value: T): this;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
#private;
```

<div class="ts-block-property-details"></div>
</div></div>

## SvelteURL

<div class="ts-block">

```dts
class SvelteURL extends URL {/*…*/}
```

<div class="ts-block-property">

```dts
get searchParams(): SvelteURLSearchParams;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
#private;
```

<div class="ts-block-property-details"></div>
</div></div>

## SvelteURLSearchParams

<div class="ts-block">

```dts
class SvelteURLSearchParams extends URLSearchParams {/*…*/}
```

<div class="ts-block-property">

```dts
[REPLACE](params: URLSearchParams): void;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
#private;
```

<div class="ts-block-property-details"></div>
</div></div>


