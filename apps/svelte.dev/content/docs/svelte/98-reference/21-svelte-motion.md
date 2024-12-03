---
title: svelte/motion
---



```js
// @noErrors
import { Spring, spring, tweened } from 'svelte/motion';
```

## Spring

A wrapper for a value that behaves in a spring-like fashion. Changes to `spring.target` will cause `spring.current` to
move towards it over time, taking account of the `spring.stiffness` and `spring.damping` parameters.

```svelte
<script>
	import { Spring } from 'svelte/motion';

	const spring = new Spring({ x: 0, y: 0 });
</script>

<input type="range" bind:value={spring.target} />
<input type="range" bind:value={spring.current} disabled />
```

<div class="ts-block">

```dts
class Spring<T> {/*…*/}
```

<div class="ts-block-property">

```dts
static of<U>(fn: () => U, options?: {
	stiffness?: number;
	damping?: number;
	precision?: number;
} | undefined): Spring<U>;
```

<div class="ts-block-property-details">

Create a spring whose value is bound to the return value of `fn`. This must be called
inside an effect root (for example, during component initialisation).

```svelte
<script>
	import { Spring } from 'svelte/motion';

	let { number } = $props();

	const spring = Spring.of(() => number);
</script>
```

</div>
</div>

<div class="ts-block-property">

```dts
constructor(value: T, options?: {
	stiffness?: number;
	damping?: number;
	precision?: number;
} | undefined);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
set(value: T, options?: {
	instant?: boolean;
	preserveMomentum?: number;
} | undefined): Promise<any>;
```

<div class="ts-block-property-details">

Sets `spring.target` to `value` and returns a `Promise` if and when `spring.current` catches up to it.

If `options.instant` is `true`, `spring.current` immediately matches `spring.target`.

If `options.preserveMomentum` is provided, the spring will continue on its current trajectory for
the specified number of seconds. This is useful for things like 'fling' gestures.

</div>
</div>

<div class="ts-block-property">

```dts
get current(): T;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
set damping(v: number);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
get damping(): number;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
set precision(v: number);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
get precision(): number;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
set stiffness(v: number);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
get stiffness(): number;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
set target(v: T);
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
get target(): T;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
#private;
```

<div class="ts-block-property-details"></div>
</div></div>



## spring

<blockquote class="tag deprecated">

Use [`Spring`](https://svelte.dev/docs/svelte/svelte-motion#Spring) instead

</blockquote>

The spring function in Svelte creates a store whose value is animated, with a motion that simulates the behavior of a spring. This means when the value changes, instead of transitioning at a steady rate, it "bounces" like a spring would, depending on the physics parameters provided. This adds a level of realism to the transitions and can enhance the user experience.

<div class="ts-block">

```dts
function spring<T = any>(
	value?: T | undefined,
	opts?: SpringOpts | undefined
): SpringStore<T>;
```

</div>



## tweened

A tweened store in Svelte is a special type of store that provides smooth transitions between state values over time.

<div class="ts-block">

```dts
function tweened<T>(
	value?: T | undefined,
	defaults?: TweenedOptions<T> | undefined
): TweenedStore<T>;
```

</div>



## SpringStore

<div class="ts-block">

```dts
interface SpringStore<T> extends Readable<T> {/*…*/}
```

<div class="ts-block-property">

```dts
set: (new_value: T, opts?: SpringUpdateOpts) => Promise<void>;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
update: (fn: Updater<T>, opts?: SpringUpdateOpts) => Promise<void>;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
precision: number;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
damping: number;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
stiffness: number;
```

<div class="ts-block-property-details"></div>
</div></div>

## TweenedStore

<div class="ts-block">

```dts
interface TweenedStore<T> extends Readable<T> {/*…*/}
```

<div class="ts-block-property">

```dts
set(value: T, opts?: TweenedOptions<T>): Promise<void>;
```

<div class="ts-block-property-details"></div>
</div>

<div class="ts-block-property">

```dts
update(updater: Updater<T>, opts?: TweenedOptions<T>): Promise<void>;
```

<div class="ts-block-property-details"></div>
</div></div>


