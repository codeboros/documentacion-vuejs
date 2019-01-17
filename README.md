# Vue js



## Interpolación

```html
<!-- Interpolación -->
<span>{{ nombre }}</span>

<!-- Interpolación de métodos (debe retornar algo que se pueda mostrar) -->
<span>{{ mostrarMensaje() }}</span>
```



## Enlace de datos doble

```html
<input type="text" v-model="nombre">
```



## Directivas

### v-for

```html
<ul>
	<li v-for="pais in paises"> <!-- también puede ser "pais of paises" -->
		{{ pais.nombre }}
	</li>
</ul>

<!-- Ejemplo con índice -->
<ul>
	<li v-for="(pais, i) of paises"> 
		{{ i + 1 }} - {{ pais }}
	</li>
</ul>
```

```html
<!-- Con la etiqueta template de html podemos repetir estructuras mas complejas -->
<ul>
<template v-for="pais of paises"> 
	<li>
		{{pais}}
	</li>
	<hr>
</template>
</ul>
```

```html
<!-- al recorrer un objeto se puede adquirir el nombre de la proiedad y el valor de esta -->
<ul>
	<li v-for="(valor, prop) of persona">
		{{ prop }} = {{ valor }}
	</li>
</ul>
```

### v-if & v-else

```html
<span v-if="miVar">texto...</span>
<span v-if="miVar==5">texto...</span>
<span v-if="x==y">texto...</span>
<span v-else>texto...</span>
```

### v-else-if

```html
<!-- debe haber primero un v-if -->
<p v-if="num==1">parrafo 1</p>
<p v-else-if="num==2">parrafo 2</p>
<p v-else-if="num==3">parrafo 3</p>
<!-- también se puede poner un v-else para cuando no es ninguna de las anteriores -->
<p v-else>no es ningún parrafo</p>
```

### v-show

```html
<!-- es lo mismo que v-if pero este oculta el elemento con css -->
<p v-show="mostrar">texto...</p>
```

### v-bind:

```html
<!-- Enlazar una variable a una propiedad html -->
<img v-bind:src="img">
<!-- manera rápida -->
<img :src="img">
```

### v-on:

```html
<button v-on:click>ingresar</button>
<input v-on:keyup.enter="enviar()"></input>
<!-- manera rápida -->
<input @keyup.enter="enviar()"></input>
```

### v-html

```html
<!-- nos sirve para que reconosca etiquetas html desde la variable -->
<p v-html="texto"></p>
```

```javascript
data: {
    texto: '<strong>Ejemplo de v-html</strong>'
}
```

### v-text

```html
<!-- esta directiva tiene la misma función que la interpolación {{ algo }} -->
<p v-text="texto"></p>
```



## Eventos

### Modificadores de eventos

```html
<!-- .once ejecuta el evento solo una vez -->
<div v-on:click.once="metodo">
    algo
</div>
<!-- .stop detiene la propagación del evento gatillado -->
<div v-on:mousemove.stop>
    algo
</div>
```

**💡TIP: ** se puede reemplazar v-on: por @ en los eventos, ejemplo: @click="metodo( )"



## Componentes

### Estructura del componente

```vue
<template>
	<!-- elementos html -->
</template>

<script>
    export default{
        props: ['parametro'], // recibe información desde componentes padres
        data(){
            return{
                titulo: 'Hola mundo' // variables locales
            }
        },
        methods: {
            miFuncion(){ // funciones del componente
                
            }
        }
    }
</script>

<style>
	/* estilos CSS */
</style>
```



### Traspaso de información a componentes hijos

```vue
<!-- Componente padre -->
<template>
	<nombre-componente :miPropiedad="miPropiedad"></nombre-componente>
</template>
```

```vue
<!-- Componente hijo -->
<script>
    export default{
        props: ['miPropiedad']
    }
</script>
```

**:bulb:Tip:** También se pueden enviar metodos desde el componente padre al hijo.

### Traspaso de información hacia el componente padre (eventos)

```vue
<!-- Componente hijo -->
<script>
    export default{
        data(){
            return{
                valor: ''
            }
        }
        methods: {
            mandarInfo(){
    // argumentos: 1. nombre del evento a emitir - 2. parametro a enviar al elemento padre
                this.$emit('incrementar', this.valor) 
            }
        }
    }
</script>
```

```vue
<!-- Componente padre -->
<template>
	<nombre-componente v-on:incrementar="total += $event"></nombre-componente>
		<!-- $event es el parametro enviado desde el componente hijo -->
</template>

<script>
    export default{
        data(){
            return {
                total: 0
            }
        }
    }
</script>
```



### Validación sobre propiedad

```vue
<script>
    export default{
        data(){
        	return {
            	props: { // se usa props como objeto, no como array
                	titulo: {
                    	default: 'titulo por defecto'
                	}
            	}
        	}
    	}
    }
    
</script>
```



## Ciclos de vida de la instancia en vue (hooks)

1. **beforeCreate( )** antes de que la instancia sea creada
2. **created( )** cuando la instancia ya ha sido creada
3. **beforeMount( )** antes de montar la plantilla en el DOM de la app
4. **mounted( )** cuando se ha montado la plantilla en el DOM
5. **beforeUpdate( )** antes que actualice información 
6. **updated( )** una vez haya terminado la actualización 
7. **beforeDestroy( )** antes de destruir la instancia, acá se cierra conexión a bd, etc.
8. **destroyed( )** cuando la instacia ha sido destruida

```javascript
new Vue({
  el: '#vm',
  data: {
    mensaje: 'Este es el mensaje'
  },
  beforeCreate(){
    console.log('Llamando beforeCreate');
  },
  created(){
    console.log('Llamando created');
  },
  beforeMount(){
    console.log('Llamando beforeMount');
  },
  mounted(){
    console.log('Llamando mounted');
  },
  beforeUpdate(){
    console.log('Llamando beforeUpdate');
  },
  updated(){
    console.log('Llamando updated');
  },
  beforeDestroy(){
    console.log('Llamando beforeDestroy');
  },
  destroyed(){
    console.log('Llamando destroyed');
  },
  activated(){
  	console.log('componente activo');      
  },
  desactivated(){
    console.log('componente inactivo');
  }
  
  methods : {
    destruir : function(){
      this.$destroy();
    }
  }
  
}) 
```



## Slots

```vue
<!-- componente hijo -->
<template>
	<slot></slot> <!-- slot por defecto -->
</template>
<!-- componente padre -->
<template>
	<nombre-componente>
    	<h1>Hola mundo</h1> <!-- esta etiqueta se mostrará en el slot del componente hijo -->
    </nombre-componente>
</template>
```

```vue
<!-- componente hijo -->
<template>
	<form class="form">
        <slot name="elementos"></slot>
        <slot name="boton"></slot>
    </form>
</template>

<!-- componente padre -->
<template>
	<nombre-componente>
    	<div slot="elementos">
            <label>Correo electrónico</label>
        	<input type="email">    
    	</div>
        <div slot="boton">
        	<button>Ingresar</button>    
    	</div>
    </nombre-componente>
</template>
```



## Componentes dinámicos

```vue
<!-- componente padre -->
<template>
	<button @click="componenteSeleccionado = 'formLogin'">Iniciar Sesión</button>
	<button @click="componenteSeleccionado = 'formregister'">Registrar cuenta</button>
	<component v-bind:is="componenteSeleccionado"></component>
</template>

<script>
import FormLogin from './components/FormLogin.vue'
import FormRegister from './components/FormRegister.vue'
    export default{
        components: {
            formLogin: FormLogin,
            formRegister: FormRegister
        },
        data(){
            return{
                componenteSeleccionado: 'formLogin'
            }
        }
    }
</script>
```

### Evitar la destrucción del componente

```vue
<!-- componente padre -->
<template>
	<button @click="componenteSeleccionado = 'formLogin'">Iniciar Sesión</button>
	<button @click="componenteSeleccionado = 'formregister'">Registrar cuenta</button>
	<keep-alive> <!-- no se destruye el componente -->
	    <component v-bind:is="componenteSeleccionado"></component>
    </keep-alive>
</template>

<!-- componente hijo -->
<script>
    export default{
        // estos hooks puede ser muy utiles para componentes dinámicos
        activated(){
            console.log('componente activo');
        },
        desactivated(){
            console.log('componente desactivado');
        }
    }
</script>
```



## Bus de datos (servicio)

```javascript
// dentro del main.js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

export const bus = new Vue({
    data:{
        // data en este caso queda como objeto, se pueden guardar variable globales
    },
    methods: {
        actualizarContador(numTareas){
            this.$emit('actualizarContador', numTareas)
        }
    }
}); // se exporta el bus

new Vue({
  render: h => h(App),
}).$mount('#app')
```

```vue
<!-- dentro del componente -->
<script>
import { bus } from './main.js'
    export default{
        methods: {
            miFuncion(){
                bus.actualizarContador(this.tareas.lenght)
            }
        }
    }
</script>

```

```vue
<!-- dentro de otro componente -->
<script>
import { bus } from './main.js'
    export default{
        data(){
            return {
                varLocal: 0
            }
        },
        created(){ // recordar que created es un ciclo de vida (hook)
            bus.$on('actualizarContador', (valor) => {
                this.varLocal = valor;
            })
        }
    }
</script>
```



## Computed properties

```html
    <div class="vue">
        <button v-on:click="primero++">Primero</button>
        <button v-on:click="segundo++">Segundo</button>
        <span>Primero = {{ primero }}</span>
        <span>Segundo = {{ segundo }}</span>
        <span>Total = {{ total }}</span>
    </div>
```

```javascript
const app = new Vue({
    el: '.vue',
    data: {
        primero: 0,
        segundo: 0,
        suma: 0
    },
    computed: { // va despues de data
        total: function(){
            return this.primero + this.segundo
        }
    }
})

// cada vez que haya un cambio relacionado con las variables comprometidas se ejecutará la función
```



## Forms

### Agrupación de datos

```vue
<!-- puede ser de gran utilidad en los forms para enviar solo un objeto con todos las propiedades que sean necesarias -->
<script>
    export default {
        data(){
            return{
                user: {
                    name: '',
                    email: '',
                    phone: ''
                }
            }
        }
    }
</script>
```

### Modificadores útiles para forms

```vue
<template>
	<!-- trim para eliminar los espacios en el texto -->
	<input type="text" v-model.trim>

	<!-- number para que el numero ingresado sea typeof number -->
	<input type="text" v-model.number>

	<!-- lazy para que el v-model haga su trabajo una vez que el focus sale del input -->
	<input type="text" v-model.lazy>
</template>
```

### Dropdown list

```vue
<template>
	<select v-model="pais">
        <option v-for="pais in paises">{{ pais }}</option>
    </select>
</template>

<script>
    export default{
        data(){
            return{
                paises: ['Chile', 'Peru', 'Argentina']
            }
        }
    }
</script>
```

### Radio buttons

```vue
<template>
	<label>
    	<input type="radio" name="optionRadios" value="hombre" v-model="usuario.genero">
    </label>
	<label>
    	<input type="radio" name="optionRadios" value="mujer" v-model="usuario.genero">
    </label>
</template>
```

### Checkbox

```vue
<template>
	<label>
    	<input type="checkbox" value="acepto" v-model="usuario.condiciones">
        Acepto condiciones
    </label>
	<label>
    	<input type="checkbox" value="acepto" v-model="usuario.condiciones">
        Recibir newsletter
    </label>
</template>

<script>
    export default{
        data(){
            return{
                usuario: {
                    condiciones: [] // se usa un array para mantener un orden en caso de tener mas de un checkbox, de lo contrario basta con una propiedad normal
                }
            }
        }
    }
</script>
```



## Filtros (pipe en angular)

```vue
<!-- componente  -->
<template>
	<span>{{ mensaje | mayuscula }}</span>
</template>
```

```vue
<!-- dentro del main.js --> 
Vue.filter('mayuscula', (texto) => {
	return texto.toUpperCase()
})
```

:bulb: **Tip:** vue js no viene con filtros por defecto

## vue cli

1. **vue create nombreProyecto** crea el proyecto.
2. **npm run serve** ejecuta el proyecto.

## Notas

- se puede referenciar a un objeto del dom ya sea con class (.nombre) o con id (#nombre) desde la instancia de Vue el: '.nombre' o '#nombre'.
- El componente `<template></template>` solo puede tener un elemento root (solo una etiqueta html global).
- props: en los components puede ser un array o un objeto.
- se puede enviar tanto variables como funciones al componente hijo usando v-bind:, por ejemplo, :miFuncion="miFuncion".
- en los slots las class de las etiquedas html toman el style del componente donde se encuentre el slot
- se puede agregar un slot por defecto **<slot></slot>** sin nombre, este tomara cualquier elemento html que no tenga asginado un slot previo.