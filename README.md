# 📘 Sistema de Gestión con Laravel - Roles y Permisos

Sistema completo desarrollado en **Laravel 11** y **PHP 8.2+** que incluye gestión de áreas, roles, permisos, usuarios y generación de certificados digitales con código QR único.

## 🚀 Características Principales

- ✅ **Autenticación y Autorización** con roles y permisos
- ✅ **Usuario Root** preconfigurado para acceso administrativo inicial
- ✅ **Gestión de Certificados** con plantillas personalizables por área
- ✅ **Generación de PDFs** con QR único para verificación
- ✅ **Importación masiva** de certificados desde Excel
- ✅ **Sistema de áreas** con plantillas independientes
- ✅ **Envío automático** de certificados por email
- ✅ **Interfaz AdminLTE** profesional y responsive

---

## 📋 Requisitos del Sistema

| Tecnología | Versión Mínima |
|------------|----------------|
| PHP | 8.2 o superior |
| Composer | 2.x |
| MySQL/MariaDB | 5.7+ / 10.3+ |
| Node.js | 18.x o superior |
| NPM | 9.x o superior |

### Extensiones PHP Requeridas:
- `ext-gd` (para generación de QR)
- `ext-zip`
- `ext-mbstring`
- `ext-pdo_mysql`
- `ext-dom`

---

## ⚙️ Instalación Paso a Paso

### 1️⃣ Clonar el Repositorio

```bash
git clone https://github.com/tu-usuario/tu-proyecto.git
cd tu-proyecto
```

### 2️⃣ Instalar Dependencias

```bash
# Dependencias de PHP
composer install

# Dependencias de Node.js
npm install
```

### 3️⃣ Configurar Variables de Entorno

```bash
# Copiar archivo de ejemplo
cp .env.example .env

# Generar clave de aplicación
php artisan key:generate
```

Edita el archivo `.env` con tu configuración:

```env
APP_NAME="Sistema de Certificados"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nombre_base_datos
DB_USERNAME=tu_usuario
DB_PASSWORD=tu_contraseña

MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=tu_email@gmail.com
MAIL_PASSWORD=tu_contraseña_app
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=noreply@tusistema.com
MAIL_FROM_NAME="${APP_NAME}"
```

### 4️⃣ Configurar Base de Datos


# Ejecutar migraciones y seeders
php artisan migrate --seed
```

### 5️⃣ Crear Enlace Simbólico de Storage

```bash
php artisan storage:link
```

### 6️⃣ Compilar Assets

```bash
# Desarrollo
npm run dev

# Producción
npm run build
```

### 7️⃣ Dar Permisos (Linux/Mac)

```bash
sudo chmod -R 775 storage bootstrap/cache
sudo chown -R $USER:www-data storage bootstrap/cache
```

---

## 🖥️ Iniciar el Proyecto

### Modo Desarrollo

```bash
# Servidor Laravel
php artisan serve

# Compilación automática de assets (en otra terminal)
npm run dev
```

Accede a: **http://localhost:8000**

### Modo Producción

```bash
# Optimizar configuración
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Compilar assets
npm run build
```

---

## 👤 Usuario Root (Acceso Inicial)

| Campo | Valor |
|-------|-------|
| **Email** | `root@gmail.com` |
| **Contraseña** | `12345678` |
| **Rol** | Root (Acceso Total) |

> ⚠️ **IMPORTANTE**: Cambia esta contraseña después del primer inicio de sesión por seguridad.

---

## 📦 Configuración Inicial del Sistema

### 1. Crear Áreas

1. Accede con el usuario Root
2. Ve a **Gestión de Áreas**
3. Crea un área (ej: "Recursos Humanos")
4. Configura las plantillas de certificados:

**Plantilla Frontal (`template_front`):**
```
certificates.pdf_template_front
```

**Plantilla Trasera (`template_back`):**
```
certificates.pdf_template_back
```

### 2. Crear Cursos

1. Ve a **Gestión de Cursos**
2. Asigna cada curso a un área
3. Define duración (horas) y otros datos

### 3. Registrar Personas

1. Ve a **Gestión de Personas**
2. Registra participantes con DNI, nombre, email, etc.

### 4. Generar Certificados

**Opción A: Individual**
1. Ve a **Certificados → Crear Nuevo**
2. Selecciona curso y persona
3. Completa datos del CUV
4. Genera certificado

**Opción B: Importación Masiva**
1. Descarga la plantilla Excel
2. Completa los datos
3. Importa el archivo
4. Los certificados se generan automáticamente

---

## 🔧 Comandos Útiles

```bash
# Limpiar cachés
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear

# Ver rutas del sistema
php artisan route:list

# Crear nuevo seeder
php artisan make:seeder NombreSeeder

# Ejecutar seeder específico
php artisan db:seed --class=NombreSeeder

# Recrear base de datos desde cero
php artisan migrate:fresh --seed

# Ver logs en tiempo real
tail -f storage/logs/laravel.log

# Ejecutar queue workers (si usas colas)
php artisan queue:work
```

---

## 📧 Configuración de Email

Para Gmail, necesitas crear una **Contraseña de Aplicación**:

1. Ve a tu cuenta de Google
2. Seguridad → Verificación en dos pasos
3. Contraseñas de aplicaciones
4. Genera una nueva contraseña
5. Úsala en `MAIL_PASSWORD` del `.env`

---

## 🧩 Archivos Clave del Sistema

### DatabaseSeeder.php

```php
public function run(): void
{
    $this->call(RoleSeeder::class);

    $rootRole = Role::where('name', 'Root')->first();

    User::create([
        'name' => 'Root',
        'email' => 'root@gmail.com',
        'password' => Hash::make('12345678'),
        'role_id' => $rootRole->id,
    ]);
}
```

### RoleSeeder.php

```php
public function run(): void
{
    $roles = [
        ['name' => 'Root', 'description' => 'Acceso total al sistema'],
        ['name' => 'Administrador', 'description' => 'Gestión de áreas'],
        ['name' => 'Persona', 'description' => 'Acceso limitado'],
    ];

    foreach ($roles as $role) {
        Role::create($role);
    }
}
```

---

## 🎨 Personalización de Plantillas

Las plantillas de certificados se encuentran en:

```
resources/views/certificates/
├── pdf_template_front.blade.php  # Cara frontal
└── pdf_template_back.blade.php   # Cara trasera
```

Puedes crear múltiples plantillas y asignarlas por área en la base de datos.

---

## 🔐 Seguridad y Mejores Prácticas

### ✅ Recomendaciones

1. **Cambiar credenciales del usuario Root** inmediatamente
2. **Usar HTTPS en producción** (certificado SSL/TLS)
3. **Configurar firewall** para proteger base de datos
4. **Habilitar rate limiting** en rutas de login
5. **Realizar backups regulares** de la base de datos
6. **No versionar el archivo `.env`** (ya está en `.gitignore`)
7. **Actualizar dependencias** regularmente:
   ```bash
   composer update
   npm update
   ```

### ⚠️ No hacer en Producción

- ❌ No usar `APP_DEBUG=true`
- ❌ No exponer rutas de desarrollo
- ❌ No usar credenciales por defecto
- ❌ No dar permisos `777` a directorios

---

## 📊 Base de Datos

### Tablas Principales

| Tabla | Descripción |
|-------|-------------|
| `users` | Usuarios del sistema |
| `roles` | Roles disponibles |
| `areas` | Áreas organizacionales |
| `courses` | Cursos registrados |
| `persons` | Personas participantes |
| `certificates` | Certificados emitidos |

### Diagrama de Relaciones

```
users (n) ──→ (1) roles
courses (n) ──→ (1) areas
certificates (n) ──→ (1) courses
certificates (n) ──→ (1) persons
```

---

## 🐛 Solución de Problemas Comunes

### Error: "Forbidden" al acceder a PDFs

```bash
php artisan storage:link
sudo chmod -R 775 storage
```

### Error: "Class not found"

```bash
composer dump-autoload
php artisan clear-compiled
```

### Error al generar PDFs

```bash
composer require barryvdh/laravel-dompdf
composer require simplesoftwareio/simple-qrcode
```

### Errores de permisos

```bash
sudo chown -R $USER:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```

---

## 🤝 Contribución

1. **Fork** el proyecto
2. Crea tu **branch de feature**:
   ```bash
   git checkout -b feature/nueva-funcionalidad
   ```
3. **Commit** tus cambios:
   ```bash
   git commit -m "feat: Añadida nueva funcionalidad"
   ```
4. **Push** al branch:
   ```bash
   git push origin feature/nueva-funcionalidad
   ```
5. Abre un **Pull Request**

### Convenciones de Commits

```
feat: Nueva característica
fix: Corrección de bug
docs: Cambios en documentación
style: Formato de código
refactor: Refactorización
test: Añadir tests
chore: Tareas de mantenimiento
```

---

## 📝 Notas de Versión

### v1.0.0 (2025-11-26)
- ✅ Sistema base de autenticación
- ✅ Gestión de certificados
- ✅ Generación de PDFs con QR
- ✅ Importación masiva desde Excel
- ✅ Envío automático de emails

---

## 📄 Licencia

Este proyecto se distribuye bajo la licencia **MIT**.

```
MIT License

Copyright (c) 2025 Tu Nombre

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

---


**¡Gracias por usar nuestro sistema! 🚀**