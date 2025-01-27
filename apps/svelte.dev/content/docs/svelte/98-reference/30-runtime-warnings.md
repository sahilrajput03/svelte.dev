---
title: 'Runtime warnings'
---

## Client warnings

<!-- This file is generated by scripts/process-messages/index.js. Do not edit! -->

### assignment_value_stale

```
Assignment to `%property%` property (%location%) will evaluate to the right-hand side, not the value of `%property%` following the assignment. This may result in unexpected behaviour.
```

Given a case like this...

```svelte
<script>
	let object = $state({ array: null });

	function add() {
		(object.array ??= []).push(object.array.length);
	}
</script>

<button onclick={add}>add</button>
<p>items: {JSON.stringify(object.items)}</p>
```

...the array being pushed to when the button is first clicked is the `[]` on the right-hand side of the assignment, but the resulting value of `object.array` is an empty state proxy. As a result, the pushed value will be discarded.

You can fix this by separating it into two statements:

```js
let object = { array: [0] };
// ---cut---
function add() {
	object.array ??= [];
	object.array.push(object.array.length);
}
```

### binding_property_non_reactive

```
`%binding%` is binding to a non-reactive property
```

```
`%binding%` (%location%) is binding to a non-reactive property
```

### console_log_state

```
Your `console.%method%` contained `$state` proxies. Consider using `$inspect(...)` or `$state.snapshot(...)` instead
```

When logging a [proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy), browser devtools will log the proxy itself rather than the value it represents. In the case of Svelte, the 'target' of a `$state` proxy might not resemble its current value, which can be confusing.

The easiest way to log a value as it changes over time is to use the [`$inspect`](https://svelte.dev/docs/svelte/$inspect) rune. Alternatively, to log things on a one-off basis (for example, inside an event handler) you can use [`$state.snapshot`](https://svelte.dev/docs/svelte/$state#$state.snapshot) to take a snapshot of the current value.

### event_handler_invalid

```
%handler% should be a function. Did you mean to %suggestion%?
```

### hydration_attribute_changed

```
The `%attribute%` attribute on `%html%` changed its value between server and client renders. The client value, `%value%`, will be ignored in favour of the server value
```

### hydration_html_changed

```
The value of an `{@html ...}` block changed between server and client renders. The client value will be ignored in favour of the server value
```

```
The value of an `{@html ...}` block %location% changed between server and client renders. The client value will be ignored in favour of the server value
```

### hydration_mismatch

```
Hydration failed because the initial UI does not match what was rendered on the server
```

```
Hydration failed because the initial UI does not match what was rendered on the server. The error occurred near %location%
```

### invalid_raw_snippet_render

```
The `render` function passed to `createRawSnippet` should return HTML for a single element
```

### legacy_recursive_reactive_block

```
Detected a migrated `$:` reactive block in `%filename%` that both accesses and updates the same reactive value. This may cause recursive updates when converted to an `$effect`.
```

### lifecycle_double_unmount

```
Tried to unmount a component that was not mounted
```

### ownership_invalid_binding

```
%parent% passed a value to %child% with `bind:`, but the value is owned by %owner%. Consider creating a binding between %owner% and %parent%
```

### ownership_invalid_mutation

```
Mutating a value outside the component that created it is strongly discouraged. Consider passing values to child components with `bind:`, or use a callback instead
```

```
%component% mutated a value owned by %owner%. This is strongly discouraged. Consider passing values to child components with `bind:`, or use a callback instead
```

### reactive_declaration_non_reactive_property

```
A `$:` statement (%location%) read reactive state that was not visible to the compiler. Updates to this state will not cause the statement to re-run. The behaviour of this code will change if you migrate it to runes mode
```

In legacy mode, a `$:` [reactive statement](https://svelte.dev/docs/svelte/legacy-reactive-assignments) re-runs when the state it _references_ changes. This is determined at compile time, by analysing the code.

In runes mode, effects and deriveds re-run when there are changes to the values that are read during the function's _execution_.

Often, the result is the same — for example these can be considered equivalent:

```js
let a = 1, b = 2, sum = 3;
// ---cut---
$: sum = a + b;
```

```js
let a = 1, b = 2;
// ---cut---
const sum = $derived(a + b);
```

In some cases — such as the one that triggered the above warning — they are _not_ the same:

```js
let a = 1, b = 2, sum = 3;
// ---cut---
const add = () => a + b;

// the compiler can't 'see' that `sum` depends on `a` and `b`, but
// they _would_ be read while executing the `$derived` version
$: sum = add();
```

Similarly, reactive properties of [deep state](https://svelte.dev/docs/svelte/$state#Deep-state) are not visible to the compiler. As such, changes to these properties will cause effects and deriveds to re-run but will _not_ cause `$:` statements to re-run.

When you [migrate this component](https://svelte.dev/docs/svelte/v5-migration-guide) to runes mode, the behaviour will change accordingly.

### state_proxy_equality_mismatch

```
Reactive `$state(...)` proxies and the values they proxy have different identities. Because of this, comparisons with `%operator%` will produce unexpected results
```

`$state(...)` creates a [proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) of the value it is passed. The proxy and the value have different identities, meaning equality checks will always return `false`:

```svelte
<script>
	let value = { foo: 'bar' };
	let proxy = $state(value);

	value === proxy; // always false
</script>
```

To resolve this, ensure you're comparing values where both values were created with `$state(...)`, or neither were. Note that `$state.raw(...)` will _not_ create a state proxy.


## Shared warnings

<!-- This file is generated by scripts/process-messages/index.js. Do not edit! -->

### dynamic_void_element_content

```
`<svelte:element this="%tag%">` is a void element — it cannot have content
```

### state_snapshot_uncloneable

```
Value cannot be cloned with `$state.snapshot` — the original value was returned
```

```
The following properties cannot be cloned with `$state.snapshot` — the return value contains the originals:

%properties%
```

