# 🚛 ConcreteWare - Interfaz de Conductor

Esta documentación describe la implementación de la interfaz de usuario específica para conductores en el sistema ConcreteWare.

## 📋 Funcionalidades Implementadas

### 🔐 **Autenticación y Seguridad**
- **Login obligatorio** con credenciales proporcionadas por administrador
- **Cambio de contraseña obligatorio** en el primer inicio de sesión
- **Validación de roles** - Los conductores solo pueden acceder a sus funcionalidades específicas
- **Sesión persistente** con Firebase Authentication

### 🏠 **Dashboard Principal del Conductor**
- **Vista de pedidos del día** - Lista solo los pedidos asignados al conductor
- **Estadísticas en tiempo real**:
  - Total de pedidos asignados
  - Pedidos en proceso
  - Pedidos completados
- **Información detallada** de cada pedido:
  - Cliente y empresa
  - Obra y dirección
  - Productos y cantidades
  - Horarios de entrega
  - Estado actual

### 🗺️ **Integración con Google Maps**
- **Botón "Abrir en Google Maps"** en cada pedido
- **Navegación directa** a la dirección de la obra
- **URL dinámica** con la dirección codificada

### 📊 **Gestión de Estados de Pedidos**
Los conductores pueden cambiar el estado de sus pedidos asignados:

1. **Cargando** - Inicio del proceso de carga
2. **Alistamiento** - Preparación para salida
3. **Salida de Planta** - Vehículo en ruta
4. **Llegada a Obra** - Llegada al destino
5. **Inicio de Descargue** - Comenzando descarga
6. **Finaliza de Descargue** - Descarga completada
7. **Llegada a Planta** - Retorno a planta

### 📸 **Evidencia Fotográfica**
- **Foto obligatoria** en cada cambio de estado
- **Observaciones opcionales** para cada evidencia
- **Almacenamiento seguro** en el backend
- **Validación de archivos** (solo imágenes)

### 🚨 **Reporte de Novedades**
- **Formulario de reporte** con título y descripción
- **Adjuntar imagen** opcional
- **Estados de seguimiento**: Pendiente, En revisión, Resuelto
- **Notificaciones** al administrador

### ✅ **Checklist de Vehículo**
- **Lista de verificación** al final de la jornada
- **Items predefinidos**:
  - Frenos en buen estado
  - Luces funcionando
  - Neumáticos en buen estado
  - Motor sin ruidos extraños
  - Documentación al día
  - Equipos de seguridad
- **Estados por item**: OK, DEFECTO, NO APLICA
- **Observaciones** por item
- **Estado general** del vehículo
- **Almacenamiento** del historial

### 📍 **Seguimiento de Ubicación en Tiempo Real**
- **Autorización permanente** de ubicación
- **Envío automático** cada 30 segundos
- **Indicador visual** de estado de conexión
- **Coordenadas precisas** con información de precisión
- **Manejo de errores** de geolocalización
- **Modo offline** con reintentos automáticos

## 🛠️ **Arquitectura Técnica**

### **Componentes Principales**

#### `ConductorDashboard.tsx`
- Dashboard principal con lista de pedidos
- Estadísticas y métricas
- Modal de detalles de pedido
- Integración con Google Maps

#### `ConductorGestionPedido.tsx`
- Gestión completa de un pedido específico
- Cambio de estados con evidencia fotográfica
- Reporte de novedades
- Checklist de vehículo

#### `ConductorCambioPassword.tsx`
- Formulario de cambio de contraseña obligatorio
- Validación de requisitos de seguridad
- Integración con Firebase Auth

#### `LocationTracker.tsx`
- Componente de seguimiento de ubicación
- Permisos de geolocalización
- Envío automático al backend
- Indicadores de estado

### **Tipos de Datos**

```typescript
// Estados específicos para conductores
enum EstadoPedidoConductor {
  CARGANDO = "CARGANDO",
  ALISTAMIENTO = "ALISTAMIENTO",
  SALIDA_PLANTA = "SALIDA_PLANTA",
  LLEGADA_OBRA = "LLEGADA_OBRA",
  INICIO_DESCARGUE = "INICIO_DESCARGUE",
  FINALIZA_DESCARGUE = "FINALIZA_DESCARGUE",
  LLEGADA_PLANTA = "LLEGADA_PLANTA"
}

// Pedido con información completa para conductor
interface PedidoConductor extends Pedido {
  cliente?: Cliente;
  obra?: Obra;
  vehiculo?: Vehiculo;
  conductor?: Conductor;
  productosDetalle?: ProductoPedidoDetalle[];
}

// Reporte de novedades
interface ReporteNovedad {
  idReporte: string;
  idPedido: string;
  idConductor: string;
  titulo: string;
  descripcion: string;
  fechaReporte: string;
  estado: 'PENDIENTE' | 'EN_REVISION' | 'RESUELTO';
  imagenUrl?: string;
}

// Evidencia fotográfica
interface EvidenciaEstado {
  idEvidencia: string;
  idPedido: string;
  idConductor: string;
  estadoAnterior: EstadoPedidoConductor;
  estadoNuevo: EstadoPedidoConductor;
  fechaCambio: string;
  imagenUrl: string;
  observaciones?: string;
  ubicacion: Ubicacion;
}

// Checklist de vehículo
interface ChecklistVehiculo {
  idChecklist: string;
  idVehiculo: string;
  idConductor: string;
  fecha: string;
  items: ChecklistItem[];
  observaciones?: string;
  estadoGeneral: 'EXCELENTE' | 'BUENO' | 'REGULAR' | 'MALO';
}
```

### **Servicios API**

```typescript
// Métodos específicos para conductores
api.getPedidosConductor(idPlanta, idConductor)
api.getPedidoConductor(idPlanta, idPedido)
api.actualizarEstadoPedidoConductor(idPlanta, idPedido, estado)
api.subirEvidenciaEstado(idPlanta, idPedido, formData)
api.reportarNovedad(idPlanta, idPedido, formData)
api.guardarChecklistVehiculo(idPlanta, checklist)
api.cambiarPasswordConductor(idConductor, passwordActual, passwordNuevo)
api.enviarUbicacionTiempoReal(idPlanta, idConductor, ubicacion)
api.verificarPrimerLogin(idConductor)
```

## 🔒 **Seguridad y Validaciones**

### **Autenticación**
- Firebase Authentication para login seguro
- Tokens JWT para validación de sesión
- Verificación de roles en cada ruta

### **Autorización**
- Conductores solo pueden ver sus pedidos asignados
- No pueden crear, modificar o eliminar pedidos
- Acceso restringido a funcionalidades específicas

### **Validaciones de Formularios**
- Contraseñas con requisitos de seguridad
- Imágenes con validación de tipo y tamaño
- Campos obligatorios en reportes y evidencias

### **Permisos de Ubicación**
- Solicitud explícita de permisos de geolocalización
- Manejo de casos de denegación
- Fallback para navegadores sin soporte

## 📱 **Experiencia de Usuario**

### **Diseño Responsivo**
- Optimizado para dispositivos móviles
- Interfaz intuitiva y fácil de usar
- Navegación clara y accesible

### **Feedback Visual**
- Estados de carga y progreso
- Notificaciones de éxito y error
- Indicadores de estado de conexión
- Colores diferenciados por estado de pedido

### **Accesibilidad**
- Contraste adecuado de colores
- Textos legibles
- Botones con tamaño apropiado
- Navegación por teclado

## 🚀 **Flujo de Trabajo Típico**

1. **Login del conductor** con credenciales
2. **Cambio de contraseña** (si es primer login)
3. **Visualización de pedidos** asignados para el día
4. **Selección de pedido** para gestionar
5. **Cambio de estados** con evidencia fotográfica
6. **Reporte de novedades** si es necesario
7. **Checklist de vehículo** al final de la jornada
8. **Seguimiento de ubicación** automático durante todo el proceso

## 🔧 **Configuración y Despliegue**

### **Variables de Entorno**
```bash
REACT_APP_API_BASE_URL=https://tu-backend.com
REACT_APP_FIREBASE_API_KEY=tu-api-key
REACT_APP_FIREBASE_AUTH_DOMAIN=tu-auth-domain
REACT_APP_FIREBASE_PROJECT_ID=tu-project-id
```

### **Dependencias Requeridas**
```json
{
  "react": "^18.0.0",
  "react-router-dom": "^6.0.0",
  "firebase": "^9.0.0",
  "axios": "^1.0.0",
  "react-hot-toast": "^2.0.0",
  "lucide-react": "^0.263.0"
}
```

### **Permisos del Navegador**
- Geolocalización
- Notificaciones (opcional)
- Almacenamiento local

## 📊 **Métricas y Monitoreo**

### **Datos Recopilados**
- Ubicación en tiempo real
- Estados de pedidos
- Evidencias fotográficas
- Reportes de novedades
- Checklist de vehículos

### **Análisis Disponible**
- Tiempo promedio por estado
- Frecuencia de novedades
- Estado general de vehículos
- Rutas y tiempos de entrega

## 🐛 **Solución de Problemas**

### **Problemas Comunes**

#### Error de Permisos de Ubicación
```javascript
// Verificar permisos
navigator.permissions.query({ name: 'geolocation' })
  .then(result => {
    if (result.state === 'denied') {
      // Mostrar instrucciones para habilitar
    }
  });
```

#### Error de Conexión
- Reintentos automáticos
- Modo offline con sincronización posterior
- Indicadores visuales de estado

#### Error de Subida de Imágenes
- Validación de tipo de archivo
- Límite de tamaño (5MB)
- Compresión automática si es necesario

### **Logs y Debugging**
```javascript
// Habilitar logs en desarrollo
if (process.env.NODE_ENV === 'development') {
  console.log('Ubicación enviada:', ubicacion);
}
```

## 🔄 **Actualizaciones Futuras**

### **Funcionalidades Planificadas**
- Notificaciones push
- Modo offline completo
- Integración con GPS del vehículo
- Reconocimiento de voz para reportes
- Escaneo de códigos QR para confirmación

### **Mejoras de Rendimiento**
- Caché de datos offline
- Optimización de imágenes
- Lazy loading de componentes
- Compresión de datos de ubicación

---

**🎉 ¡Interfaz de conductor completamente funcional y lista para uso en producción!** 