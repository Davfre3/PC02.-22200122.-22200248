# Módulo de Listado y Filtrado de Digimons

Este módulo contiene los componentes necesarios para listar y filtrar Digimons usando la API de https://digimon-api.vercel.app/

## 📁 Estructura de Archivos

```
src/
├── components/
│   └── digimon/
│       ├── DigimonCard.vue       # Tarjeta individual de Digimon
│       └── DigimonFilter.vue     # Componente de filtros
└── pages/
    └── digimon/
        └── DigimonListPage.vue   # Página principal con listado
```

## 🎯 Funcionalidades Implementadas

### 1. **DigimonCard.vue**
- Muestra la imagen, nombre y nivel del Digimon
- Colores dinámicos según el nivel
- Efecto hover con animación
- Manejo de errores en la carga de imágenes

### 2. **DigimonFilter.vue**
- Filtro por nombre (búsqueda de texto)
- Filtro por nivel (selector desplegable)
- Botón para limpiar todos los filtros
- Diseño sticky para mantener los filtros visibles

### 3. **DigimonListPage.vue**
- Carga de datos desde la API
- Aplicación de filtros en tiempo real
- Estados de carga, error y sin resultados
- Diseño responsive con grid adaptable
- Contador de resultados filtrados

## 🚀 Cómo Integrar

### Paso 1: Agregar la Ruta

Edita el archivo `src/router/routes.js` y agrega la nueva ruta:

```javascript
const routes = [
  {
    path: '/',
    component: () => import('layouts/MainLayout.vue'),
    children: [
      { path: '', component: () => import('pages/IndexPage.vue') },
      // Agrega esta línea para el listado de Digimons
      { path: '/digimons', component: () => import('pages/digimon/DigimonListPage.vue') }
    ]
  },
  {
    path: '/:catchAll(.*)*',
    component: () => import('pages/ErrorNotFound.vue')
  }
]

export default routes
```

### Paso 2: Agregar Link en el Menú (Opcional)

Si quieres agregar un enlace en el menú lateral, edita `src/layouts/MainLayout.vue`:

```javascript
const linksList = [
  {
    title: 'Digimons',
    caption: 'Lista y filtros',
    icon: 'pets',
    link: '/digimons'
  },
  // ... otros links
]
```

### Paso 3: Verificar Axios

El proyecto ya tiene axios configurado en `src/boot/axios.js`, por lo que no es necesario instalar nada adicional.

## 🎨 Características del Diseño

- **Responsive**: Se adapta a diferentes tamaños de pantalla
- **Filtros Sticky**: Los filtros permanecen visibles al hacer scroll
- **Colores por Nivel**:
  - Fresh: Gris
  - In Training: Azul-Gris
  - Rookie: Verde
  - Champion: Azul
  - Ultimate: Púrpura
  - Mega: Rojo
  - Armor: Ámbar

## 🔄 Merge con Login

Este módulo está completamente aislado en carpetas separadas (`components/digimon/` y `pages/digimon/`), lo que facilita el merge con el módulo de login:

1. Los componentes de login pueden ir en `components/auth/` o similar
2. La ruta `/digimons` puede protegerse con guards de autenticación
3. No hay conflictos de nombres o rutas

### Ejemplo de Ruta Protegida (después del merge)

```javascript
{
  path: '/digimons',
  component: () => import('pages/digimon/DigimonListPage.vue'),
  meta: { requiresAuth: true } // Agregar después del merge con login
}
```

## 🧪 Pruebas

Para probar el módulo:

1. Ejecuta el proyecto: `quasar dev`
2. Navega a: `http://localhost:9000/#/digimons`
3. Prueba los filtros por nombre y nivel
4. Verifica que la lista se actualice en tiempo real

## 📝 Notas

- La API no requiere autenticación
- Los datos se cargan al montar el componente
- Los filtros se aplican localmente (no hacen peticiones adicionales)
- El diseño usa componentes de Quasar
