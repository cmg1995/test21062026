# Dashboard Hortofrutícola

Dashboard operacional y financiero con datos compartidos entre usuarios vía Supabase.

## Estructura del proyecto

```
.
├── public/
│   └── index.html     ← el dashboard completo (HTML/JS/CSS en un solo archivo)
└── netlify.toml        ← configuración de Netlify (publish dir)
```

## Cómo subir a GitHub

1. Crea un repositorio nuevo en GitHub (puede estar vacío).
2. Desde esta carpeta descomprimida, ejecuta:
   ```bash
   git init
   git add .
   git commit -m "Dashboard hortofrutícola con Supabase"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
   git push -u origin main
   ```

## Cómo conectar con Netlify

1. Entra en [app.netlify.com](https://app.netlify.com) → **Add new site** → **Import an existing project**.
2. Conecta tu cuenta de GitHub y selecciona el repositorio.
3. Netlify detecta automáticamente el directorio `public` gracias a `netlify.toml`.
4. Pulsa **Deploy site**.

No hace falta configurar ninguna variable de entorno ni función: las credenciales de Supabase (URL del proyecto y clave pública `anon`) ya están incluidas directamente en `public/index.html`, igual que en cualquier app de Supabase pensada para uso sin autenticación de usuarios.

## Base de datos (Supabase)

El dashboard guarda y lee los datos de la tabla `dashboard_datos`, creada con este SQL (ejecutado una sola vez en el SQL Editor de Supabase):

```sql
create table dashboard_datos (
  tipo text primary key,
  datos jsonb not null,
  actualizado timestamptz not null default now()
);

alter table dashboard_datos disable row level security;

grant all on dashboard_datos to anon, authenticated;
```

Cada vez que alguien sube un Excel (operacional o financiero) desde el dashboard, se guarda/actualiza una fila en esta tabla (`tipo = 'operacional'` o `tipo = 'financiero'`), visible automáticamente para cualquiera que abra la misma URL del dashboard.

## Notas

- Si quieres reiniciar los datos guardados, simplemente sube un nuevo Excel desde el propio dashboard: sobrescribe los datos anteriores de ese módulo.
- Si en algún momento cambias de proyecto Supabase, solo tienes que actualizar las constantes `SUPA_URL` y `SUPA_KEY` al principio de la sección "GUARDADO/CARGA REMOTA" dentro de `public/index.html`.
