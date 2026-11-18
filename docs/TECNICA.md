# Documentación técnica

Arquitectura, decisiones y límites del proyecto. Escrito para quien tenga que
mantenerlo dentro de un año —incluido su autor.

← Volver al [README](../README.md) · Ver también:
[Guía de uso](GUIA-DE-USO.md) · [Accesos](ACCESOS.md)

---

## Arquitectura

```
   ~/Proyectos-2026/Proyectos-Varios/Emprendimientos/MiTercerMillon/portfolio (local)
        │  git push
        ▼
   github.com/alvaretto/portfolio  (master)
        │  webhook de la app de GitHub
        ▼
   Cloudflare Pages · proyecto "alvaro-portfolio"
        │  sin build: sirve los archivos tal cual
        ▼
   https://alvaro-portfolio.pages.dev
```

Un solo archivo HTML con el CSS y el JavaScript embebidos. Sin framework, sin
empaquetador, sin paso de compilación. La configuración de build en Cloudflare
está **deliberadamente vacía**: no hay nada que compilar y cualquier comando
ahí haría fallar el despliegue.

## Decisiones y su porqué

### Por qué Cloudflare Pages y no otra cosa

Se evaluaron cuatro opciones para todo el portafolio de proyectos del autor.
El veredicto fue que **Cloudflare aplica a 2 de ~20 proyectos**: el resto está
anclado a la máquina local por datos personales de estudiantes (Ley 1581/2012)
o por dependencias nativas de R, LaTeX e ImageMagick que no existen en el
runtime de Workers.

Este sitio es uno de los dos casos donde sí aplica, precisamente porque es un
HTML estático sin lógica de servidor.

### Por qué no hay Workers, D1 ni KV

Restricción explícita del autor: **nada de código atado al runtime de
Cloudflare**. Todo lo desplegado debe poder correr igual en cualquier otro
hosting o en local. Eso deja fuera Workers, Durable Objects, D1, KV y los Cron
Triggers, y reduce el uso de Cloudflare a servir archivos estáticos.

Consecuencia práctica: migrar este sitio a Netlify, GitHub Pages o un servidor
propio es copiar tres archivos. No hay dependencia real del proveedor.

### Por qué un enlace `mailto:` y no un formulario

El sitio traía un formulario de contacto con `onsubmit="event.preventDefault()"`
y ningún manejador. Se tragaba los mensajes en silencio: el visitante creía
haber escrito y nadie recibía nada.

Un formulario funcional exigía o un servicio externo (Formspree, Web3Forms) o
un Worker —lo segundo viola la restricción anterior—. El `mailto:` no puede
fallar, no depende de terceros y no introduce credenciales.

### Por qué WhisperLab se muestra con código y no con captura

*(Histórico: el proyecto fue eliminado del sitio por decisión del autor.)*

WhisperLab es un notebook de Google Colab sin interfaz gráfica. No existía
ninguna captura real. Se optó por mostrar código literal del notebook en vez
de fabricar una pantalla de aplicación inventada: el portafolio no debe
mostrar evidencia de algo que no existe con esa forma.

### Origen de las imágenes

Ninguna es genérica ni de banco de imágenes. Todas salen de material propio:

| Imagen | Origen |
|---|---|
| `foto.jpg` | Foto de perfil de LinkedIn (100×100 escalada a 600×600 con Lanczos + realce) |
| `rexams.png` | Página renderizada de un ejercicio real del repositorio R-Exams, recortada para eliminar los encabezados internos `Question`/`Answerlist` de r-exams |
| `manim.png` | Fotograma de `IntegralSustitucion.mp4`, con niveles y contraste corregidos (el original era azul oscuro sobre negro, ilegible) |

## Costos

**$0/mes.** Todo cabe holgadamente en el plan gratuito de Cloudflare Pages:

| Límite del plan gratuito | Consumo actual |
|---|---|
| 500 builds/mes | ~10 |
| 20 000 archivos por despliegue | 4 |
| 25 MiB por archivo | máx. 101 KB |
| Ancho de banda | sin límite documentado |

El plan de pago ($5/mes) fue autorizado por el autor pero **no es necesario**.
No activarlo mientras el sitio siga siendo estático.

## Rendimiento

- Peso total: ~270 KB (HTML 45 KB + imágenes 224 KB)
- Imágenes con `loading="lazy"`, `width`/`height` declarados (evita saltos de
  maquetación) y `decoding="async"`
- `preconnect` a Google Fonts para adelantar el handshake
- Sin JavaScript de terceros, sin analítica, sin cookies

## Accesibilidad

- Idioma declarado (`lang="es"`)
- Texto alternativo descriptivo en las tres imágenes
- La tarjeta de código llevaba `aria-label` (bloque ya retirado)

**Sin auditar:** contraste de la paleta dorada sobre fondo oscuro, navegación
por teclado, comportamiento con lector de pantalla.

## Asistencia de IA local

Nada de esto se despliega ni está en el repositorio: es configuración de Claude
Code en la máquina del autor, documentada aquí para que se sepa que existe.

Desde el 2026-11-17 el proyecto usa **Modern Web Guidance** (MWG), un plugin de
Google Chrome que le da a Claude Code guías actuales de la plataforma web, con
datos de compatibilidad Baseline: qué API nativa usar y qué alternativa dejar
para los navegadores que todavía no la soportan.

| Pieza | Dónde | Por qué |
|---|---|---|
| Plugin `modern-web-guidance@googlechrome` (v1.0.1) | `.claude/settings.local.json`, alcance local, ignorado por git | Solo este proyecto lo carga |
| Línea «usa la skill `modern-web-guidance`» | `CLAUDE.local.md`, excluido en `.git/info/exclude` | La skill no se activa sola (ver abajo) |
| Hook que apaga la telemetría | `~/.claude/hooks/mwg-sin-telemetria.py` | El CLI de MWG envía a Google las búsquedas por defecto |

**Por qué hace falta `CLAUDE.local.md`.** Claude Code reserva un espacio fijo
para la lista de skills que ve el modelo (unos 30 000 caracteres) y da
prioridad a las más usadas. Una skill nueva llega solo con su nombre, y el
modelo nunca lee su disparador. En la prueba, el mismo pedido sin esa línea no
consultó MWG; con ella, sí.

**Por qué la telemetría se apaga con un hook y no en `.zshrc`.** La variable que
pide Google, `DISABLE_TELEMETRY=1`, también la lee Claude Code y le apaga
funciones. El hook la pone solo en los comandos que ejecutan MWG.

**Cómo saber si se usó.** La respuesta termina con una sección
`### Modern Web Guidance` que cita las guías aplicadas. Una tarea web que
termina sin ella no la consultó.

**Convivencia con Impeccable.** Impeccable decide el diseño (layout, color,
tipografía, movimiento); MWG decide qué API lo implementa y con qué
alternativa. Si una guía de MWG fija un valor visual, gana Impeccable. La regla
está en `~/.claude/rules/impeccable-frontend.md`.

**Evidencia.** Pedido de prueba: un diálogo de contacto que se cierre al hacer
clic fuera. Sin MWG salió JavaScript escrito a mano para detectar el clic; con
MWG, `<dialog closedby="any">` más una alternativa para Safari, que aún no lo
soporta.

Detalle y vuelta atrás: `~/.claude/backups/mwg-instalacion-2026-11-17/LEEME.md`.

## Pendientes

| Pendiente | Impacto | Nota |
|---|---|---|
| Retrato de baja resolución | Medio | Origen de 100×100; se necesita una foto de ≥800 px |
| Sin dominio propio | Alto si se busca cliente de pago | `*.pages.dev` no se factura a un cliente; un dominio cuesta ~$10/año |
| Sin verificar en móvil | Medio | El diseño es responsive pero solo se probó en escritorio |
| El correo de la cuenta de Cloudflare no coincide con la identidad del sitio | Bajo | Ver [Accesos](ACCESOS.md) |
| Historial de git contiene un correo personal | Bajo | Ver [Accesos](ACCESOS.md#limpieza-pendiente-del-historial) |
| Accesibilidad sin auditar | Medio | Contraste y navegación por teclado |

## Lecciones aprendidas

**La documentación oficial de Cloudflare estaba desactualizada.** Afirma que un
proyecto creado con Direct Upload no puede convertirse a integración con Git y
que hay que borrarlo y recrearlo. Es falso: el panel actual tiene un botón
**Connect** en *Settings → Build → Git repository* que hace exactamente eso.
Seguir la documentación al pie de la letra habría destruido el proyecto sin
necesidad.

**Verificar en el plano de datos, no en el de control.** Que el panel muestre
un repositorio conectado no prueba que el despliegue automático funcione. La
comprobación válida fue hacer un `push` real y confirmar que el contenido
cambiaba en producción.

**Instalar un plugin no es usarlo.** Que Claude Code cargue una skill no
garantiza que el modelo lea su descripción: con muchas skills instaladas, las
nuevas llegan solo con el nombre. Se descubrió leyendo lo que el modelo recibe,
no lo que reporta el instalador. Ver [Asistencia de IA local](#asistencia-de-ia-local).
