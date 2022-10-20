
#rule/structure
- [ ] Define the following structure for the application:
[How to Structure a Large Scale Vue.js Application](https://vueschool.io/articles/vuejs-tutorials/how-to-structure-a-large-scale-vue-js-application/)
- [ ] Create the `docs` folder in the root and add the rules below:

#rule/imports
- [ ] Standardization in the order of imports
	- Example:
```ts
// vue
import { ref } from 'vue';
// quasar
import { useQuasar } from 'quasar';
// utils
import { Events } from '@/utils/eventBus';
// helper
import { useGetters, useMethods } from '@/helpers/store';

// types
import type { PreferenceGetters } from '@/types/store/getters';

// others and components
import HelloWorld from '@/components/HelloWorld.vue';
import WorldHello from '@/components/WorldHello.vue';
```

#rule/script-setup
- [ ] Standardize setup definition
	- Example:
```ts
<script lang="ts" setup>
```

#rule/declaration-constants
- [ ] Standardize the declaration of constants
	- Example:
```ts
// imports

const props = defineProps<{
	modelValue: number;
}>();

const emits = defineEmits<{
	(e: 'update:modelValue', value: number): void;
}>();

const { useGetters } = useGetters<Getters>('getter');

const state = reactive({});

const val = ref();

const comp = computed();

// anothers
```

#rule/end-of-script-declarations
- [ ] Standardize end-of-script declarations
	- Example:
```ts
<script lang="ts" setup>
/* Body */

const myNameIs = () => console.log('John')
 
// LifeCycles 
onMounted(() => {});

// Call functions
myNameIs();
</script>
```

#rule/PascalCase
- [ ] Set default for file nodes:
	-   PascalCase (every word start with upper case)

#rule/class-name
- [ ] Standard for scss class naming. If your component calls `AppCarousel` you must define the main style class with the name `'app-carousel'` and style them all from this, example `'app-carousel__button'`. This makes it easier to find the component by inspecting the DOM.

#rule/async-await
- [ ] Always try to choose to use async and await instead of then.
	- Example
```ts
// Bad
const sendEmail = () => {
	forgotPassword(state.email.toLowerCase().trim())
	.then(() => {
		router.push({ name: 'Login', params: { forgotPassword: 'true' } });
	})

	.catch((error) => {
		$q.notify({
			message: `Request failed. ${error.response.data.detail}`,
			color: 'red',
			position: 'top'
		});
	});
};


// Good
const sendEmail = async () => {
	try {
		await forgotPassword(state.email.toLowerCase().trim())
		router.push({ name: 'Login', params: { forgotPassword: 'true' } });
	} catch (error) {
		$q.notify({
			message: `Request failed. ${error.response.data.detail}`,
			color: 'red',
			position: 'top'
		});
	}
};
```

#rule/ref-reactive
More infos [Reactivity API: Core](https://vuejs.org/api/reactivity-core.html)
- [ ] Use `ref()` when
	- it's a primitive _(for example `'string'`, `true`, `23`, etc)_
- [ ] User `reactive()` when
	- it's an object you don't need to reassign, and you want to avoid the overhead of `ref()`

#rule/rules-validation
- [ ] Dont use rules direct in template, create const for this
	- Example:
```ts
// Bad

<q-input
	outlined
	v-model="state.email"
	type="email"
	label="Enter your mail"
	lazy-rules
	:rules="[
		(val) => (val && val.length > 0) || 'Please type something'
	]"
>

// Good

<q-input
	outlined
	v-model="state.email"
	type="email"
	label="Enter your mail"
	lazy-rules
	:rules="myConst"
>

<script lang="ts" setup>

const myConst = [
	(val) => (val && val.length > 0) || 'Please type something',
	(val) => (val && val.length > 0) || 'Please type something'
]

<script>

```

#rule/dont-use-style-direct-in-template
- [ ] Dont use style direct in template, create class for this
```vue
// Bad

<q-icon
	name="mail"
	style="color: var(--color-primary)"
/>

// Good

<q-icon
	name="mail"
	class="app-my-componente__my-icon"
/>
```

#rule/use-rem
- [ ] Dont use PX like `font-size: 16px`. Use REM `font-size: 1rem`. If you need help check this site [PX to REM converter](https://nekocalc.com/px-to-rem-converter)

#rule/slot
```vue
// Bad
<template v-slot:preview>

// Good
<template #preview>
```

#rule/dont-use-async-await-with-then
- [ ] NEVER USE ASYNC AWAIT WITH THEN
	- Example:
```ts
// BAD
async () => {
	await login(state.email.trim(), state.password)
	.then((data: Data) => {})
}

// Good
async () => {
	const data = await login(state.email.trim(), state.password)
}
````

#rule/add-try-catch-in-services
- [ ] Add try catch in services, so the place that consumes some service will not need to
