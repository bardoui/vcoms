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

## JalaaliInput

Jalaali date input component.

**Note**: minimum and maximum check applied on v-model change and input blur on valid input only.

**Note**: modal value is a standard gregorian date string.

**Note**: min and max accept jalaali date string.

```vue
<script lang="ts">
// index.ts
import { vJalaaliInput } from "@bardoui/vcoms";
import { ref } from "vue";
const v = ref("");
</script>

<template>
  <v-jalaali-input separator="/" v-model="v" />
</template>
```

| Property  | Type     | Default | Description         |
| :-------- | :------- | :------ | :------------------ |
| separator | `String` | `"-"`   | separator string    |
| min       | `String` | `""`    | minimum jalaali date |
| max       | `String` | `""`    | maximum jalaali date |

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

## DataView

Data view container for lists rendering.

```vue
<template>
  <vDataView :data="records" :error="error">
    <template #error="{ error }">{{ error }} occurred!</template>
    <template #empty>No records exists</template>
    <template #default="{ records }">
      <!-- Show cards layout on mobile  -->
      <div class="my-card-layout show-on-mobile">
        <div v-for="(item, index) in records" :key="index">
          <p>{{ item.name }}</p>
        </div>
      </div>
      <!-- Show table on other devices  -->
      <table class="hide-on-mobile">
        <tbody>
          <tr v-for="(item, index) in records" :key="index">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
          </tr>
        </tbody>
      </table>
    </template>
  </vDataView>
</template>
```

Data View have three slots:

- **error**: content for render when error property not a falsy value (error passed as scoped-slot value).
- **empty**: content to render when no record exists.
- **default**: list views to render (records passed as scoped-slot value).

**Note**: Data view have no wrapper element and rendered directly in dom!

**Note**: for safe list rendering use records scoped-slot!

**Note**: you can use multiple list renderer for different device with default [Termeh](https://github.com/bardoui/termeh) visibility classes (like above example).

| Property | Type         | Default | Description   |
| :------- | :----------- | :------ | :------------ |
| data     | `Array|null` | `null`  | data records  |
| error    | `any`        | `null`  | error message |
