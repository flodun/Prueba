# Implementation Plan: Landing Page PP-009
## Vercel Deployable Project

**Versión:** 1.0  
**Fecha:** 2025  
**Objetivo:** Implementar landing page minimalista y optimizada para Vercel basada en Design Brief

---

## 1. Stack Tecnológico Recomendado

### Opción A: Next.js 14+ (Recomendada)
**Razones:**
- ✅ Optimización automática para Vercel
- ✅ Server-side rendering (SSR) y Static Site Generation (SSG)
- ✅ Image optimization built-in
- ✅ API routes si se necesitan formularios
- ✅ Excelente performance out-of-the-box

**Stack:**
- **Framework:** Next.js 14+ (App Router)
- **Styling:** Tailwind CSS (minimalista, mobile-first)
- **TypeScript:** Para type safety
- **Animations:** Framer Motion (microinteracciones sutiles)
- **Forms:** React Hook Form + Zod validation
- **Analytics:** Vercel Analytics (built-in)

### Opción B: Astro (Alternativa ligera)
**Razones:**
- ✅ Extremadamente rápido (menos JavaScript)
- ✅ Perfecto para landing pages estáticas
- ✅ Mejor performance inicial
- ⚠️ Menos interactividad nativa

**Stack:**
- **Framework:** Astro
- **Styling:** Tailwind CSS
- **TypeScript:** Opcional pero recomendado
- **Components:** React o Vue para interactividad

### Recomendación Final: **Next.js 14+ con App Router**

---

## 2. Estructura del Proyecto

```
landing-page-pp009/
├── .next/                    # Build output (gitignored)
├── .vercel/                  # Vercel config (gitignored)
├── public/
│   ├── images/
│   │   ├── hero/
│   │   │   ├── app-mockup.webp
│   │   │   └── consultant-using-app.webp
│   │   ├── testimonials/
│   │   │   ├── maria-gonzalez.webp
│   │   │   ├── ana-martinez.webp
│   │   │   └── laura-sanchez.webp
│   │   ├── features/
│   │   │   ├── crm-icon.svg
│   │   │   ├── catalog-icon.svg
│   │   │   ├── orders-icon.svg
│   │   │   └── dashboard-icon.svg
│   │   └── offline-mode.webp
│   ├── favicon.ico
│   ├── apple-touch-icon.png
│   └── robots.txt
├── src/
│   ├── app/
│   │   ├── layout.tsx        # Root layout
│   │   ├── page.tsx          # Home page
│   │   ├── globals.css       # Global styles + Tailwind
│   │   └── metadata.ts       # SEO metadata
│   ├── components/
│   │   ├── sections/
│   │   │   ├── Hero.tsx
│   │   │   ├── PainPoints.tsx
│   │   │   ├── Solution.tsx
│   │   │   ├── OfflineMode.tsx
│   │   │   ├── Testimonials.tsx
│   │   │   ├── HowItWorks.tsx
│   │   │   └── CTASection.tsx
│   │   ├── ui/
│   │   │   ├── Button.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── TestimonialCard.tsx
│   │   │   └── FeatureCard.tsx
│   │   └── layout/
│   │       ├── Header.tsx
│   │       └── Footer.tsx
│   ├── lib/
│   │   ├── utils.ts          # Utility functions
│   │   └── constants.ts      # Content constants
│   └── types/
│       └── index.ts          # TypeScript types
├── .env.local                # Environment variables (gitignored)
├── .env.example              # Example env file
├── .gitignore
├── next.config.js            # Next.js configuration
├── tailwind.config.ts        # Tailwind configuration
├── tsconfig.json             # TypeScript config
├── package.json
├── vercel.json               # Vercel-specific config
└── README.md
```

---

## 3. Configuración de Archivos Clave

### 3.1 package.json

```json
{
  "name": "landing-page-pp009",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit"
  },
  "dependencies": {
    "next": "^14.1.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "framer-motion": "^10.18.0",
    "react-hook-form": "^7.49.0",
    "zod": "^3.22.0",
    "@hookform/resolvers": "^3.3.0",
    "@vercel/analytics": "^1.1.0"
  },
  "devDependencies": {
    "@types/node": "^20.11.0",
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "typescript": "^5.3.0",
    "tailwindcss": "^3.4.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "eslint": "^8.56.0",
    "eslint-config-next": "^14.1.0"
  }
}
```

### 3.2 next.config.js

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Optimizaciones de imagen
  images: {
    formats: ['image/avif', 'image/webp'],
    deviceSizes: [640, 750, 828, 1080, 1200, 1920],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
  },
  // Compresión
  compress: true,
  // Headers de seguridad y performance
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'X-DNS-Prefetch-Control',
            value: 'on'
          },
          {
            key: 'X-Frame-Options',
            value: 'SAMEORIGIN'
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff'
          },
          {
            key: 'Referrer-Policy',
            value: 'origin-when-cross-origin'
          }
        ]
      }
    ]
  }
}

module.exports = nextConfig
```

### 3.3 vercel.json

**Nota:** Este archivo es opcional. Vercel detecta automáticamente Next.js y aplica configuraciones por defecto. Solo agregar si necesitas configuraciones específicas.

```json
{
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "nextjs",
  "regions": ["iad1"],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=3600, must-revalidate"
        }
      ]
    },
    {
      "source": "/images/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    }
  ]
}
```

**Explicación:**
- `buildCommand`: Comando para build (Vercel lo detecta automáticamente para Next.js)
- `devCommand`: Comando para desarrollo local
- `installCommand`: Comando para instalar dependencias
- `framework`: Framework detectado (Next.js)
- `regions`: Región de deployment (iad1 = US East)
- `headers`: Headers HTTP personalizados para caching

**Alternativa:** Si no necesitas configuraciones especiales, puedes omitir este archivo completamente. Vercel funcionará perfectamente sin él.

### 3.4 tailwind.config.ts

```typescript
import type { Config } from 'tailwindcss'

const config: Config = {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: '#667eea',
          dark: '#5568d3',
          light: '#7c8ef5',
        },
        secondary: {
          DEFAULT: '#2c3e50',
          light: '#34495e',
        },
        accent: {
          DEFAULT: '#7f8c8d',
          light: '#95a5a6',
        },
        success: '#28a745',
        warning: '#ffc107',
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem',
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in',
        'slide-up': 'slideUp 0.5s ease-out',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        slideUp: {
          '0%': { transform: 'translateY(20px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
      },
    },
  },
  plugins: [],
}
export default config
```

### 3.5 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### 3.6 postcss.config.js

```javascript
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### 3.7 .gitignore

```
# Dependencies
/node_modules
/.pnp
.pnp.js

# Testing
/coverage

# Next.js
/.next/
/out/

# Production
/build

# Misc
.DS_Store
*.pem

# Debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Local env files
.env*.local
.env

# Vercel
.vercel

# TypeScript
*.tsbuildinfo
next-env.d.ts

# IDE
.vscode/
.idea/
*.swp
*.swo
*~
```

### 3.8 src/app/globals.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    scroll-behavior: smooth;
  }
  
  body {
    @apply antialiased;
  }
}

@layer utilities {
  .text-balance {
    text-wrap: balance;
  }
}
```

### 3.9 src/app/layout.tsx (Ejemplo Base)

```typescript
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import { Analytics } from '@vercel/analytics/react'
import './globals.css'

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
})

export const metadata: Metadata = {
  title: 'Gestiona tu Negocio de Belleza desde tu Móvil | App para Consultoras',
  description: 'Reduce el tiempo administrativo a la mitad y aumenta tus ventas hasta 30%. La app todo-en-uno diseñada para consultoras de venta directa en Latinoamérica.',
  metadataBase: new URL(process.env.NEXT_PUBLIC_SITE_URL || 'https://app-consultoras.com'),
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="es" className={inter.variable}>
      <body className="font-sans antialiased">
        {children}
        <Analytics />
      </body>
    </html>
  )
}
```

### 3.10 src/app/page.tsx (Ejemplo Base)

```typescript
import { Hero } from '@/components/sections/Hero'
import { PainPoints } from '@/components/sections/PainPoints'
import { Solution } from '@/components/sections/Solution'
import { OfflineMode } from '@/components/sections/OfflineMode'
import { Testimonials } from '@/components/sections/Testimonials'
import { HowItWorks } from '@/components/sections/HowItWorks'
import { CTASection } from '@/components/sections/CTASection'
import { Header } from '@/components/layout/Header'
import { Footer } from '@/components/layout/Footer'

export const revalidate = 3600 // ISR: Revalidar cada hora

export default function Home() {
  return (
    <main className="min-h-screen">
      <Header />
      <Hero />
      <PainPoints />
      <Solution />
      <OfflineMode />
      <Testimonials />
      <HowItWorks />
      <CTASection />
      <Footer />
    </main>
  )
}
```

### 3.11 public/robots.txt

```
User-agent: *
Allow: /

Sitemap: https://app-consultoras.com/sitemap.xml
```

### 3.12 src/app/sitemap.ts (Sitemap Dinámico)

```typescript
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  const baseUrl = process.env.NEXT_PUBLIC_SITE_URL || 'https://app-consultoras.com'
  
  return [
    {
      url: baseUrl,
      lastModified: new Date(),
      changeFrequency: 'weekly',
      priority: 1,
    },
  ]
}
```

### 3.13 README.md (Template)

```markdown
# Landing Page PP-009

Landing page minimalista para consultoras de venta directa en Latinoamérica.

## 🚀 Inicio Rápido

### Prerrequisitos

- Node.js 18+ 
- npm o yarn

### Instalación

\`\`\`bash
# Instalar dependencias
npm install

# Desarrollo local
npm run dev

# Build para producción
npm run build

# Iniciar servidor de producción
npm start
\`\`\`

Abre [http://localhost:3000](http://localhost:3000) en tu navegador.

## 📁 Estructura del Proyecto

\`\`\`
src/
├── app/              # App Router de Next.js
├── components/       # Componentes React
├── lib/              # Utilidades y constantes
└── types/            # TypeScript types
\`\`\`

## 🛠️ Stack Tecnológico

- **Framework:** Next.js 14+ (App Router)
- **Styling:** Tailwind CSS
- **Language:** TypeScript
- **Animations:** Framer Motion
- **Deployment:** Vercel

## 📝 Variables de Entorno

Copia \`.env.example\` a \`.env.local\` y completa las variables:

\`\`\`bash
cp .env.example .env.local
\`\`\`

## 🚢 Deployment

Este proyecto está configurado para deployment automático en Vercel.

1. Conecta tu repositorio Git a Vercel
2. Vercel detectará automáticamente Next.js
3. Las variables de entorno se configuran en Vercel Dashboard
4. Cada push a \`main\` desplegará automáticamente

## 📊 Performance

- Lighthouse Score: > 90
- Core Web Vitals: Optimizados
- Mobile-First: Diseño responsive

## 📄 Licencia

[Tu licencia aquí]
```

---

## 4. Componentes Principales

### 4.1 Hero Section (src/components/sections/Hero.tsx)

**Funcionalidades:**
- Headline y subheadline del design brief
- CTA principal destacado
- Mockup de smartphone con la app
- Badges de confianza ("100% Gratis", "Sin tarjeta")
- Animación sutil de entrada

**Implementación:**
- Next.js Image component para optimización
- Framer Motion para animación de entrada
- Responsive: stack vertical en mobile, horizontal en desktop

### 4.2 Pain Points Section (src/components/sections/PainPoints.tsx)

**Funcionalidades:**
- 3 pain points con iconos
- Diseño minimalista con cards
- Scroll reveal animation

**Implementación:**
- Iconos SVG inline o desde librería (Heroicons)
- Intersection Observer para scroll animations

### 4.3 Solution Section (src/components/sections/Solution.tsx)

**Funcionalidades:**
- 4 feature cards (CRM, Catálogo, Pedidos, Dashboard)
- Grid responsive (2x2 desktop, stack mobile)
- Hover effects sutiles

**Implementación:**
- FeatureCard component reutilizable
- Tailwind grid system

### 4.4 Testimonials Section (src/components/sections/Testimonials.tsx)

**Funcionalidades:**
- 3 testimonios con fotos
- Métricas destacadas
- Carousel opcional (si hay más de 3)

**Implementación:**
- TestimonialCard component
- Next.js Image para fotos optimizadas
- Opcional: Swiper.js para carousel

### 4.5 CTA Section (src/components/sections/CTASection.tsx)

**Funcionalidades:**
- CTA final destacado
- Badges de confianza
- Links a términos y privacidad

**Implementación:**
- Button component con variantes
- Fondo con color primario

---

## 5. Optimizaciones de Performance

### 5.1 Imágenes

**Estrategia:**
- Formato WebP/AVIF para todas las imágenes
- Lazy loading para imágenes below the fold
- Responsive images con srcset
- Optimización automática con Next.js Image

**Implementación:**
```typescript
import Image from 'next/image'

<Image
  src="/images/hero/app-mockup.webp"
  alt="App mockup"
  width={600}
  height={1200}
  priority // Solo para hero
  placeholder="blur" // Si hay blur data URL
/>
```

### 5.2 Fonts

**Estrategia:**
- Google Fonts con next/font para optimización
- Preload de font principal
- Font display: swap

**Implementación:**
```typescript
import { Inter } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
})
```

### 5.3 Code Splitting

**Estrategia:**
- Dynamic imports para componentes pesados
- Lazy load de animaciones (Framer Motion)
- Separar vendor chunks

**Implementación:**
```typescript
import dynamic from 'next/dynamic'

const Testimonials = dynamic(() => import('./Testimonials'), {
  loading: () => <TestimonialsSkeleton />,
})
```

### 5.4 Caching

**Estrategia:**
- Static generation para toda la página
- Cache headers en Vercel
- CDN caching para assets estáticos

**Implementación:**
- `export const revalidate = 3600` en page.tsx (ISR cada hora)
- Headers en vercel.json

---

## 6. SEO y Metadata

### 6.1 Metadata (src/app/metadata.ts)

```typescript
export const metadata = {
  title: 'Gestiona tu Negocio de Belleza desde tu Móvil | App para Consultoras',
  description: 'Reduce el tiempo administrativo a la mitad y aumenta tus ventas hasta 30%. La app todo-en-uno diseñada para consultoras de venta directa en Latinoamérica.',
  keywords: ['consultoras', 'venta directa', 'belleza', 'app móvil', 'gestión de clientes', 'pedidos'],
  openGraph: {
    title: 'App para Consultoras de Venta Directa',
    description: 'Gestiona tu negocio completo desde tu móvil',
    images: ['/images/og-image.webp'],
    locale: 'es_LA',
    type: 'website',
  },
  twitter: {
    card: 'summary_large_image',
    title: 'App para Consultoras de Venta Directa',
    description: 'Gestiona tu negocio completo desde tu móvil',
    images: ['/images/twitter-image.webp'],
  },
  alternates: {
    canonical: 'https://app-consultoras.com',
    languages: {
      'es-MX': 'https://app-consultoras.com/mx',
      'es-CO': 'https://app-consultoras.com/co',
      'es-AR': 'https://app-consultoras.com/ar',
    },
  },
}
```

### 6.2 Structured Data (JSON-LD)

```typescript
// src/lib/structured-data.ts
export const organizationSchema = {
  '@context': 'https://schema.org',
  '@type': 'SoftwareApplication',
  name: 'App para Consultoras',
  applicationCategory: 'BusinessApplication',
  operatingSystem: 'Android, iOS',
  offers: {
    '@type': 'Offer',
    price: '0',
    priceCurrency: 'USD',
  },
}
```

---

## 7. Analytics y Tracking

### 7.1 Vercel Analytics

**Instalación:**
```bash
npm install @vercel/analytics
```

**Implementación en layout.tsx:**
```typescript
import { Analytics } from '@vercel/analytics/react'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="es">
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  )
}
```

**Nota:** Vercel Analytics se activa automáticamente cuando el proyecto está desplegado en Vercel. No requiere configuración adicional.

### 7.2 Event Tracking

**Eventos a trackear:**
- CTA clicks (hero y final)
- Scroll depth (25%, 50%, 75%, 100%)
- Form interactions
- Time on page

**Implementación:**
```typescript
// src/lib/analytics.ts
export const trackEvent = (eventName: string, properties?: object) => {
  if (typeof window !== 'undefined' && window.analytics) {
    window.analytics.track(eventName, properties)
  }
}

// Uso en componentes
<Button onClick={() => trackEvent('cta_clicked', { location: 'hero' })}>
  Descargar Gratis
</Button>
```

---

## 8. Formularios (Si aplica)

### 8.1 Registro/Lead Capture

**Stack:**
- React Hook Form para gestión de formularios
- Zod para validación
- API route para envío

**Implementación:**
```typescript
// src/app/api/register/route.ts
import { NextResponse } from 'next/server'
import { z } from 'zod'

const registerSchema = z.object({
  email: z.string().email(),
  phone: z.string().optional(),
})

export async function POST(request: Request) {
  const body = await request.json()
  const validated = registerSchema.parse(body)
  
  // Lógica de guardado (base de datos, email service, etc.)
  
  return NextResponse.json({ success: true })
}
```

---

## 9. Pasos de Implementación

### Fase 1: Setup Inicial (Día 1)

1. **Crear proyecto Next.js**
   ```bash
   npx create-next-app@latest landing-page-pp009 --typescript --tailwind --app
   cd landing-page-pp009
   ```

2. **Instalar dependencias**
   ```bash
   # Dependencias de producción
   npm install framer-motion react-hook-form zod @hookform/resolvers @vercel/analytics
   
   # Dependencias de desarrollo
   npm install -D @types/node @types/react @types/react-dom
   ```

3. **Configurar archivos base**
   - next.config.js
   - tailwind.config.ts
   - tsconfig.json
   - vercel.json

4. **Setup de estructura de carpetas**
   - Crear estructura de componentes
   - Crear archivos de constantes

### Fase 2: Componentes Base (Días 2-3)

1. **UI Components**
   - Button.tsx (variantes: primary, secondary, outline)
   - Card.tsx (reutilizable)
   - FeatureCard.tsx
   - TestimonialCard.tsx

2. **Layout Components**
   - Header.tsx (navegación simple)
   - Footer.tsx (links, copyright)

3. **Utilities**
   - utils.ts (cn function para Tailwind)
   - constants.ts (contenido del design brief)

### Fase 3: Secciones Principales (Días 4-6)

1. **Hero Section**
   - Implementar con mockup
   - CTA destacado
   - Animaciones de entrada

2. **Pain Points Section**
   - 3 cards con iconos
   - Scroll animations

3. **Solution Section**
   - 4 feature cards
   - Grid responsive

4. **Offline Mode Section**
   - Sección destacada
   - Mockup o ilustración

5. **Testimonials Section**
   - 3 testimonios
   - Fotos optimizadas

6. **How It Works Section**
   - 3 pasos simples
   - Timeline visual

7. **CTA Final Section**
   - CTA destacado
   - Badges de confianza

### Fase 4: Optimizaciones (Día 7)

1. **Performance**
   - Optimizar imágenes
   - Lazy loading
   - Code splitting

2. **SEO**
   - Metadata completo
   - Structured data
   - Sitemap

3. **Analytics**
   - Setup Vercel Analytics
   - Event tracking

4. **Testing**
   - Lighthouse audit
   - Mobile testing
   - Cross-browser testing

### Fase 5: Deployment (Día 8)

1. **Pre-deployment**
   ```bash
   # Verificar que todo funciona localmente
   npm run build
   npm run start
   
   # Verificar TypeScript
   npm run type-check
   
   # Verificar linting
   npm run lint
   ```

2. **Preparar Repositorio Git**
   ```bash
   # Inicializar git si no existe
   git init
   
   # Agregar todos los archivos
   git add .
   
   # Commit inicial
   git commit -m "Initial commit: Landing page PP-009"
   
   # Crear repositorio en GitHub/GitLab/Bitbucket
   # Luego conectar:
   git remote add origin <tu-repo-url>
   git branch -M main
   git push -u origin main
   ```

3. **Vercel Setup - Opción A: Vía Dashboard**
   - Ir a [vercel.com](https://vercel.com) y hacer login
   - Click en "Add New Project"
   - Importar repositorio Git (GitHub/GitLab/Bitbucket)
   - Vercel detectará automáticamente Next.js
   - Configurar:
     - **Framework Preset:** Next.js (auto-detectado)
     - **Root Directory:** `./` (default)
     - **Build Command:** `npm run build` (default)
     - **Output Directory:** `.next` (default, no cambiar)
     - **Install Command:** `npm install` (default)
   - Agregar Environment Variables (ver sección 11)
   - Click "Deploy"

4. **Vercel Setup - Opción B: Vía CLI**
   ```bash
   # Instalar Vercel CLI globalmente
   npm i -g vercel
   
   # Login a Vercel
   vercel login
   
   # Deploy (primera vez pregunta configuración)
   vercel
   
   # Deploy a producción
   vercel --prod
   ```

5. **Configurar Variables de Entorno en Vercel**
   - Ir a Project Settings > Environment Variables
   - Agregar cada variable:
     - `NEXT_PUBLIC_SITE_URL` = `https://tu-dominio.vercel.app`
     - `NEXT_PUBLIC_APP_STORE_URL` = (si aplica)
     - `NEXT_PUBLIC_GOOGLE_PLAY_URL` = (si aplica)
   - Seleccionar ambientes: Production, Preview, Development
   - Guardar y redeploy

6. **Configurar Dominio Custom (Opcional)**
   - Ir a Project Settings > Domains
   - Agregar dominio custom
   - Seguir instrucciones de DNS
   - Esperar propagación (puede tardar hasta 48 horas)

7. **Post-deployment**
   - Verificar que la página carga: `https://tu-proyecto.vercel.app`
   - Verificar imágenes se cargan correctamente
   - Probar CTAs y formularios
   - Verificar analytics en Vercel Dashboard
   - Test de performance con Lighthouse
   - Verificar mobile responsive
   - Configurar Google Search Console (si aplica)

---

## 10. Checklist de Deployment

### Pre-Deployment

- [ ] Build exitoso sin errores
- [ ] TypeScript sin errores (`npm run type-check`)
- [ ] ESLint sin errores (`npm run lint`)
- [ ] Todas las imágenes optimizadas (WebP/AVIF)
- [ ] Metadata SEO completo
- [ ] Structured data implementado
- [ ] Analytics configurado
- [ ] Formularios funcionando (si aplica)
- [ ] Links externos verificados
- [ ] Mobile responsive verificado
- [ ] Performance score > 90 (Lighthouse)

### Vercel Configuration

- [ ] Repositorio conectado
- [ ] Framework detectado correctamente (Next.js)
- [ ] Build command: `npm run build`
- [ ] Output directory: `.next` (automático)
- [ ] Environment variables configuradas
- [ ] Custom domain configurado (si aplica)
- [ ] SSL automático activado

### Post-Deployment

- [ ] Página carga correctamente en URL de Vercel
- [ ] Imágenes se cargan (verificar en Network tab)
- [ ] CTAs funcionan y redirigen correctamente
- [ ] Formularios envían (si aplica)
- [ ] Analytics tracking funciona (verificar en Vercel Dashboard > Analytics)
- [ ] Mobile funciona correctamente (probar en dispositivo real)
- [ ] Performance en producción verificado (Lighthouse en producción)
- [ ] SEO verificado:
  - [ ] Metadata visible en View Source
  - [ ] Open Graph funciona (probar con [opengraph.xyz](https://www.opengraph.xyz))
  - [ ] Sitemap accesible: `https://tu-dominio.vercel.app/sitemap.xml`
  - [ ] Robots.txt accesible: `https://tu-dominio.vercel.app/robots.txt`
- [ ] Google Search Console configurado (si aplica)
- [ ] Dominio custom funciona (si aplica)
- [ ] SSL/HTTPS activo (automático en Vercel)

---

## 11. Variables de Entorno y Configuración

### .env.example

```env
# Site URL (usado para metadata, sitemap, etc.)
NEXT_PUBLIC_SITE_URL=https://app-consultoras.com

# Analytics (Vercel Analytics no requiere ID, se activa automáticamente)
# Si usas Google Analytics u otro:
# NEXT_PUBLIC_GA_ID=

# API (si aplica para formularios)
# NEXT_PUBLIC_API_URL=https://api.example.com

# App Store Links
NEXT_PUBLIC_APP_STORE_URL=https://apps.apple.com/app/...
NEXT_PUBLIC_GOOGLE_PLAY_URL=https://play.google.com/store/apps/...

# Social Media (opcional)
NEXT_PUBLIC_TWITTER_HANDLE=@tu_handle
NEXT_PUBLIC_FACEBOOK_PAGE=https://facebook.com/tu-pagina
NEXT_PUBLIC_INSTAGRAM_HANDLE=@tu_handle
```

### Configuración en Vercel

**Pasos detallados:**

1. **Ir a Vercel Dashboard:**
   - Seleccionar tu proyecto
   - Click en "Settings" > "Environment Variables"

2. **Agregar Variables:**
   - Click "Add New"
   - Ingresar Key (ej: `NEXT_PUBLIC_SITE_URL`)
   - Ingresar Value (ej: `https://app-consultoras.com`)
   - Seleccionar ambientes:
     - ✅ Production
     - ✅ Preview (opcional, para PRs)
     - ✅ Development (opcional, para local)

3. **Importante:**
   - Variables que empiezan con `NEXT_PUBLIC_` son expuestas al cliente
   - Variables sin `NEXT_PUBLIC_` son solo server-side
   - Después de agregar variables, hacer redeploy

4. **Verificar Variables:**
   ```bash
   # En Vercel CLI
   vercel env ls
   ```

---

## 11.5 Manejo de Errores y Páginas Especiales

### 11.5.1 Error Pages

**src/app/not-found.tsx (404)**
```typescript
import Link from 'next/link'
import { Button } from '@/components/ui/Button'

export default function NotFound() {
  return (
    <div className="flex min-h-screen flex-col items-center justify-center px-4">
      <h1 className="text-4xl font-bold text-secondary mb-4">404</h1>
      <p className="text-xl text-accent mb-8">Página no encontrada</p>
      <Link href="/">
        <Button>Volver al inicio</Button>
      </Link>
    </div>
  )
}
```

**src/app/error.tsx (Error Boundary)**
```typescript
'use client'

import { useEffect } from 'react'
import { Button } from '@/components/ui/Button'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    // Log error to monitoring service
    console.error(error)
  }, [error])

  return (
    <div className="flex min-h-screen flex-col items-center justify-center px-4">
      <h2 className="text-2xl font-bold text-secondary mb-4">
        Algo salió mal
      </h2>
      <Button onClick={reset}>Intentar de nuevo</Button>
    </div>
  )
}
```

**src/app/global-error.tsx (Error Global)**
```typescript
'use client'

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <html>
      <body>
        <div className="flex min-h-screen flex-col items-center justify-center">
          <h2>Algo salió mal</h2>
          <button onClick={reset}>Intentar de nuevo</button>
        </div>
      </body>
    </html>
  )
}
```

### 11.5.2 Loading States

**src/app/loading.tsx**
```typescript
export default function Loading() {
  return (
    <div className="flex min-h-screen items-center justify-center">
      <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-primary"></div>
    </div>
  )
}
```

---

## 12. Monitoreo y Mantenimiento

### Performance Monitoring

- **Vercel Analytics:** Built-in, automático
- **Lighthouse CI:** Integración con CI/CD
- **Real User Monitoring:** Vercel Speed Insights

### Métricas a Monitorear

- **Core Web Vitals:**
  - LCP (Largest Contentful Paint) < 2.5s
  - FID (First Input Delay) < 100ms
  - CLS (Cumulative Layout Shift) < 0.1

- **Business Metrics:**
  - Tasa de conversión
  - Tiempo en página
  - Scroll depth
  - Bounce rate

### Actualizaciones

- **Contenido:** Actualizar constantes en `src/lib/constants.ts`
- **Imágenes:** Reemplazar en `public/images/`
- **Deploy:** Automático con push a main branch

---

## 13. Recursos y Referencias

### Documentación

- [Next.js Documentation](https://nextjs.org/docs)
- [Vercel Deployment Guide](https://vercel.com/docs)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [Framer Motion](https://www.framer.com/motion/)

### Herramientas

- **Design:** Figma (si hay diseño)
- **Images:** Squoosh (optimización), TinyPNG
- **Icons:** Heroicons, Feather Icons
- **Fonts:** Google Fonts (Inter)

### Testing Tools

- **Lighthouse:** Performance audit
- **PageSpeed Insights:** Google
- **WebPageTest:** Detailed analysis
- **BrowserStack:** Cross-browser testing

---

## 14. Consideraciones Especiales

### Mobile-First

- **Testing:** Probar en dispositivos reales
- **Touch targets:** Mínimo 44x44px
- **Viewport:** Meta tag correcto
- **Font size:** Mínimo 16px para evitar zoom

### Accesibilidad

- **Semantic HTML:** Usar elementos correctos
- **ARIA labels:** Donde sea necesario
- **Keyboard navigation:** Funcional
- **Screen readers:** Probar con NVDA/JAWS

### Internacionalización (Futuro)

- **i18n:** Next.js i18n routing
- **Content:** Separar contenido por locale
- **URLs:** `/es`, `/es-mx`, `/es-co`

---

## 15. Timeline Estimado

**Total: 8 días de desarrollo**

- **Día 1:** Setup y configuración
- **Días 2-3:** Componentes base
- **Días 4-6:** Secciones principales
- **Día 7:** Optimizaciones y testing
- **Día 8:** Deployment y verificación

**Nota:** Timeline asume 1 desarrollador full-time. Ajustar según equipo.

---

## 16. Troubleshooting Común

### Problema: Build falla en Vercel

**Solución:**
1. Verificar logs en Vercel Dashboard > Deployments
2. Probar build local: `npm run build`
3. Verificar que todas las dependencias están en `package.json`
4. Verificar que no hay imports de archivos que no existen
5. Verificar TypeScript: `npm run type-check`

### Problema: Imágenes no cargan

**Solución:**
1. Verificar que las imágenes están en `/public/images/`
2. Verificar que las rutas en código son correctas (`/images/...`)
3. Verificar formato de imagen (WebP/AVIF recomendado)
4. Verificar que Next.js Image component está configurado correctamente

### Problema: Variables de entorno no funcionan

**Solución:**
1. Verificar que variables empiezan con `NEXT_PUBLIC_` si son client-side
2. Verificar que están configuradas en Vercel Dashboard
3. Hacer redeploy después de agregar variables
4. Verificar que no hay espacios en los valores

### Problema: Estilos de Tailwind no aplican

**Solución:**
1. Verificar que `globals.css` importa Tailwind
2. Verificar `tailwind.config.ts` tiene los paths correctos
3. Verificar que `postcss.config.js` existe
4. Reiniciar dev server: `npm run dev`

### Problema: Analytics no funciona

**Solución:**
1. Verificar que `@vercel/analytics` está instalado
2. Verificar que `<Analytics />` está en `layout.tsx`
3. Verificar que el proyecto está desplegado en Vercel (no funciona local)
4. Esperar unos minutos después del deploy

---

## 17. Comandos Útiles

### Desarrollo

```bash
# Iniciar servidor de desarrollo
npm run dev

# Build para producción
npm run build

# Iniciar servidor de producción local
npm start

# Verificar TypeScript
npm run type-check

# Linting
npm run lint

# Fix linting automáticamente
npm run lint -- --fix
```

### Vercel CLI

```bash
# Login
vercel login

# Deploy a preview
vercel

# Deploy a producción
vercel --prod

# Ver logs
vercel logs

# Listar variables de entorno
vercel env ls

# Agregar variable de entorno
vercel env add VARIABLE_NAME

# Ver información del proyecto
vercel inspect
```

### Git

```bash
# Verificar estado
git status

# Agregar cambios
git add .

# Commit
git commit -m "Descripción del cambio"

# Push (trigger deploy automático en Vercel)
git push origin main
```

---

## 18. Próximos Pasos

1. **Aprobar plan:** Revisar y aprobar stack y estructura
2. **Setup inicial:** 
   ```bash
   npx create-next-app@latest landing-page-pp009 --typescript --tailwind --app
   cd landing-page-pp009
   ```
3. **Configurar archivos base:** Copiar configuraciones de sección 3
4. **Instalar dependencias:**
   ```bash
   npm install framer-motion react-hook-form zod @hookform/resolvers @vercel/analytics
   ```
5. **Crear estructura de carpetas:** Seguir estructura de sección 2
6. **Implementar componentes:** Seguir fases de sección 9
7. **Testing local:** Verificar que todo funciona antes de deploy
8. **Setup Git:** Inicializar repositorio y hacer primer commit
9. **Deploy a Vercel:** Seguir instrucciones de sección 9, Fase 5
10. **Configurar dominio:** Si aplica, seguir sección 9, Fase 5, paso 6
11. **Monitoreo:** Configurar analytics y verificar métricas

---

## 19. Recursos Adicionales

### Documentación Oficial

- [Next.js App Router](https://nextjs.org/docs/app)
- [Vercel Deployment](https://vercel.com/docs)
- [Vercel Analytics](https://vercel.com/docs/analytics)
- [Tailwind CSS](https://tailwindcss.com/docs)

### Guías Específicas

- [Next.js Image Optimization](https://nextjs.org/docs/app/building-your-application/optimizing/images)
- [Next.js Metadata API](https://nextjs.org/docs/app/building-your-application/optimizing/metadata)
- [Vercel Environment Variables](https://vercel.com/docs/projects/environment-variables)

### Comunidad

- [Next.js Discord](https://nextjs.org/discord)
- [Vercel Community](https://github.com/vercel/community)

---

## 20. Verificación Rápida Post-Deployment

### Checklist de 5 Minutos

Después del primer deployment, verificar rápidamente:

1. **URL Funciona:**
   ```bash
   curl -I https://tu-proyecto.vercel.app
   # Debe retornar 200 OK
   ```

2. **Página Carga:**
   - Abrir URL en navegador
   - Verificar que no hay errores en consola (F12)
   - Verificar que contenido se muestra

3. **Imágenes:**
   - Verificar que imágenes hero cargan
   - Verificar que no hay errores 404 en Network tab

4. **CTAs:**
   - Click en botón principal
   - Verificar que redirige o abre formulario

5. **Mobile:**
   - Abrir en móvil o DevTools mobile view
   - Verificar que layout es responsive

6. **Analytics:**
   - Ir a Vercel Dashboard > Analytics
   - Verificar que hay datos (puede tardar unos minutos)

### Comandos de Verificación

```bash
# Verificar build local antes de deploy
npm run build && npm start

# Verificar TypeScript
npm run type-check

# Verificar linting
npm run lint

# Verificar que no hay errores de dependencias
npm audit
```

---

## 21. Resumen Ejecutivo para Deployment

### Pasos Mínimos para Deploy

1. **Crear proyecto:**
   ```bash
   npx create-next-app@latest landing-page-pp009 --typescript --tailwind --app
   ```

2. **Instalar dependencias:**
   ```bash
   npm install @vercel/analytics framer-motion
   ```

3. **Configurar archivos base:**
   - Copiar configuraciones de sección 3
   - Crear estructura de carpetas

4. **Implementar componentes básicos:**
   - Layout con Analytics
   - Page con secciones
   - Componentes UI básicos

5. **Git y Deploy:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin <repo-url>
   git push -u origin main
   ```

6. **Vercel:**
   - Importar repo en Vercel Dashboard
   - Deploy automático

**Tiempo estimado:** 2-3 horas para setup básico + deployment

---

**Documento Versión:** 2.0  
**Última Actualización:** 2025  
**Para uso de:** Equipo de Desarrollo  
**Estado:** Listo para implementación - Completo y verificable

