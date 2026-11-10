# Outlook Add-in Static Assets & Vercel CDN Delivery
## Repositorio de Publicación de Activos para Outlook Add-in (Bajaworks)

Este proyecto aloja y entrega los activos estáticos públicos (iconografía multi-dispositivo, taskpane de respaldo y polyfills) consumidos por el complemento de Microsoft Outlook (`outlookAddin.bajaworks.manifest.xml`).

---

## 1. Repositorio Remoto y Despliegue en Vercel

* **Repositorio GitHub:** [https://github.com/helpdsp/rc_outlook-addin-spfx.git](https://github.com/helpdsp/rc_outlook-addin-spfx.git)
* **Entorno de Producción (Vercel):** [https://rc-outlook-addin-spfx.vercel.app](https://rc-outlook-addin-spfx.vercel.app)
* **Ubicación en el Workspace:** `C:\DATA\Repos\helpdesk-workspace\apps\rc_outlook-addin-spfx`

### Flujo de Despliegue Continuo (CI/CD)
Cualquier cambio, adición o actualización de activos gráficos en la carpeta `public/assets/` se publica automáticamente en Vercel al hacer push a la rama principal:

```bash
cd apps/rc_outlook-addin-spfx
git add .
git commit -m "feat(icons): actualizar iconografia oficial Bajaworks multi-dispositivo"
git push origin main
```

En cuanto GitHub recibe el push, Vercel dispara el despliegue automático y los activos quedan inmediatamente disponibles en `https://rc-outlook-addin-spfx.vercel.app/assets/...`.

---

## 2. Estructura del Proyecto

```text
apps/rc_outlook-addin-spfx/
├── public/
│   ├── assets/               # Iconos corporativos oficiales de Bajaworks
│   │   ├── icon-16.png       # Desktop Ribbon pequeño
│   │   ├── icon-25.png       # Mobile/iPad barra de lectura (scale="1")
│   │   ├── icon-32.png       # Desktop estándar
│   │   ├── icon-48.png       # Mobile Bottom Sheet (scale="1")
│   │   ├── icon-50.png       # iPad Retina / iPhone (scale="2")
│   │   ├── icon-64.png       # App Catalog / Mobile scale="2"
│   │   ├── icon-75.png       # iPhone Pro / Android OLED (scale="3")
│   │   ├── icon-80.png       # Desktop Ribbon grande
│   │   ├── icon-96.png       # Retina scale="3" / scale="2"
│   │   ├── icon-128.png      # App Catalog High Resolution
│   │   ├── icon-144.png      # Mobile Ultra HD scale="3"
│   │   ├── icon-192.png      # PWA / Android
│   │   ├── icon-512.png      # Master Icon
│   │   └── helpdesk-logo.svg # Vector maestro SVG
│   ├── commands.html         # Endpoint de comandos de Outlook
│   ├── taskpane.html         # Taskpane HTML de respaldo
│   └── polyfill.js           # Polyfills para motores WebView antiguos
├── vercel.json               # Configuración de ruteo y headers CORS para Vercel
├── package.json
└── README.md
```

---

## 3. Consumo en los Manifiestos de Outlook

Los manifiestos XML (`dist/deploy/bajaworks/outlookAddin.bajaworks.manifest.xml`) referencian estos activos mediante la URL pública de Vercel:

```xml
<IconUrl DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/icon-64.png"/>
<HighResolutionIconUrl DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/icon-128.png"/>
```

Y en los recursos de imagen para Desktop y Móvil:

```xml
<bt:Image id="Icon.16x16" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/icon-16.png"/>
<bt:Image id="Icon.32x32" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/icon-32.png"/>
<bt:Image id="Icon.80x80" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/icon-80.png"/>
<bt:Image id="Mobile.Icon25" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/icon-25.png"/>
<bt:Image id="Mobile.Icon48" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/icon-48.png"/>
```

