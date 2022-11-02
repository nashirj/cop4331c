# Vue crash course
Taken from vuejs.org/tutorial

## Declarative rendering
Each component is it's own set of data that can be accessed from within template:

<script>
export default {
  data() {
    return {
      message: 'Hello World!'
    }
  }
}
</script>

<template>
  <h1>{{ message }}</h1>
</template>

## Attribute bindings
Directive values are JavaScript expressions that have access to the component's state. The part after the colon (:id) is the "argument" of the directive. Here, the element's id attribute will be synced with the dynamicId property from the component's state.

<div v-bind:id="dynamicId"></div>
// Shorthand
<div :id="dynamicId"></div>

Ex:

<script>
export default {
  data() {
    return {
      titleClass: 'title'
    }
  }
}
</script>

<template>
  <h1 :class="titleClass">Make me red</h1>
</template>

<style>
.title {
  color: red;
}
</style>

## Event listeners
We can listen to DOM events using the v-on directive:

<button v-on:click="increment">{{ count }}</button>
// Shorthand
<button @click="increment">{{ count }}</button>

We also can have methods defined in the component which have access to component state.

<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  }
}
</script>

<template>
  <button @click="increment">count is: {{ count }}</button>
</template>

## Form bindings
Using v-bind and v-on together, we can create two-way bindings on form input elements:

<input :value="text" @input="onInput">
// Shorthand
<input v-model="text">

Example: this code updates the contents of the `text` variable as you type in the input textbox

<script>
export default {
  data() {
    return {
      text: ''
    }
  }
}
</script>

<template>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>

## Conditional rendering
We can use the v-if directive to conditionally render an element. In the example below, awesome is a truthy variable (defined in component's `data()`)

<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>

## List Rendering
We can use the v-for directive to render a list of elements based on a source array:

<script>
// give each todo a unique id
let id = 0

export default {
  data() {
    return {
      newTodo: '',
      todos: [
        { id: id++, text: 'Learn HTML' },
        { id: id++, text: 'Learn JavaScript' },
        { id: id++, text: 'Learn Vue' }
      ]
    }
  },
  methods: {
    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    }
  }
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>    
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>

## Computed property
Can have methods that compute modifications to properties, such as filtering todos to only return those that are complete using the `computed` property:

<script>
let id = 0

export default {
  data() {
    return {
      newTodo: '',
      hideCompleted: false,
      todos: [
        { id: id++, text: 'Learn HTML', done: true },
        { id: id++, text: 'Learn JavaScript', done: true },
        { id: id++, text: 'Learn Vue', done: false }
      ]
    }
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos.filter((t) => !t.done)
        : this.todos
    }
  },
  methods: {
    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo, done: false })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    }
  }
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>

## Lifecycle and Template Refs
Can get references to elements in the template using `ref` attribute; can then combine with `mounted()` option:

<script>
export default {
  mounted() {
    this.$refs.p.textContent = 'mounted!'
  }
}
</script>

<template>
  <p ref="p">hello</p>
</template>

## Watchers
We can use watchers to perform side effects reactively when the value of a property changes

<script>
export default {
  data() {
    return {
      todoId: 1,
      todoData: null
    }
  },
  methods: {
    async fetchData() {
      this.todoData = null
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
      )
      this.todoData = await res.json()
    }
  },
  mounted() {
    this.fetchData()
  },
  watch: {
    todoId() {
      this.fetchData()
    }
  }
}
</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>

## Components
Real Vue applications are typically created with nested components. A parent component can render another component in its template as a child component. To use a child component, we need to first import it.

<script>
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  }
}
</script>

<template>
  <ChildComp />
</template>

## Props
A child component can accept input from the parent via props. First, it needs to declare the props it accepts. Once declared, the msg prop is exposed on this and can be used in the child component's template. The parent can pass the prop to the child just like attributes. To pass a dynamic value, we can also use the v-bind syntax:

### App.vue
<script>
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  },
  data() {
    return {
      greeting: 'Hello from parent'
    }
  }
}
</script>

<template>
  <ChildComp :msg="greeting" />
</template>

### ChildComp.vue
<script>
export default {
  props: {
    msg: String
  }
}
</script>

<template>
  <h2>{{ msg || 'No props passed yet' }}</h2>
</template>

## Emits
In addition to receiving props, a child component can also emit events to the parent. The first argument to `this.$emit()` is the event name. Any additional arguments are passed on to the event listener.

The parent can listen to child-emitted events using v-on - here the handler receives the extra argument from the child emit call and assigns it to local state:

### App.vue
<script>
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  },
  data() {
    return {
      childMsg: 'No child msg yet'
    }
  }
}
</script>

<template>
  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>
</template>

### ChildComp.vue
<script>
export default {
  emits: ['response'],
  created() {
    this.$emit('response', 'hello from child')
  }
}
</script>

<template>
  <h2>Child component</h2>
</template>

## Slots
In addition to passing data via props, the parent component can also pass down template fragments to the child via slots.

<ChildComp>
  This is some slot content!
</ChildComp>

The child component can render the slot content from the parent using the <slot> element as outlet:
<!-- in child template -->
<slot/>

Content inside the <slot> outlet will be treated as "fallback" content; it will be displayed if the parent did not pass down any slot content:
<slot>Fallback content</slot>

### App.vue
<script>
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  },
  data() {
    return {
      msg: 'from parent'
    }
  }
}
</script>

<template>
  <ChildComp>Message: {{ msg }}</ChildComp>
</template>

### ChildComp.vue
<template>
  <slot>Fallback content</slot>
</template>
