# CLAUDE.md — Portafolio Álvaro Ángel Molina

Contexto persistente para Claude Code. Léelo antes de tocar nada.

**Documentación relacionada:** [README](README.md) ·
[Guía de uso](docs/GUIA-DE-USO.md) · [Técnica](docs/TECNICA.md) ·
[Accesos](docs/ACCESOS.md)

---

## Qué es este proyecto

Sitio personal estático de un solo archivo, desplegado en Cloudflare Pages con
redespliegue automático desde GitHub.

- **Producción:** <https://alvaro-portfolio.pages.dev>
- **Repo:** `github.com/alvaretto/portfolio` (público, rama `master`)
- **Proyecto Pages:** `alvaro-portfolio`
- **Costo:** $0/mes

## Reglas del proyecto

### 1. Español siempre
Toda interacción, commit y documentación en español. Sin excepciones.

### 2. El nombre del usuario
**Álvaro Ángel Molina.** `Ángel` es el **primer apellido**, no un segundo
nombre. Forma corta correcta: **Álvaro Ángel**. Nunca "Álvaro Molina".
El logo del sitio dice `álvaro.ángel` por esta razón.

### 3. Nada atado al runtime de Cloudflare
Restricción explícita del usuario. Prohibido introducir Workers, Pages
Functions, D1, KV, Durable Objects o Cron Triggers. Todo lo desplegado debe
poder correr igual en cualquier otro hosting. Si una solución los requiere,
proponer alternativa o decir que no se puede.

### 4. No fabricar evidencia
Las imágenes y cifras del sitio deben provenir de material real y verificable
del usuario. Prohibido: capturas de interfaces inventadas, cifras de plantilla,
imágenes de banco. Si no hay material real para algo, decirlo y proponer
retirar esa sección.

### 5. Cifras verificadas únicamente
Las tres actuales están comprobadas: 31 repositorios (API de GitHub), 20 años
de docencia (perfil de LinkedIn), 302 seguidores (LinkedIn). Antes de tocar
una cifra, verificarla. Si no se puede verificar, no ponerla.

### 6. Sin build
La configuración de build en Cloudflare está vacía a propósito. No añadir
empaquetadores, transpiladores ni `package.json`. Cualquier comando de build
rompería el despliegue.

### 7. Nunca versionar `.wrangler/`
Contiene el `account_id` y el correo de la cuenta de Cloudflare. Ya está en
`.gitignore`. Ver [Accesos](docs/ACCESOS.md).

## Flujo de trabajo

```bash
cd ~/Proyectos-2026/Proyectos-Varios/Emprendimientos/MiTercerMillon/portfolio
# editar
git add -A && git commit -m "…" && git push
# ~30 s después está en producción
```

**Verificación obligatoria tras cada cambio** (plano de datos, no de control):

```bash
curl -sI https://alvaro-portfolio.pages.dev | head -1        # HTTP/2 200
curl -s https://alvaro-portfolio.pages.dev | grep "lo que cambiaste"
```

No basta con que el panel de Cloudflare diga que desplegó. Hay que comprobar
que el contenido cambió realmente en producción. Los primeros intentos tras un
despliegue pueden devolver **522** o servir la versión anterior: es propagación
normal, reintentar unos segundos después.

## Estructura

| Ruta | Qué es |
|---|---|
| `index.html` | Todo el sitio: HTML + CSS + JS embebidos (~1560 líneas) |
| `img/foto.jpg` | Retrato 600×600 |
| `img/rexams.png` | Captura del proyecto R-Exams |
| `img/manim.png` | Fotograma de animación Manim |
| `docs/` | Guía de uso, técnica y accesos |

### Puntos de referencia en `index.html`

Localizarlos siempre con `grep -n`, no por número de línea (cambian):

```bash
grep -n 'stat-number'            # las tres cifras
grep -n 'class="project-item'    # tarjetas de proyecto
grep -n 'mailto:'                # correo de contacto
grep -n 'nav-logo'               # logo álvaro.ángel
grep -n 'og:title'               # metadatos para compartir
```

## Historia y decisiones ya tomadas

No volver a proponer lo ya descartado:

- **Formulario de contacto:** eliminado. Estaba roto (`preventDefault` sin
  manejador, se tragaba los mensajes). Sustituido por `mailto:`. No reintroducir
  formularios sin backend.
- **WhisperLab:** eliminado del sitio por decisión del usuario (2026-07-19).
  No reañadir.
- **Direct Upload → Git:** ya migrado. La documentación oficial de Cloudflare
  dice que hay que borrar y recrear el proyecto; **es falso**, existe un botón
  *Connect* en Settings → Build → Git repository.
- **Plan de pago de Cloudflare:** autorizado por el usuario pero innecesario.
  No activar mientras el sitio sea estático.

## Contexto del usuario

- Docente de matemáticas en Armenia, Quindío, Colombia. Manjaro/KDE/zsh.
- Este portafolio forma parte del proyecto **MilDolares-2026** (meta de
  ingresos freelance). El objetivo real es captar clientes.
- **No es desarrollador web profesional.** Explicar los pasos de interfaz con
  precisión y sin jerga. Si un panel externo cambió de forma, pedir captura en
  vez de dictar rutas de memoria.
- Prefiere que se resuelva el problema, no que se le devuelva una lista de
  opciones — pero espera que se le señalen los compromisos antes de actuar.

## Pendientes

Ver la tabla completa en [Técnica → Pendientes](docs/TECNICA.md#pendientes).
Los dos que más importan:

1. **Retrato de baja resolución** — origen de 100×100 px. Hace falta una foto
   de ≥800 px. El usuario sabe cómo tomarla.
2. **Sin dominio propio** — `*.pages.dev` no se le factura a un cliente.
   Comprar dominio (~$10/año) solo cuando aparezca el primer cliente real.
