---
title: $app/server
---



```js
// @noErrors
import { read, upgrade } from '$app/server';
```

## read

Read the contents of an imported asset from the filesystem

```js
// @errors: 7031
import { read } from '$app/server';
import somefile from './somefile.txt';

const asset = read(somefile);
const text = await asset.text();
```

<div class="ts-block">

```dts
function read(asset: string): Response;
```

</div>



## upgrade

Read the contents of an imported asset from the filesystem

```js
// @errors: 7031
import { upgrade } from '$app/server';
import somefile from './somefile.txt';

const asset = read(somefile);
const text = await asset.text();
```

<div class="ts-block">

```dts
function upgrade(): void;
```

</div>




