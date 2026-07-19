# Portafolio — Álvaro Ángel Molina

Sitio personal de un docente y desarrollador especializado en tecnología
educativa: evaluación automatizada con R-exams, animaciones matemáticas con
Manim y visualización de datos para la enseñanza de las matemáticas.

**En producción:** <https://alvaro-portfolio.pages.dev>

[![Estado](https://img.shields.io/badge/estado-en%20producci%C3%B3n-brightgreen)](https://alvaro-portfolio.pages.dev)
[![Hosting](https://img.shields.io/badge/hosting-Cloudflare%20Pages-orange)](https://pages.cloudflare.com/)
[![Costo](https://img.shields.io/badge/costo-%240%2Fmes-blue)](docs/TECNICA.md#costos)

---

## Qué es

Una página estática de un solo archivo. Sin framework, sin backend, sin
dependencias de compilación. Todo el HTML, el CSS y el JavaScript viven en
`index.html`; las únicas peticiones externas son las tipografías de Google
Fonts.

El sitio se despliega solo: cada `git push` a `master` dispara una
publicación en Cloudflare Pages sin intervención manual.

## Cómo actualizarlo

```bash
cd ~/portfolio
# editar index.html
git add -A && git commit -m "descripción del cambio" && git push
```

En unos 30 segundos el cambio está en vivo. No hay más pasos.

Para el detalle completo —cómo cambiar textos, fotos, proyectos y cifras—
ver la **[Guía de uso](docs/GUIA-DE-USO.md)**.

## Estructura

```
portfolio/
├── index.html              # Todo el sitio: HTML + CSS + JS en un archivo
├── img/
│   ├── foto.jpg            # Retrato (600×600)
│   ├── rexams.png          # Captura del proyecto R-Exams
│   └── manim.png           # Fotograma de una animación de Manim
├── docs/
│   ├── GUIA-DE-USO.md      # Cómo modificar el sitio, paso a paso
│   ├── TECNICA.md          # Arquitectura, decisiones y costos
│   └── ACCESOS.md          # Qué cuentas intervienen y quién las controla
├── CLAUDE.md               # Contexto del proyecto para Claude Code
├── .gitignore
└── README.md               # Este archivo
```

## Documentación

| Documento | Para qué sirve | Léelo si… |
|---|---|---|
| **[Guía de uso](docs/GUIA-DE-USO.md)** | Instrucciones operativas: cambiar textos, imágenes, proyectos y cifras | Quieres modificar el contenido |
| **[Documentación técnica](docs/TECNICA.md)** | Arquitectura, decisiones de diseño, costos, límites del plan gratuito | Quieres entender por qué está construido así |
| **[Accesos](docs/ACCESOS.md)** | Inventario de cuentas y credenciales (sin secretos) | Necesitas recuperar o transferir el control |
| **[CLAUDE.md](CLAUDE.md)** | Contexto persistente para Claude Code | Vas a trabajar en el repo con asistencia de IA |

## Estado actual

- ✅ Desplegado en Cloudflare Pages con redespliegue automático desde GitHub
- ✅ Metadatos Open Graph configurados (previsualización correcta al compartir)
- ✅ Todas las cifras del sitio verificadas contra fuentes reales
- ✅ Cero enlaces rotos
- ⚠️ El retrato procede de una imagen de 100×100 px escalada — ver
  [pendientes](docs/TECNICA.md#pendientes)
- ⚠️ Sin dominio propio: la URL `*.pages.dev` sirve para mostrar trabajo,
  no para facturarlo a un cliente

## Licencia

Sin licencia explícita. Todos los derechos reservados por el autor.

---

Contacto: [alvaroangelm@gmail.com](mailto:alvaroangelm@gmail.com) ·
[GitHub](https://github.com/alvaretto) ·
[LinkedIn](https://www.linkedin.com/in/alvaro-angel-molina/)
