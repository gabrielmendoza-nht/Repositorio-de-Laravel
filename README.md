<div align="center">

# 🔐 Laboratorio #2 – Implementación del Login en Laravel

**Universidad Tecnológica de Panamá**
Facultad de Ingeniería de Sistemas Computacionales · Campus Víctor Levis Sasso
**Desarrollo Web** · II Semestre 2026

![Laravel](https://img.shields.io/badge/Laravel-13.37-FF2D20?logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.x-777BB4?logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-phpMyAdmin-4479A1?logo=mysql&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8.3-646CFF?logo=vite&logoColor=white)
![WampServer](https://img.shields.io/badge/WampServer-Windows-orange)

</div>

---

## 📑 Tabla de contenido

1. [Introducción y objetivo](#1--introducción-y-objetivo)
2. [Requisitos previos](#2--requisitos-previos)
3. [Instalación y flujo de comandos](#3--instalación-y-flujo-de-comandos)
4. [Base de datos](#4--base-de-datos)
5. [Resultado del laboratorio](#5--resultado-del-laboratorio)
6. [Dificultades y soluciones](#6--dificultades-y-soluciones)
7. [Referencias](#7--referencias)
8. [Fecha de ejecución](#8--fecha-de-ejecución)
9. [Información del estudiante](#9--información-del-estudiante)

---

## 1. 📖 Introducción y objetivo

**Objetivo del laboratorio:** configurar un proyecto Laravel en un entorno local (WampServer), conectarlo a una base de datos MySQL, ejecutar las migraciones y dejar listo el entorno para implementar el módulo de login, aplicando la arquitectura **Modelo–Vista–Controlador (MVC)**.

### Arquitectura MVC en Laravel

| Capa | Carpeta | Función |
|------|---------|---------|
| **Modelo** | `app/Models/` | Representa las tablas de la base de datos y su lógica (ej. `User.php`). |
| **Vista** | `resources/views/` | Plantillas Blade (HTML) que ve el usuario. |
| **Controlador** | `app/Http/Controllers/` | Recibe la petición, usa los modelos y devuelve una vista o respuesta. |
| **Rutas** | `routes/web.php` | Define qué URL ejecuta cada controlador o vista. |

Otras carpetas importantes:

- `database/migrations/` → define la estructura de las tablas.
- `config/` y `.env` → configuración de la aplicación y de la conexión a la base de datos.
- `app/Providers/` → proveedores de servicios (ej. `AppServiceProvider.php`).
- `public/` → punto de entrada del servidor web.

### Comandos usados para las migraciones

```bash
php artisan migrate          # ejecuta las migraciones pendientes
php artisan migrate:fresh    # elimina todas las tablas y las vuelve a crear
```

---

## 2. 🧰 Requisitos previos

| Tecnología | Detalle |
|:---:|---|
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/windows8/windows8-original.svg" width="32"/> | **Sistema operativo:** Windows |
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/php/php-original.svg" width="32"/> | **PHP:** `[POR COMPLETAR: resultado de php -v]` |
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/composer/composer-original.svg" width="32"/> | **Composer:** última versión estable `[POR COMPLETAR: composer -V]` |
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/laravel/laravel-original.svg" width="32"/> | **Laravel Installer 5.32.0** · **Laravel Framework 13.37.0** (`laravel new`) |
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/apache/apache-original.svg" width="32"/> | **Servidor web:** Apache (incluido en WampServer) |
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" width="32"/> | **Base de datos:** MySQL administrada con phpMyAdmin (usuario `root`) |
| 🟧 | **Entorno de desarrollo local:** WampServer (`C:\wamp64\www`) |
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vscode/vscode-original.svg" width="32"/> | **Editor de código:** Visual Studio Code |
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/npm/npm-original-wordmark.svg" width="32"/> | **NPM:** sí, necesario (`npm install` y `npm run dev` con **Vite 8.3.4**) |

---

## 3. ⚙️ Instalación y flujo de comandos

### 3.1 Crear el proyecto

Desde PowerShell, dentro de la carpeta `www` de WampServer:

```bash
cd C:\wamp64\www
laravel new lab_laravel
```

Durante el asistente se eligió: *starter kit* **none** y *frontend stack* **Blade**.

![Creación del proyecto](imagenes/01_laravel_new.png)

### 3.2 Configurar el archivo `.env`

Configuración de la aplicación:

```env
APP_NAME=Laravel
APP_ENV=local
APP_DEBUG=true
APP_URL=http://lab_laravel
```

Configuración de la conexión a la base de datos:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

![.env aplicación](imagenes/06_env_app.png)
![.env base de datos](imagenes/05_env_base_de_datos.png)

### 3.3 Ajuste en `AppServiceProvider.php`

Para evitar errores de longitud de índices en MySQL se limitó la longitud por defecto de los *strings* a 191 caracteres en `app/Providers/AppServiceProvider.php`:

```php
use Illuminate\Support\Facades\Schema;

public function boot(): void
{
    Schema::defaultStringLength(191);
}
```

![AppServiceProvider](imagenes/07_app_service_provider.png)

### 3.4 Limpiar caché y actualizar dependencias

```bash
php artisan config:clear
php artisan config:cache
composer update
```

![config:clear y composer update](imagenes/03_config_clear_composer_update.png)
![config:cache](imagenes/08_config_cache.png)

### 3.5 Migraciones

```bash
php artisan migrate
```

Laravel detectó que la base de datos `laravel` no existía y ofreció crearla (se respondió `yes`).

![migrate crea la BD](imagenes/02_migrate_crear_bd.png)

### 3.6 Dependencias de frontend (NPM + Vite)

```bash
npm install
npm run dev
```

El servidor de Vite queda disponible en `http://localhost:5173/`.

![npm install y npm run dev](imagenes/09_npm_install_run_dev.png)
![Vite en cmd](imagenes/10_vite_en_cmd.png)

### 3.7 Resumen del flujo completo

```bash
cd C:\wamp64\www
laravel new lab_laravel
cd lab_laravel
# configurar .env (base de datos)
php artisan config:clear
composer update
php artisan migrate
php artisan migrate:fresh
npm install
npm run dev
```

### 3.8 Paquete de autenticación

> `[POR COMPLETAR: indicar si se usó laravel/ui, Breeze u otro y los comandos usados]`
>
> Ejemplos de referencia (usar solo el que corresponda):
>
> ```bash
> # Opción A: Laravel UI
> composer require laravel/ui
> php artisan ui bootstrap --auth
> npm install && npm run dev
>
> # Opción B: Breeze
> composer require laravel/breeze --dev
> php artisan breeze:install
> ```

---

## 4. 🗄️ Base de datos

- **Motor:** MySQL (WampServer), administrada desde **phpMyAdmin**.
- **Nombre de la base de datos:** `laravel`
- **Usuario:** `root` (sin contraseña en el entorno local).
- **Conexión:** definida en el archivo `.env` (`DB_CONNECTION`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`).

### Migraciones y tablas

Las migraciones están en `database/migrations/` y crean las tablas necesarias del laboratorio:

| Migración | Tabla |
|-----------|-------|
| `create_users_table` | `users` (y tablas auxiliares de autenticación) |
| `create_cache_table` | `cache` |
| `create_jobs_table` | `jobs` |

Comandos usados:

```bash
php artisan migrate         # ejecutar migraciones
php artisan migrate:fresh   # reiniciar y recrear todas las tablas
```

![migrate:fresh](imagenes/12_migrate_fresh.png)

### 💾 Respaldo (backup) de la base de datos

Exportar desde **phpMyAdmin**: seleccionar la base `laravel` → pestaña **Exportar** → método *Rápido*, formato *SQL* → **Exportar**.

Guardar el archivo en el repositorio, por ejemplo en:

```
database/backup/laravel.sql
```

> `[POR COMPLETAR: subir el archivo laravel.sql al repositorio]`

---

## 5. 🖼️ Resultado del laboratorio

Vista obtenida en el navegador al ejecutar el proyecto (Laravel v13.37.0):

![Resultado](imagenes/15_resultado_dashboard.png)

---

## 6. 🛠️ Dificultades y soluciones

### Dificultad 1: `Illuminate\Database\QueryException` al ejecutar las migraciones

Al correr `php artisan migrate`, la migración `create_users_table` falló con el error *"Base table or view already exists: 1050 Table 'users' already exists"*, porque la tabla ya había sido creada en un intento anterior. Más adelante apareció otro `QueryException` durante las migraciones.

![Error 1](imagenes/11_error_query_exception_users.png)
![Error 2](imagenes/13_error_query_exception_2.png)

**Solución:** se reinició la base de datos con:

```bash
php artisan migrate:fresh
```

Este comando elimina todas las tablas y vuelve a ejecutar todas las migraciones desde cero. Adicionalmente se configuró `Schema::defaultStringLength(191);` en `AppServiceProvider`.

### Dificultad 2: `"vite" no se reconoce como un comando interno o externo`

Al ejecutar `npm run dev` apareció el error de que `vite` no se reconocía, lo que indica que las dependencias de Node no estaban instaladas correctamente.

![Error Vite](imagenes/14_error_vite_no_reconocido.png)

**Solución:** ejecutar `npm install` dentro de la carpeta del proyecto y luego `npm run dev` nuevamente.

### Dificultad 3: Error de escritura en el comando de artisan

Se escribió mal el comando (`php artisian config::clear`) y no se ejecutó.

![Error tipográfico](imagenes/04_error_tipografico_artisan.png)

**Solución:** corregirlo a `php artisan config:clear`.

---

## 7. 📚 Referencias

1. Laravel. *Documentación oficial – Database: Migrations*. <https://laravel.com/docs/migrations>
2. Laravel. *Documentación oficial – Configuration y Database*. <https://laravel.com/docs/database>
3. Laravel. *Starter Kits (autenticación)*. <https://laravel.com/docs/starter-kits>
4. Composer. *Documentación oficial*. <https://getcomposer.org/doc/>
5. Vite. *Guía oficial*. <https://vite.dev/guide/>
6. WampServer. *Sitio oficial*. <https://www.wampserver.com/en/>

---

## 8. 📅 Fecha de ejecución

**22 de octubre de 2026**

---

## 9. 👤 Información del estudiante

<div align="center">

Este laboratorio ha sido desarrollado por el estudiante de la **Universidad Tecnológica de Panamá**:

**Nombre:** Gabriel Mendoza
**Correo:** `[POR COMPLETAR]`
**Curso:** Desarrollo Web 2
**Instructor del Laboratorio:** Irina Fong

</div>
