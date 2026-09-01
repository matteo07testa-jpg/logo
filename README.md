# Logo Overlay

Pon tu logo en la esquina inferior izquierda o derecha de cualquier vídeo.
Procesamiento 100% en el navegador con ffmpeg.wasm — 5–15× más rápido que en
tiempo real.

## Desarrollar localmente

```bash
bun install
bun run dev
```

Abre http://localhost:3000

## Desplegar en Vercel (gratis, recomendado)

Sigue estos pasos desde cero. Necesitas una cuenta gratuita en GitHub y en
Vercel.

### Paso 1: Crear un repositorio en GitHub

1. Ve a https://github.com/new
2. **Repository name:** `logo-overlay` (o el nombre que prefieras)
3. **Visibility:** Public (o Private, ambos funcionan en Vercel gratis)
4. **No** marques "Add a README" ni ningún `.gitignore` — ya los tienes.
5. Click **Create repository**
6. GitHub te mostrará comandos similares a estos. CóPIALOS pero NO los
   ejecutes todavía:

```
git remote add origin https://github.com/TU_USUARIO/logo-overlay.git
git branch -M main
git push -u origin main
```

### Paso 2: Subir el código al repositorio

Desde la raíz del proyecto (donde está `package.json`), ejecuta:

```bash
# Inicializa git si no estaba inicializado
git init

# Añade todos los archivos
git add .

# Crea el primer commit
git commit -m "Logo Overlay — versión inicial con ffmpeg.wasm"

# Conecta con tu repositorio de GitHub (cambia TU_USUARIO)
git remote add origin https://github.com/TU_USUARIO/logo-overlay.git
git branch -M main

# Sube el código
git push -u origin main
```

Te pedirá usuario y contraseña de GitHub. Usa un Personal Access Token como
contraseña (GitHub ya no acepta la contraseña normal para git push):
- Crea un token en https://github.com/settings/tokens/new
- Marca el scope `repo`
- Cópialo y pégalo cuando pida contraseña

### Paso 3: Conectar Vercel con GitHub

1. Ve a https://vercel.com/new
2. Inicia sesión con tu cuenta de GitHub (más fácil) o email
3. Click en **Import Git Repository**
4. Si no aparece tu repo, click **Adjust GitHub App Permissions** y dale acceso
5. Selecciona `TU_USUARIO/logo-overlay`
6. En la pantalla de configuración, Vercel detecta Next.js automáticamente.
   **No cambies nada** — los valores por defecto son correctos:
   - Framework: **Next.js**
   - Build Command: `next build` (o el que ponga)
   - Output Directory: `.next`
7. Click **Deploy**

Vercel tarda 1–2 minutos en construir. Cuando termine, te da una URL tipo
`logo-overlay-tUUSUARIO.vercel.app`. ¡Esa es la que le das a tu madre!

### Paso 4: Verificar

1. Entra a la URL de Vercel
2. Sube un vídeo + un logo
3. Click "Procesar vídeo"
4. La primera vez tardará un poco más (descarga ffmpeg ~30MB)
5. Si todo funciona, ¡listo!

## Notas técnicas

- **Headers COOP/COEP**: ya configurados en `next.config.ts`. Son necesarios
  para que ffmpeg.wasm pueda usar SharedArrayBuffer. Si los quitas, deja de
  funcionar.
- **ffmpeg core**: se carga desde unpkg (`@ffmpeg/core@0.12.10`).
  Vercel no requiere configuración extra.
- **Memoria**: vídeos muy grandes (1h+ o 4K) pueden agotar la memoria del
  navegador. Para uso normal (30 min, 1080p) va bien.
- **Privacidad**: el vídeo nunca se sube a un servidor. Todo ocurre en el
  navegador del usuario.

## Estructura

```
src/
├── app/
│   ├── layout.tsx          # Layout raíz
│   └── page.tsx            # Página principal con toda la lógica
├── components/
│   └── DropZone.tsx        # Zona de subida reutilizable
└── hooks/
    ├── use-ffmpeg.ts       # Hook para ffmpeg.wasm
    ├── use-toast.ts        # Toast de shadcn/ui
    └── use-mobile.ts       # Detección de móvil
next.config.ts              # COOP/COEP headers
```
