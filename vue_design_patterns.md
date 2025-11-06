# Complete Vue.js Design Patterns Guide

A comprehensive guide combining Vue.js-specific patterns, architectural decisions, and practical JavaScript/TypeScript code examples. This guide covers everything from basic Vue.js patterns to advanced application architectures.

## Table of Contents

### Vue.js-Specific Patterns
- [1. Component Patterns](#1-component-patterns)
- [2. Composition API Patterns](#2-composition-api-patterns)
- [3. State Management Patterns](#3-state-management-patterns)
- [4. Routing Patterns](#4-routing-patterns)
- [5. Form Patterns](#5-form-patterns)
- [6. Performance Patterns](#6-performance-patterns)
- [7. Testing Patterns](#7-testing-patterns)
- [8. Directive Patterns](#8-directive-patterns)

### Advanced Patterns
- [9. Plugin Patterns](#9-plugin-patterns)
- [10. Provide/Inject Patterns](#10-provideinject-patterns)
- [11. Slot Patterns](#11-slot-patterns)
- [12. Custom Hooks (Composables) Patterns](#12-custom-hooks-composables-patterns)

### Decision Tools
- [Vue.js Pattern Decision Tree](#vuejs-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## Vue.js Pattern Decision Tree

### When building Vue.js applications...

```
┌─────────────────────────────────────────────────────────────┐
│                   VUE.JS CHALLENGE                            │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   COMPONENT   │ │ STATE      │ │ ARCHITECTURE│
        │   PATTERNS    │ │ MANAGEMENT │ │ PATTERNS    │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ OPTIONS API   │ │ PINIA      │ │ COMPOSITION │
        │ COMPOSITION   │ │ VUEX        │ │ API         │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ COMPOSABLES   │ │ PROVIDE/   │ │ SLOTS       │
        │ CUSTOM HOOKS  │ │ INJECT     │ │ TELEPORT    │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Component Patterns

### Options API Pattern
**Description:** Traditional Vue component pattern using options object.

```vue
<template>
  <div class="user-card">
    <h3>{{ user.name }}</h3>
    <p>{{ user.email }}</p>
    <button @click="handleClick">Click Me</button>
  </div>
</template>

<script>
export default {
  name: 'UserCard',
  props: {
    user: {
      type: Object,
      required: true,
      validator: (value) => {
        return value.name && value.email
      }
    }
  },
  data() {
    return {
      clicked: false
    }
  },
  computed: {
    displayName() {
      return this.user.name.toUpperCase()
    }
  },
  methods: {
    handleClick() {
      this.clicked = true
      this.$emit('user-clicked', this.user)
    }
  },
  watch: {
    user: {
      handler(newVal, oldVal) {
        console.log('User changed', newVal, oldVal)
      },
      immediate: true,
      deep: true
    }
  },
  mounted() {
    console.log('Component mounted')
  },
  beforeUnmount() {
    console.log('Component will be unmounted')
  }
}
</script>
```

### Composition API Pattern
**Description:** Modern Vue component pattern using composition API.

```vue
<template>
  <div class="user-card">
    <h3>{{ displayName }}</h3>
    <p>{{ user.email }}</p>
    <button @click="handleClick">Click Me</button>
  </div>
</template>

<script setup>
import { computed, ref, watch, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  user: {
    type: Object,
    required: true,
    validator: (value) => {
      return value.name && value.email
    }
  }
})

const emit = defineEmits(['user-clicked'])

const clicked = ref(false)

const displayName = computed(() => {
  return props.user.name.toUpperCase()
})

const handleClick = () => {
  clicked.value = true
  emit('user-clicked', props.user)
}

watch(() => props.user, (newVal, oldVal) => {
  console.log('User changed', newVal, oldVal)
}, { immediate: true, deep: true })

onMounted(() => {
  console.log('Component mounted')
})

onBeforeUnmount(() => {
  console.log('Component will be unmounted')
})
</script>
```

### Component Communication Pattern
**Description:** Various ways components communicate in Vue.

```vue
<!-- Parent Component -->
<template>
  <div>
    <ChildComponent
      :message="parentMessage"
      @child-event="handleChildEvent"
    />
    <SiblingComponent ref="siblingRef" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'
import SiblingComponent from './SiblingComponent.vue'

const parentMessage = ref('Hello from parent')
const siblingRef = ref(null)

const handleChildEvent = (data) => {
  console.log('Received from child:', data)
  // Communicate with sibling
  if (siblingRef.value) {
    siblingRef.value.updateData(data)
  }
}
</script>

<!-- Child Component -->
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="notifyParent">Notify Parent</button>
  </div>
</template>

<script setup>
defineProps({
  message: String
})

const emit = defineEmits(['child-event'])

const notifyParent = () => {
  emit('child-event', { data: 'Hello from child' })
}
</script>
```

---

## 2. Composition API Patterns

### Reactive State Pattern
**Description:** Use reactive refs and reactive objects for state management.

```vue
<script setup>
import { ref, reactive, computed, watch } from 'vue'

// Reactive primitives
const count = ref(0)
const name = ref('John')

// Reactive object
const user = reactive({
  firstName: 'John',
  lastName: 'Doe'
})

// Computed properties
const fullName = computed(() => {
  return `${user.firstName} ${user.lastName}`
})

const doubledCount = computed(() => count.value * 2)

// Watchers
watch(count, (newVal, oldVal) => {
  console.log(`Count changed from ${oldVal} to ${newVal}`)
})

watch(() => user.firstName, (newVal) => {
  console.log(`First name changed to ${newVal}`)
})

// Methods
const increment = () => {
  count.value++
}

const updateName = () => {
  user.firstName = 'Jane'
}
</script>
```

### Lifecycle Hooks Pattern
**Description:** Use composition API lifecycle hooks.

```vue
<script setup>
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted
} from 'vue'

onBeforeMount(() => {
  console.log('Before mount')
})

onMounted(() => {
  console.log('Mounted')
  // Setup: fetch data, setup listeners
})

onBeforeUpdate(() => {
  console.log('Before update')
})

onUpdated(() => {
  console.log('Updated')
})

onBeforeUnmount(() => {
  console.log('Before unmount')
  // Cleanup: remove listeners
})

onUnmounted(() => {
  console.log('Unmounted')
})
</script>
```

---

## 3. State Management Patterns

### Pinia Pattern (Recommended)
**Description:** Modern state management solution for Vue 3.

```javascript
// stores/user.js
import { defineStore } from 'pinia'

export const useUserStore = defineStore('user', {
  state: () => ({
    user: null,
    loading: false,
    error: null
  }),
  
  getters: {
    isAuthenticated: (state) => state.user !== null,
    userName: (state) => state.user?.name || 'Guest'
  },
  
  actions: {
    async fetchUser(userId) {
      this.loading = true
      this.error = null
      try {
        const response = await fetch(`/api/users/${userId}`)
        this.user = await response.json()
      } catch (error) {
        this.error = error.message
      } finally {
        this.loading = false
      }
    },
    
    logout() {
      this.user = null
    }
  }
})

// Usage in component
<script setup>
import { useUserStore } from '@/stores/user'

const userStore = useUserStore()

// Access state
console.log(userStore.user)

// Access getters
console.log(userStore.isAuthenticated)
console.log(userStore.userName)

// Call actions
userStore.fetchUser(1)
</script>
```

### Vuex Pattern (Legacy)
**Description:** State management for Vue 2 or Vue 3.

```javascript
// store/index.js
import { createStore } from 'vuex'

export default createStore({
  state: {
    user: null,
    loading: false
  },
  
  getters: {
    isAuthenticated: (state) => state.user !== null,
    userName: (state) => state.user?.name || 'Guest'
  },
  
  mutations: {
    SET_USER(state, user) {
      state.user = user
    },
    SET_LOADING(state, loading) {
      state.loading = loading
    }
  },
  
  actions: {
    async fetchUser({ commit }, userId) {
      commit('SET_LOADING', true)
      try {
        const response = await fetch(`/api/users/${userId}`)
        const user = await response.json()
        commit('SET_USER', user)
      } finally {
        commit('SET_LOADING', false)
      }
    }
  }
})

// Usage in component
<script setup>
import { useStore } from 'vuex'
import { computed } from 'vue'

const store = useStore()

const user = computed(() => store.state.user)
const isAuthenticated = computed(() => store.getters.isAuthenticated)

const fetchUser = () => {
  store.dispatch('fetchUser', 1)
}
</script>
```

### Provide/Inject Pattern
**Description:** Share state across component tree without prop drilling.

```vue
<!-- Parent Component -->
<script setup>
import { provide, ref } from 'vue'

const theme = ref('light')
const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
}

provide('theme', {
  theme,
  toggleTheme
})
</script>

<!-- Child Component (any level deep) -->
<script setup>
import { inject } from 'vue'

const { theme, toggleTheme } = inject('theme', {
  theme: ref('light'),
  toggleTheme: () => {}
})
</script>
```

---

## 4. Routing Patterns

### Vue Router Pattern
**Description:** Use Vue Router for navigation.

```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'
import Home from '@/views/Home.vue'
import UserProfile from '@/views/UserProfile.vue'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/user/:id',
    name: 'UserProfile',
    component: UserProfile,
    props: true, // Pass route params as props
    meta: {
      requiresAuth: true
    }
  },
  {
    path: '/about',
    name: 'About',
    component: () => import('@/views/About.vue') // Lazy loading
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

// Navigation guard
router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isAuthenticated()) {
    next('/login')
  } else {
    next()
  }
})

export default router
```

### Programmatic Navigation Pattern
**Description:** Navigate programmatically in components.

```vue
<script setup>
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

// Navigate to route
const goToUser = (userId) => {
  router.push(`/user/${userId}`)
  // Or using named route
  router.push({ name: 'UserProfile', params: { id: userId } })
}

// Navigate with query params
const search = () => {
  router.push({
    name: 'Search',
    query: { q: 'vue', page: 1 }
  })
}

// Access route params
const userId = route.params.id
const query = route.query.q

// Go back
const goBack = () => {
  router.go(-1)
}
</script>
```

---

## 5. Form Patterns

### Form Handling Pattern
**Description:** Handle forms with v-model and validation.

```vue
<template>
  <form @submit.prevent="handleSubmit">
    <div>
      <label>Name:</label>
      <input v-model="form.name" type="text" />
      <span v-if="errors.name">{{ errors.name }}</span>
    </div>
    
    <div>
      <label>Email:</label>
      <input v-model="form.email" type="email" />
      <span v-if="errors.email">{{ errors.email }}</span>
    </div>
    
    <button type="submit" :disabled="!isValid">Submit</button>
  </form>
</template>

<script setup>
import { reactive, computed } from 'vue'

const form = reactive({
  name: '',
  email: ''
})

const errors = reactive({
  name: '',
  email: ''
})

const isValid = computed(() => {
  return form.name && form.email && !errors.name && !errors.email
})

const validateForm = () => {
  errors.name = form.name ? '' : 'Name is required'
  errors.email = form.email ? '' : 'Email is required'
  
  if (form.email && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
    errors.email = 'Invalid email format'
  }
}

const handleSubmit = () => {
  validateForm()
  if (isValid.value) {
    // Submit form
    console.log('Form submitted', form)
  }
}
</script>
```

### Form Validation with Vuelidate Pattern
**Description:** Use Vuelidate for form validation.

```vue
<script setup>
import { useVuelidate } from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'

const form = reactive({
  name: '',
  email: ''
})

const rules = {
  name: { required },
  email: { required, email }
}

const v$ = useVuelidate(rules, form)

const handleSubmit = async () => {
  const isValid = await v$.value.$validate()
  if (isValid) {
    // Submit form
  }
}
</script>
```

---

## 6. Performance Patterns

### Computed Properties Pattern
**Description:** Use computed properties for derived state.

```vue
<script setup>
import { ref, computed } from 'vue'

const items = ref([
  { id: 1, name: 'Item 1', price: 10 },
  { id: 2, name: 'Item 2', price: 20 },
  { id: 3, name: 'Item 3', price: 30 }
])

const searchQuery = ref('')

// Computed property - only recalculates when dependencies change
const filteredItems = computed(() => {
  if (!searchQuery.value) {
    return items.value
  }
  return items.value.filter(item =>
    item.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const totalPrice = computed(() => {
  return filteredItems.value.reduce((sum, item) => sum + item.price, 0)
})
</script>
```

### v-memo Pattern
**Description:** Use v-memo for expensive list rendering.

```vue
<template>
  <div v-for="item in items" :key="item.id" v-memo="[item.id, item.name]">
    <ExpensiveComponent :item="item" />
  </div>
</template>
```

### Lazy Loading Pattern
**Description:** Lazy load components for better performance.

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

// Lazy load component
const HeavyComponent = defineAsyncComponent(() =>
  import('./HeavyComponent.vue')
)

// With loading component
const AsyncComponent = defineAsyncComponent({
  loader: () => import('./AsyncComponent.vue'),
  loadingComponent: LoadingComponent,
  errorComponent: ErrorComponent,
  delay: 200,
  timeout: 3000
})
</script>

<template>
  <Suspense>
    <template #default>
      <AsyncComponent />
    </template>
    <template #fallback>
      <div>Loading...</div>
    </template>
  </Suspense>
</template>
```

---

## 7. Testing Patterns

### Component Testing Pattern
**Description:** Test Vue components with Vitest and Vue Test Utils.

```javascript
import { mount } from '@vue/test-utils'
import { describe, it, expect } from 'vitest'
import UserCard from '@/components/UserCard.vue'

describe('UserCard', () => {
  it('renders user information', () => {
    const user = {
      name: 'John Doe',
      email: 'john@example.com'
    }
    
    const wrapper = mount(UserCard, {
      props: { user }
    })
    
    expect(wrapper.text()).toContain('John Doe')
    expect(wrapper.text()).toContain('john@example.com')
  })
  
  it('emits event when clicked', async () => {
    const user = { name: 'John', email: 'john@example.com' }
    const wrapper = mount(UserCard, {
      props: { user }
    })
    
    await wrapper.find('button').trigger('click')
    
    expect(wrapper.emitted('user-clicked')).toBeTruthy()
    expect(wrapper.emitted('user-clicked')[0]).toEqual([user])
  })
})
```

---

## 8. Directive Patterns

### Custom Directive Pattern
**Description:** Create custom directives for reusable DOM manipulation.

```javascript
// directives/focus.js
export default {
  mounted(el) {
    el.focus()
  },
  updated(el) {
    el.focus()
  }
}

// Usage
<script setup>
import focus from '@/directives/focus'
</script>

<template>
  <input v-focus type="text" />
</template>

// Directive with arguments
export default {
  mounted(el, binding) {
    el.style.color = binding.value
  },
  updated(el, binding) {
    el.style.color = binding.value
  }
}

// Usage
<input v-color="'red'" />
```

---

## 9. Plugin Patterns

### Vue Plugin Pattern
**Description:** Create reusable Vue plugins.

```javascript
// plugins/myPlugin.js
export default {
  install(app, options) {
    // Add global property
    app.config.globalProperties.$myMethod = () => {
      console.log('My method called')
    }
    
    // Add global component
    app.component('MyComponent', MyComponent)
    
    // Add directive
    app.directive('my-directive', {
      mounted(el, binding) {
        // Directive logic
      }
    })
    
    // Provide/inject
    app.provide('myPlugin', options)
  }
}

// Usage in main.js
import { createApp } from 'vue'
import MyPlugin from './plugins/myPlugin'

const app = createApp(App)
app.use(MyPlugin, { option1: 'value1' })
app.mount('#app')
```

---

## 10. Provide/Inject Patterns

### Provide/Inject Pattern
**Description:** Share data across component tree.

```vue
<!-- Parent Component -->
<script setup>
import { provide, ref } from 'vue'

const theme = ref('light')
const user = ref({ name: 'John' })

provide('theme', theme)
provide('user', user)

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
}

provide('toggleTheme', toggleTheme)
</script>

<!-- Child Component (any level) -->
<script setup>
import { inject } from 'vue'

const theme = inject('theme')
const user = inject('user')
const toggleTheme = inject('toggleTheme')

// With default value
const config = inject('config', { defaultValue: 'default' })

// With factory function
const api = inject('api', () => createApi())
</script>
```

---

## 11. Slot Patterns

### Named Slots Pattern
**Description:** Use slots for component composition.

```vue
<!-- Base Component -->
<template>
  <div class="card">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<!-- Usage -->
<Card>
  <template #header>
    <h2>Card Title</h2>
  </template>
  
  <p>Card content</p>
  
  <template #footer>
    <button>Action</button>
  </template>
</Card>
```

### Scoped Slots Pattern
**Description:** Use scoped slots to pass data to parent.

```vue
<!-- List Component -->
<template>
  <ul>
    <li v-for="item in items" :key="item.id">
      <slot :item="item" :index="index"></slot>
    </li>
  </ul>
</template>

<!-- Usage -->
<List :items="users">
  <template #default="{ item, index }">
    <div>
      <span>{{ index + 1 }}. {{ item.name }}</span>
    </div>
  </template>
</List>
```

---

## 12. Custom Hooks (Composables) Patterns

### Composable Pattern
**Description:** Extract reusable logic into composables.

```javascript
// composables/useCounter.js
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)
  
  const increment = () => {
    count.value++
  }
  
  const decrement = () => {
    count.value--
  }
  
  const reset = () => {
    count.value = initialValue
  }
  
  const isEven = computed(() => count.value % 2 === 0)
  
  return {
    count,
    increment,
    decrement,
    reset,
    isEven
  }
}

// Usage in component
<script setup>
import { useCounter } from '@/composables/useCounter'

const { count, increment, decrement, reset, isEven } = useCounter(0)
</script>
```

### Fetch Data Composable Pattern
**Description:** Create composable for data fetching.

```javascript
// composables/useFetch.js
import { ref, onMounted } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const loading = ref(false)
  const error = ref(null)
  
  const fetchData = async () => {
    loading.value = true
    error.value = null
    try {
      const response = await fetch(url)
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`)
      }
      data.value = await response.json()
    } catch (e) {
      error.value = e.message
    } finally {
      loading.value = false
    }
  }
  
  onMounted(() => {
    fetchData()
  })
  
  return {
    data,
    loading,
    error,
    refetch: fetchData
  }
}

// Usage
<script setup>
import { useFetch } from '@/composables/useFetch'

const { data, loading, error } = useFetch('/api/users')
</script>
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Component API** | Composition API | Options API | Vue 3 projects |
| **State Management** | Pinia | Vuex/Provide-Inject | Global state |
| **Component Communication** | Props/Emit | Provide/Inject | Parent-child / Deep nesting |
| **Form Handling** | v-model + Validation | Vuelidate | Form validation |
| **Performance** | Computed Properties | v-memo | Derived state / Lists |

---

## Best Practices Checklist

### Vue.js
- [ ] Use Composition API for new projects
- [ ] Use Pinia for state management
- [ ] Extract reusable logic into composables
- [ ] Use computed properties for derived state
- [ ] Implement proper form validation
- [ ] Use slots for component composition
- [ ] Write component tests
- [ ] Follow Vue.js naming conventions
- [ ] Use TypeScript for type safety
- [ ] Implement proper error handling

---

## Conclusion

This comprehensive guide provides Vue.js-specific patterns for building scalable applications. Remember:

- **Use Composition API** - Modern approach for Vue 3
- **Extract composables** - Create reusable logic
- **Use Pinia** - Modern state management solution
- **Leverage slots** - For flexible component composition
- **Optimize performance** - Use computed properties and v-memo
- **Test thoroughly** - Write component and integration tests

Good luck with your Vue.js development! 🚀

