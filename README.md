# vComs

Vue 3 components.

## Installation

You must import and register components manually.

### CDN

This package published as `vComs` module in umd.

```html
<script src="https://unpkg.com/@bardoui/vcoms"></script>
```

### NPM

```bash
npm i @bardoui/vcoms
```

## Numeric

Numeric input component.

```vue
<script lang="ts">
// index.ts
import { vNumericInput } from "@bardoui/vcoms";
import { ref } from "vue";
const v = ref("");
</script>

<template>
  <v-numeric-input prefix="$ " separator="," :min="-10" :decimal="2" v-model="v">
</template>
```

| Property  | Type     | Default                   | Description                 |
| :-------- | :------- | :------------------------ | :-------------------------- |
| prefix    | `String` | `""`                      | Number prefix               |
| suffix    | `String` | `""`                      | Number suffix               |
| separator | `String` | `","`                     | separator string            |
| decimal   | `number` | `0`                       | max allowed fraction number |
| min       | `number` | `0`                       | minimum number              |
| max       | `number` | `Number.MAX_SAFE_INTEGER` | maximum number              |

**Caution**: when passed min and max property, value set to min/max if goes out of range.

| Event  | Type                  | Description                           |
| :----- | :-------------------- | :------------------------------------ |
| format | `(v: string) => void` | pass formatted number on value update |

## MaskInput

Masked input component.

```vue
<script lang="ts">
// index.ts
import { vMaskInput } from "@bardoui/vcoms";
import { ref } from "vue";
const v = ref("");
</script>

<template>
  <v-mask-input guide="_" mask="####-###.##" v-model="v">
</template>
```

| Property | Type                      | Default | Description                                                                                                                                         |
| :------- | :------------------------ | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| options  | `Options`                 | `{}`    | default options. see [Text Mask Core Options](https://github.com/text-mask/text-mask/blob/master/componentDocumentation.md#readme) for more details |
| mask     | `string\|array\|function` |         | mask. see [vMask](https://github.com/bardoui/vmask) for more details                                                                                |
| guide    | `String`                  | `""`    | guide character to show as placeholder                                                                                                              |

## JalaliInput

Jalali date input component.

**Note**: minimum and maximum check applied on v-model change and input blur on valid input only.

```vue
<script lang="ts">
// index.ts
import { vJalaliInput } from "@bardoui/vcoms";
import { ref } from "vue";
const v = ref("");
</script>

<template>
  <v-jalali-input separator="/" v-model="v" />
</template>
```

| Property  | Type     | Default | Description         |
| :-------- | :------- | :------ | :------------------ |
| separator | `String` | `"-"`   | separator string    |
| min       | `String` | `""`    | minimum jalali date |
| max       | `String` | `""`    | maximum jalali date |

## NumberAnim

Animate number. Default slot contains two value:

- **value** _(number)_: animated value.
- **formatted** _(string)_: formatted animated value.

```vue
<script lang="ts">
// index.ts
import { vNumberAnim } from "@bardoui/vcoms";
import { ref } from "vue";
const v = ref(0);
</script>

<template>
  <v-number-anim
    :value="v_num"
    :decimals="3"
    separator="/"
    easing="linear"
    :duration="500"
  >
    <template #="{ formatted, value }">
      <h1>{{ formatted }}</h1>
      <p>{{ value }}</p>
    </template>
  </v-number-anim>
</template>
```

| Property  | Type     | Default                                 | Description                          |
| :-------- | :------- | :-------------------------------------- | :----------------------------------- |
| value     | `Number` |                                         | number to animate                    |
| decimals  | `Number` | `0`                                     | decimal places limit                 |
| separator | `String` | `","`                                   | number format separator              |
| easing    | `String` | `"cubicBezier(0.25, 0.46, 0.45, 0.94)"` | animation easing                     |
| duration  | `Number` | `750`                                   | animation duration in (milliseconds) |
