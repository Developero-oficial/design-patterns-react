# Design Patterns React

![Design patterns react](https://github.com/Developero-oficial/design-patterns-react/blob/master/assets/react-js-design-patterns.png?raw=true)
Bienvenido a **Design Patterns React** o **Patrones de diseño en React.js**. Esta guía cubre los patrones más efectivos y prácticos en React.js para ayudarte a escribir código más limpio y mantenible.

## Tabla de Contenidos

1. [Patrones de diseño tradicionales VS patrones de diseño en React.js](#patrones-de-diseño-tradicionales-vs-patrones-de-diseño-en-react-js)
2. [Introducción a los Patrones de Diseño React JS](#introducción-a-los-patrones-de-diseño-react-js)
3. [Custom Hook](#custom-hook)
4. [HOC (Higher Order Component)](#hoc-higher-order-component)
5. [Estilos Extensibles](#estilos-extensibles)
6. [Componentes Compuestos](#componentes-compuestos)
7. [Render Props](#render-props)
8. [Control Props](#control-props)
9. [Props Getters](#props-getters)
10. [Inicializador de Estado](#inicializador-de-estado)
11. [Descargar el Ebook Completo](#descargar-el-ebook-completo)

---

### Patrones de diseño tradicionales VS patrones de diseño en React JS

Los patrones de diseño son soluciones generales y reutilizables a problemas comunes que se presentan en el desarrollo de software.

Hicieron su debut en el libro "Design Patterns: Elements of Reusable Object-Oriented Software" de Erich Gamma, Richard Helm, Ralph Johnson y John Vlissides, también conocido como "Gang of Four".

Originalmente, los patrones de diseño se aplicaban a la programación orientada a objetos, pero con el tiempo se han adaptado a otros paradigmas de programación, incluido el desarrollo web.

Sin embargo, los patrones que veremos a continuación son creados en base a los principios SOLID (principalmente Open - Close principle) y en las características especiales de React.js, como el uso de estados, composición, etc.

Por lo tanto, estos patrones no son aplicables a otras bibliotecas o frameworks como Vue.js o Angular.

### Introducción a los Patrones de Diseño React JS
Aprende los conceptos básicos y la importancia de los patrones de diseño en React JS.  
**[Leer el Blog](https://developero.io/blog/react-patrones-avanzados-introduccion)** | **[Ver el Video](https://www.youtube.com/watch?v=your_video_id)**

### Custom Hook
Un custom hook es una función que empieza por "use" para que React.js lo pueda reconocer como un hook, y sirve para que puedas crear tus propios hooks.

Considera:

- Un custom hook puede utilizar los hooks nativos de React.js y otros custom hooks
- Sigue las mismas reglas de uso de un hook (por ejemplo: no puedes inicializar un hook fuera de un componente).

Ejemplo de un custom hook:
```jsx
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key)
      return item ? JSON.parse(item) : initialValue
    } catch (error) {
      console.log(error)
      return initialValue
    }
  })

  const setValue = (value) => {
    try {
      setStoredValue(value)
      window.localStorage.setItem(key, JSON.stringify(value))
    } catch (error) {
      console.log(error)
    }
  }

  return [storedValue, setValue]
}
```


**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/custom-hook)** | **[Ver Custom Hook en Video](https://youtu.be/VJlANS3IPxg)**

### HOC (Higher Order Component)
Un HOC, o Higher Order Component, es esencialmente una función que recibe un componente y devuelve otro componente, pero con funcionalidades adicionales o modificadas. Piensa en los HOC como una forma de envolver un componente existente con nueva lógica, sin alterar el componente original.

Ejemplo de un HOC:
```jsx
import React, { useEffect } from 'react';
import { useHistory } from 'react-router-dom';

function withAuthentication(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const history = useHistory();

    useEffect(() => {
      if (!props.isAuthenticated) {
        history.push('/login');
      }
    }, [props.isAuthenticated, history]);

    return <WrappedComponent {...props} />;
  };
}

const AuthenticatedComponent = withAuthentication(MyComponent);

```

**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/hoc)** | **[Ver HOC en Video](https://youtu.be/Krg6N0cLPAI)**

### Estilos Extensibles
El patrón de Estilos Extensibles se refiere a la capacidad de crear componentes que acepten estilos personalizados y se combinen o extiendan de los estilos base del componente, permitiendo así personalizaciones más allá de las definiciones de estilo predeterminadas.

Ejemplo de estilos extendibles:

1. Define un conjunto base de estilos para tu componente.
```js
const baseStyle = {
  color: 'blue',
  padding: '10px',
  margin: '5px',
}

```
2. Permite que se pasen estilos personalizados como props y combínalos con tus estilos base.
```jsx
function CustomButton({ customStyle }) {
  return <button style={{ ...baseStyle, ...customStyle }}>Click me!</button>
}

```
3. Ahora, cuando utilices tu componente, puedes pasar estilos personalizados
```jsx
<CustomButton customStyle={{ backgroundColor: 'red', fontSize: '20px' }} />

```

**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/extensible-styles)** | **[Ver Estilos Extensibles en Video](https://youtu.be/gkLx5qMs53A)**

### Componentes Compuestos
El Patrón de Componentes Compuestos en React es un patrón de diseño que, al ser combinado con Contexto de React, permite compartir estado entre componentes relacionados de manera implícita, sin tener que pasar props explícitamente a través de cada nivel del árbol de componentes.

Ejemplo de componente compuesto con Context:

1. **Crea tu Contexto:**

```js
const ArticleContext = React.createContext()
```

2. **Crea Componentes que Consuman el Contexto:**
   Estos componentes accederán al estado y funciones compartidos a través del contexto.

```jsx
function Title() {
  const { title } = React.useContext(ArticleContext)
  return <h1>{title}</h1>
}

function Content() {
  const { content } = React.useContext(ArticleContext)
  return <p>{content}</p>
}
```

3. **Combina los Componentes en un Componente Padre con Proveedor de Contexto:**
   Este componente actuará como el componente compuesto, proporcionando el estado y funciones a sus hijos.

```jsx
function Article(props) {
  return (
    <ArticleContext.Provider value={props}>
      <div>
        <Title />
        <Content />
      </div>
    </ArticleContext.Provider>
  )
}
```

4. **Usa tu Componente Compuesto con Contexto:**

```jsx
<Article title="Título del Artículo" content="Contenido del artículo..." />
```

**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/compound-component-pattern)** | **[Ver el Video](https://youtu.be/kEq36QY3PUM)**

### Render Props
El patrón Render Props en React se refiere a una técnica donde un componente recibe una función a través de sus props que devuelve un elemento React. Esta función se utiliza para determinar qué renderizar, permitiendo así una gran flexibilidad y reutilización de lógica entre componentes.

Ejemplo de Render Props:
```jsx
import React, { useState } from 'react'

const MouseTracker = ({ render }) => {
  const [position, setPosition] = useState({ x: 0, y: 0 })

  const handleMouseMove = (event) => {
    setPosition({
      x: event.clientX,
      y: event.clientY,
    })
  }

  return (
    <div style={{ height: '100vh' }} onMouseMove={handleMouseMove}>
      {render(position)}
    </div>
  )
}

const App = () => {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <h1>
          La posición del mouse es: (x: {x}, y: {y})
        </h1>
      )}
    />
  )
}

export default App

```

**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/render-props)** | **[Ver el Video](https://youtu.be/TTOfKSh1Fyk)**

### Control Props
Los Control Props se refieren a props que un componente acepta para controlar su estado interno.

Si alguna vez has utilizado un componente input controlado en React, imagina que puedes replicar lo mismo pero en cualquier componente.

La idea es que podamos tener la opción de que en lugar de que el componente gestione su propio estado, este recibe su estado a través de props y notifica cualquier cambio a través de callbacks.

Ejemplo de control props:
```jsx
```jsx
import React, { useState } from 'react'

const Toggle = ({ value, setValue, defaultIsOn = false }) => {
  const [isOn, setIsOn] = useState(defaultIsOn)

  const isControlled = value !== undefined && setValue !== undefined

  const currentState = isControlled ? value : isOn

  const handleToggle = () => {
    if (isControlled) {
      setValue(!currentState)
    } else {
      setIsOn(!currentState)
    }
  }

  return <button onClick={handleToggle}>{currentState ? 'ON' : 'OFF'}</button>
}

export default Toggle

```

**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/control-props)** | **[Ver el Video](https://youtu.be/WHlO3gduNk8)**

### Props Getters
El Props Getters pattern en React es una técnica avanzada que nos permite abstraer y distribuir propiedades a componentes hijos de una manera más eficiente y controlada.

Se utiliza comúnmente en componentes de bibliotecas para proporcionar un mayor control sobre cómo se aplican las props a los componentes hijos, sin sacrificar la personalización.

Ejemplo de Props Getters:

```jsx
function Dropdown({ children }) {
  const [isOpen, setIsOpen] = useState(false)

  const getButtonProps = ({ onClick, ...props } = {}) => ({
    onClick: (...args) => {
      setIsOpen(!isOpen)
      if (onClick) {
        onClick(...args)
      }
    },
    ...props,
  })

  return (
    <>
      <button {...getButtonProps()}>Toggle Dropdown</button>
      {isOpen && children}
    </>
  )
}

```

**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/props-getters)** | **[Ver el Video](https://youtu.be/CMfPGVKw0yU)**

### Inicializador de Estado
El patrón State Initializer permite inicializar el estado de un componente en función de las props recibidas. Aunque esto puede lograrse directamente en el constructor de un componente de clase o en un Hook useState en un componente funcional, el patrón de State Initializer lleva esto un paso más allá, ofreciendo una mayor flexibilidad y control sobre el estado inicial.

Ejemplo de Inicializador de Estado:
```jsx
function Counter({ initialCount }) {
  const [count, setCount] = React.useState(initialCount)

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
    </div>
  )
}

// Uso:
;<Counter initialCount={5} />

```

**[Leer el Blog](https://developero.io/blog/react-js-design-patterns/state-initializer)** | **[Ver el Video]([https://youtu.be/CMfPGVKw0yU](https://youtu.be/J-l97FzHAW4))**

---

## Descargar el Ebook Completo

![Descargar ebook patrones avanzados en react js](https://github.com/Developero-oficial/design-patterns-react/blob/master/assets/ebook-patrones-avanzados-reactjs-download.png?raw=true)

¿Quieres profundizar más en estos patrones con ejemplos detallados, ventajas, desventajas y más? Descarga el ebook completo **"9 Patrones Avanzados en React.js"** gratis.

[**Descargar el Ebook**](https://developero.ipzmarketing.com/f/XttdO4jxMu4) 📘

--- 

Aquí tienes un llamado a la acción en neutral español:

---

## ¡Mantente Conectado!

¿Te ha sido útil esta guía? No olvides darle una estrella a este repositorio ⭐ para que más desarrolladores puedan encontrarlo. Además, sigue nuestras redes para estar al tanto de más contenido sobre React.js y desarrollo frontend.

- **[Suscríbete a nuestro canal de YouTube]([https://www.youtube.com/channel/your_channel](https://www.youtube.com/@Developero))** para más tutoriales en video.
- **[Únete a nuestra lista de correos](https://developero.ipzmarketing.com/f/adJdhFI4A9w)** para recibir actualizaciones y recursos exclusivos.
- **[Síguenos en LinkedIn](https://www.linkedin.com/company/developero/)** para consejos de carrera y noticias de la comunidad.

¡Gracias por ser parte de nuestra comunidad de desarrolladores!

