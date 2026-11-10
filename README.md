# Outlook Add-in Static Assets & Vercel CDN Delivery
## Repositorio de Publicación de Activos para Outlook Add-in (Multi-Tenant)

Este proyecto aloja y entrega los activos estáticos públicos (iconografía multi-dispositivo, taskpane de respaldo y polyfills) consumidos por el complemento de Microsoft Outlook.

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
git commit -m "feat(bajaworks): agregar suite oficial multi-dispositivo aislada en /assets/bajaworks"
git push origin main
```

En cuanto GitHub recibe el push, Vercel dispara el despliegue automático y los activos quedan inmediatamente disponibles en `https://rc-outlook-addin-spfx.vercel.app/assets/...`.

---

## 2. Estructura Multi-Tenant / Multi-Marca

El CDN soporta aislamiento por tenant / marca para evitar sobreescritura accidental:

```text
apps/rc_outlook-addin-spfx/
├── public/
│   ├── assets/                       # Activos Rancho Cordova (Tenant activo en producción)
│   │   ├── icon-16.png
│   │   ├── icon-25.png
│   │   ├── icon-32.png
│   │   ├── icon-48.png
│   │   ├── icon-64.png
│   │   ├── icon-80.png
│   │   ├── icon-128.png
│   │   └── logo-filled.png
│   ├── assets/bajaworks/             # Suite corporativa oficial Bajaworks multi-dispositivo
│   │   ├── icon-16.png               # Desktop Ribbon pequeño (16x16)
│   │   ├── icon-25.png               # Mobile/iPad barra de lectura scale="1" (25x25)
│   │   ├── icon-32.png               # Desktop estándar (32x32)
│   │   ├── icon-48.png               # Mobile Bottom Sheet scale="1" (48x48)
│   │   ├── icon-50.png               # iPad Retina / iPhone scale="2" (50x50)
│   │   ├── icon-64.png               # App Catalog / Mobile scale="2" (64x64)
│   │   ├── icon-75.png               # iPhone Pro / Android OLED scale="3" (75x75)
│   │   ├── icon-80.png               # Desktop Ribbon grande (80x80)
│   │   ├── icon-96.png               # Retina scale="3" / scale="2" (96x96)
│   │   ├── icon-128.png              # App Catalog High Resolution (128x128)
│   │   ├── icon-144.png              # Mobile Ultra HD scale="3" (144x144)
│   │   ├── icon-192.png              # PWA / Android / Teams Color (192x192)
│   │   ├── icon-256.png              # High-DPI Desktop Store (256x256)
│   │   ├── icon-512.png              # Master Icon (512x512)
│   │   ├── outline-32.png            # Teams Outline Monocromático (32x32)
│   │   ├── teams-outline-32.png      # Teams Outline (32x32)
│   │   ├── teams-color-192.png       # Teams Color App Store (192x192)
│   │   └── helpdesk-logo.svg         # Vector maestro SVG
│   ├── commands.html                 # Endpoint de comandos de Outlook
│   ├── taskpane.html                 # Taskpane HTML de respaldo
│   └── polyfill.js                   # Polyfills para motores WebView antiguos
├── vercel.json                       # Configuración de ruteo, CORS y MIME types para Vercel
├── package.json
└── README.md
```

---

## 3. Consumo en los Manifiestos de Outlook

Los manifiestos XML (`dist/deploy/bajaworks/outlookAddin.bajaworks.manifest.xml`) referencian estos activos mediante la URL aislada de Bajaworks en Vercel con soporte completo de alta definición (`scale="1"`, `scale="2"`, `scale="3"`):

```xml
<IconUrl DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-64.png"/>
<HighResolutionIconUrl DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-128.png"/>
```

### Mobile Icon List (MailAppVersionOverrides 1.1)

```xml
<Icon xsi:type="bt:MobileIconList">
  <bt:Image size="25" scale="1" resid="Mobile.Icon25.1x"/>
  <bt:Image size="25" scale="2" resid="Mobile.Icon25.2x"/>
  <bt:Image size="25" scale="3" resid="Mobile.Icon25.3x"/>
  <bt:Image size="32" scale="1" resid="Icon.32x32.1x"/>
  <bt:Image size="32" scale="2" resid="Icon.32x32.2x"/>
  <bt:Image size="32" scale="3" resid="Icon.32x32.3x"/>
  <bt:Image size="48" scale="1" resid="Mobile.Icon48.1x"/>
  <bt:Image size="48" scale="2" resid="Mobile.Icon48.2x"/>
  <bt:Image size="48" scale="3" resid="Mobile.Icon48.3x"/>
</Icon>
```

### Recursos de Imagen Asociados (`<bt:Images>`)

```xml
<bt:Image id="Icon.16x16"      DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-16.png"/>
<bt:Image id="Icon.32x32"      DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-32.png"/>
<bt:Image id="Icon.80x80"      DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-80.png"/>
<bt:Image id="Mobile.Icon25.1x" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-25.png"/>
<bt:Image id="Mobile.Icon25.2x" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-50.png"/>
<bt:Image id="Mobile.Icon25.3x" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-75.png"/>
<bt:Image id="Icon.32x32.1x"   DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-32.png"/>
<bt:Image id="Icon.32x32.2x"   DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-64.png"/>
<bt:Image id="Icon.32x32.3x"   DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-96.png"/>
<bt:Image id="Mobile.Icon48.1x" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-48.png"/>
<bt:Image id="Mobile.Icon48.2x" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-96.png"/>
<bt:Image id="Mobile.Icon48.3x" DefaultValue="https://rc-outlook-addin-spfx.vercel.app/assets/bajaworks/icon-144.png"/>
```
