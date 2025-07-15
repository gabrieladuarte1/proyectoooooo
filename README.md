# Módulo Cliente - ConcreteWare

## Descripción

El **Módulo Cliente** de ConcreteWare es una aplicación React con TypeScript que permite a los clientes gestionar sus pedidos de concreto premezclado de manera eficiente y en tiempo real.

## Características Principales

### 🔐 Autenticación y Seguridad
- **Firebase Authentication** con inicio de sesión por correo y contraseña
- **Cambio obligatorio de contraseña** en el primer ingreso
- **Recuperación de contraseña** mediante verificación por correo
- **Cierre de sesión seguro** desde la aplicación o navegador

### 📱 Interfaz de Usuario
- **Diseño moderno y responsivo** con TailwindCSS
- **Pantalla de inicio** con información del cliente y estado de pedidos
- **Navegación intuitiva** entre diferentes secciones
- **Componentes reutilizables** y bien estructurados

### 📋 Gestión de Pedidos
- **Vista de pedidos en curso y pendientes** con tarjetas visuales
- **Detalles completos de pedidos** con información de estado
- **Modificación de franjas horarias** (hasta 4 horas antes)
- **Ajuste de cantidades** para pedidos pendientes
- **Creación de nuevos pedidos** con formulario completo

### 🗺️ Seguimiento en Tiempo Real
- **Mapa interactivo** con ubicación de vehículos asignados
- **Enlaces públicos de rastreo** para compartir con terceros
- **Actualizaciones en tiempo real** del estado de entrega

### 📞 Comunicación
- **Contacto rápido con la planta** (WhatsApp/llamada)
- **Botones de contacto** visibles desde la vista principal y detalles

## Tecnologías Utilizadas

- **React 18** con TypeScript
- **TailwindCSS** para estilos
- **Firebase Authentication** para autenticación
- **React Router** para navegación
- **PostCSS** para procesamiento de CSS

## Estructura del Proyecto

```
src/
├── components/     # Componentes reutilizables
├── pages/         # Páginas principales
├── services/      # Servicios de API y Firebase
├── contexts/      # Contextos de React
├── types/         # Definiciones de tipos TypeScript
├── utils/         # Utilidades y helpers
└── config/        # Configuraciones
```

## Instalación y Configuración

1. **Clonar el repositorio**
2. **Instalar dependencias**: `npm install`
3. **Configurar variables de entorno**: Copiar `env.example` a `.env.local`
4. **Configurar Firebase**: Agregar credenciales de Firebase
5. **Iniciar desarrollo**: `npm start`

## Scripts Disponibles

- `npm start` - Inicia el servidor de desarrollo
- `npm build` - Construye la aplicación para producción
- `npm test` - Ejecuta las pruebas
- `npm eject` - Expone la configuración de webpack

## Contribución

Este módulo sigue las convenciones establecidas en `CONVENTION.md` para mantener la consistencia en el código y la estructura del proyecto.

## Licencia

Proyecto académico - Universidad Nacional de Colombia
