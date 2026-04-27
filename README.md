# Moda Kids — Tienda Online Guatemala

Tienda online de ropa infantil con panel de administración completo. Desarrollada con HTML + CSS + JavaScript puro, base de datos en Supabase y deploy automático en Vercel.

---

## Demo en vivo

- **Tienda pública:** https://demo-tienda-online-sigma.vercel.app
- **Panel admin:** https://demo-tienda-online-sigma.vercel.app/admin.html

---

## Características principales

### Tienda pública
- Hero interactivo con 3 slides, fotos reales y avance automático
- Catálogo de productos con búsqueda, filtros por categoría y ordenamiento
- Carrito persistente (sobrevive recargas de página)
- Detalle de producto con galería y selector de talla y cantidad
- Descuento de stock en tiempo real al agregar al carrito
- Devolución de stock al cancelar o quitar productos
- Confirmación de compra por WhatsApp con resumen detallado
- Validación de teléfono guatemalteco
- Diseño responsive para móvil, tablet y escritorio
- Botón directo de WhatsApp por producto

### Panel de administración
- Login con sesión persistente
- Dashboard con estadísticas, ventas del día, mes y acumuladas
- Gestión completa de pedidos con estados y filtros por columna
- Gestión de productos con subida de fotos a Supabase Storage
- Historial de movimientos de inventario (entradas y salidas)
- Contacto directo al cliente por WhatsApp desde el admin

---

## Stack tecnológico

| Tecnología | Uso | Costo |
|---|---|---|
| HTML + CSS + JS | Frontend completo | Gratis |
| Supabase | Base de datos + Storage | Gratis |
| Vercel | Hosting y deploy automático | Gratis |
| GitHub | Repositorio de código | Gratis |

---

## Estructura del proyecto

```
demo-tiendaOnline/
├── index.html       # Tienda pública completa
├── admin.html       # Panel de administración
├── .env.example     # Variables de entorno de referencia
├── .gitignore       # Archivos excluidos del repo
└── README.md        # Este archivo
```

---

## Configuración rápida

### 1. Clonar el repositorio
```bash
git clone https://github.com/rbarrientos1990/demo-tiendaOnline.git
cd demo-tiendaOnline
```

### 2. Crear proyecto en Supabase
- Entrar a https://supabase.com y crear un proyecto nuevo
- Ejecutar el SQL de creación de tablas (ver sección abajo)
- Crear un bucket público llamado `fotos`

### 3. Configurar las credenciales
En `index.html` y `admin.html`, reemplazar:
```javascript
const SUPABASE_URL = 'TU_PROJECT_URL'
const SUPABASE_KEY = 'TU_ANON_KEY'
```

### 4. Configurar el número de WhatsApp
En `index.html`, reemplazar:
```javascript
const WA_NUMBER = 'TU_NUMERO_SIN_ESPACIOS'  // Ej: 50212345678
```

### 5. Deploy en Vercel
- Conectar el repositorio de GitHub a https://vercel.com
- El deploy es automático en cada `git push`

---

## Tablas de base de datos (SQL)

```sql
-- Tabla de productos
create table productos (
  id uuid default gen_random_uuid() primary key,
  nombre text not null,
  categoria text not null,
  precio numeric not null,
  stock integer not null default 0,
  tallas text,
  imagen_url text,
  created_at timestamp with time zone default now()
);

-- Tabla de pedidos
create table pedidos (
  id uuid default gen_random_uuid() primary key,
  cliente_nombre text not null,
  whatsapp text,
  departamento text,
  producto_nombre text,
  talla text,
  estado text default 'nuevo',
  created_at timestamp with time zone default now()
);

-- Tabla de movimientos de inventario
create table movimientos (
  id uuid default gen_random_uuid() primary key,
  producto_id uuid references productos(id),
  tipo text not null,
  cantidad integer not null,
  stock_anterior integer,
  stock_nuevo integer,
  motivo text,
  created_at timestamp with time zone default now()
);
```

---

## Políticas RLS en Supabase

```sql
-- Habilitar RLS en todas las tablas
alter table productos enable row level security;
alter table pedidos enable row level security;
alter table movimientos enable row level security;

-- Políticas para productos
create policy "Lectura pública" on productos for select to anon using (true);
create policy "Insertar" on productos for insert to anon with check (true);
create policy "Actualizar" on productos for update to anon using (true) with check (true);
create policy "Eliminar" on productos for delete to anon using (true);

-- Políticas para pedidos
create policy "Lectura" on pedidos for select to anon using (true);
create policy "Insertar" on pedidos for insert to anon with check (true);
create policy "Eliminar" on pedidos for delete to anon using (true);

-- Políticas para movimientos
create policy "Lectura" on movimientos for select to anon using (true);
create policy "Insertar" on movimientos for insert to anon with check (true);
create policy "Eliminar" on movimientos for delete to anon using (true);
```

---

## Credenciales del panel admin (demo)

| Usuario | Contraseña |
|---|---|
| admin | demo2026 |
| ronald | tienda2026 |

> Para producción cambiar estas credenciales directamente en `admin.html`

---

## Desarrollado por

**Ronald Barrientos**  
Infraestructura & Desarrollo Web — Guatemala  
barrientosronald1990@gmail.com  
+502 5048-1469

---

*Proyecto disponible como servicio de desarrollo personalizado para negocios en Guatemala.*
