# Convenciones de Diseño y Estructura - Módulo Cliente

## 📁 Estructura de Carpetas

### Organización Principal
```
src/
├── components/     # Componentes reutilizables
│   ├── ui/        # Componentes de interfaz básicos
│   ├── forms/     # Componentes de formularios
│   └── layout/    # Componentes de estructura
├── pages/         # Páginas principales
├── services/      # Servicios de API y Firebase
├── contexts/      # Contextos de React
├── types/         # Definiciones de tipos TypeScript
├── utils/         # Utilidades y helpers
└── config/        # Configuraciones
```

### Convenciones de Nomenclatura

#### Archivos y Carpetas
- **PascalCase** para componentes: `OrderCard.tsx`, `LoginForm.tsx`
- **camelCase** para utilidades: `dateUtils.ts`, `validationHelpers.ts`
- **kebab-case** para archivos de configuración: `firebase-config.ts`

#### Componentes
- **Prefijo descriptivo**: `ClientOrderCard`, `AdminDashboard`
- **Sufijo por tipo**: `LoginForm`, `OrderList`, `MapView`

## 🎨 Convenciones de Diseño

### Colores (TailwindCSS)
```css
/* Colores principales */
--primary: #2563eb;      /* Azul principal */
--secondary: #64748b;    /* Gris secundario */
--success: #10b981;      /* Verde éxito */
--warning: #f59e0b;      /* Amarillo advertencia */
--danger: #ef4444;       /* Rojo error */
--info: #3b82f6;         /* Azul información */

/* Estados */
--pending: #f59e0b;      /* Pendiente */
--in-progress: #3b82f6;  /* En progreso */
--completed: #10b981;    /* Completado */
--cancelled: #ef4444;    /* Cancelado */
```

### Espaciado
- **Base**: `4px` (0.25rem)
- **Múltiplos**: `8px`, `12px`, `16px`, `20px`, `24px`, `32px`
- **Responsive**: `sm:`, `md:`, `lg:`, `xl:`

### Tipografía
```css
/* Jerarquía de textos */
--text-xs: 0.75rem;     /* 12px */
--text-sm: 0.875rem;    /* 14px */
--text-base: 1rem;      /* 16px */
--text-lg: 1.125rem;    /* 18px */
--text-xl: 1.25rem;     /* 20px */
--text-2xl: 1.5rem;     /* 24px */
--text-3xl: 1.875rem;   /* 30px */
```

## 💻 Convenciones de Código

### TypeScript

#### Interfaces y Tipos
```typescript
// Interfaces para modelos de datos
interface Client {
  id: string;
  name: string;
  email: string;
  phone: string;
  address: string;
}

// Tipos para props de componentes
type OrderCardProps = {
  order: Order;
  onEdit?: (order: Order) => void;
  onDelete?: (orderId: string) => void;
};

// Enums para estados
enum OrderStatus {
  PENDING = 'PENDING',
  IN_PROGRESS = 'IN_PROGRESS',
  COMPLETED = 'COMPLETED',
  CANCELLED = 'CANCELLED'
}
```

#### Funciones
```typescript
// Funciones puras
const formatDate = (date: Date): string => {
  return date.toLocaleDateString('es-ES');
};

// Funciones async
const fetchOrders = async (clientId: string): Promise<Order[]> => {
  try {
    const response = await api.get(`/orders/${clientId}`);
    return response.data;
  } catch (error) {
    console.error('Error fetching orders:', error);
    throw error;
  }
};

// Hooks personalizados
const useOrders = (clientId: string) => {
  const [orders, setOrders] = useState<Order[]>([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetchOrders(clientId)
      .then(setOrders)
      .finally(() => setLoading(false));
  }, [clientId]);
  
  return { orders, loading };
};
```

### React Components

#### Estructura de Componentes
```typescript
// Componente funcional con TypeScript
interface OrderCardProps {
  order: Order;
  onEdit?: (order: Order) => void;
}

const OrderCard: React.FC<OrderCardProps> = ({ order, onEdit }) => {
  // Hooks al inicio
  const [isExpanded, setIsExpanded] = useState(false);
  
  // Handlers
  const handleEdit = () => {
    onEdit?.(order);
  };
  
  // Render
  return (
    <div className="bg-white rounded-lg shadow-md p-4">
      {/* Contenido del componente */}
    </div>
  );
};

export default OrderCard;
```

#### Naming de Props
- **Booleanos**: `isLoading`, `hasError`, `canEdit`
- **Eventos**: `onClick`, `onSubmit`, `onChange`
- **Datos**: `order`, `client`, `user`

### CSS y TailwindCSS

#### Clases Organizadas
```jsx
// Orden recomendado: Layout > Spacing > Typography > Colors > Effects
<div className="
  flex items-center justify-between
  p-4 mb-4
  text-lg font-semibold
  bg-white text-gray-900
  rounded-lg shadow-md
">
```

#### Responsive Design
```jsx
// Mobile-first approach
<div className="
  w-full
  md:w-1/2
  lg:w-1/3
  xl:w-1/4
">
```

## 🔧 Convenciones de Configuración

### Variables de Entorno
```bash
# Firebase
REACT_APP_FIREBASE_API_KEY=
REACT_APP_FIREBASE_AUTH_DOMAIN=
REACT_APP_FIREBASE_PROJECT_ID=

# API
REACT_APP_API_URL=http://localhost:8080

# Maps
REACT_APP_GOOGLE_MAPS_API_KEY=
```

### Imports Organizados
```typescript
// 1. React y librerías externas
import React, { useState, useEffect } from 'react';
import { useNavigate } from 'react-router-dom';

// 2. Componentes internos
import OrderCard from '../components/OrderCard';
import LoadingSpinner from '../components/ui/LoadingSpinner';

// 3. Hooks y contextos
import { useAuth } from '../contexts/AuthContext';
import { useOrders } from '../hooks/useOrders';

// 4. Servicios
import { orderService } from '../services/orderService';

// 5. Tipos y utilidades
import { Order, OrderStatus } from '../types';
import { formatDate } from '../utils/dateUtils';
```

## 📝 Convenciones de Documentación

### Comentarios
```typescript
/**
 * Componente para mostrar una tarjeta de pedido
 * @param order - Datos del pedido a mostrar
 * @param onEdit - Callback opcional para editar el pedido
 */
const OrderCard: React.FC<OrderCardProps> = ({ order, onEdit }) => {
  // Lógica del componente
};
```

### README por Carpeta
Cada carpeta principal debe tener un `README.md` explicando:
- Propósito de la carpeta
- Tipos de archivos que contiene
- Convenciones específicas
- Ejemplos de uso

## 🚀 Convenciones de Despliegue

### Scripts de Package.json
```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "lint": "eslint src --ext .ts,.tsx",
    "lint:fix": "eslint src --ext .ts,.tsx --fix"
  }
}
```

### Estructura de Build
```
build/
├── static/
│   ├── css/
│   ├── js/
│   └── media/
├── asset-manifest.json
└── index.html
```

## ✅ Checklist de Calidad

### Antes de Commit
- [ ] Código sigue las convenciones de nomenclatura
- [ ] Componentes están tipados correctamente
- [ ] No hay console.log en producción
- [ ] Responsive design implementado
- [ ] Accesibilidad básica (alt, aria-labels)
- [ ] Manejo de errores implementado
- [ ] Loading states definidos

### Antes de Merge
- [ ] Tests pasan
- [ ] Linting sin errores
- [ ] Build exitoso
- [ ] Documentación actualizada
- [ ] Variables de entorno configuradas

---

**Estas convenciones aseguran consistencia y mantenibilidad en el código del módulo cliente.** 