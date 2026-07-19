# Guía de uso

Cómo modificar el portafolio sin romperlo. No hace falta saber programar:
todo se edita en un archivo de texto y se sube con tres comandos.

← Volver al [README](../README.md) · Ver también:
[Documentación técnica](TECNICA.md) · [Accesos](ACCESOS.md)

---

## El ciclo completo

Cualquier cambio sigue siempre los mismos cuatro pasos:

```bash
cd ~/portfolio                                   # 1. entrar al proyecto
# 2. editar index.html con el editor que prefieras
git add -A
git commit -m "descripción breve de lo que cambiaste"
git push                                         # 3. subir
# 4. esperar ~30 segundos y recargar el sitio
```

Si algo sale mal, **nada se pierde**: cada cambio queda registrado y se puede
deshacer (ver [Deshacer un cambio](#deshacer-un-cambio)).

---

## Cambiar textos

Todos los textos visibles están en `index.html`. Búscalos por su contenido:

```bash
grep -n "el texto que quieres cambiar" index.html
```

Eso te da el número de línea. Ábrelo en tu editor, cambia el texto entre las
etiquetas y guarda. **No toques lo que está entre `<` y `>`**, solo el texto
que hay entre medias.

Ejemplo — cambiar el titular:

```html
<!-- antes -->
<h3>Animaciones Matemáticas</h3>
<!-- después -->
<h3>Visualización Matemática con Manim</h3>
```

## Cambiar las cifras

Están en el bloque `about-stats`, alrededor de la línea 1300:

```html
<div class="stat-number">31</div>
<div class="stat-label">Repositorios públicos</div>
```

> ⚠️ **Regla importante:** solo pon cifras que puedas demostrar. Un cliente
> que revise tu GitHub y cuente algo distinto de lo que promete el sitio se
> lleva mala impresión. Las tres cifras actuales están verificadas:
> 31 repositorios (API de GitHub), 20 años de docencia (tu perfil de
> LinkedIn), 302 seguidores (LinkedIn).

## Cambiar la foto

1. Deja el archivo nuevo en cualquier carpeta
2. Procésalo y colócalo:

```bash
cd ~/portfolio
magick /ruta/a/tu/foto.jpg -resize 600x600^ -gravity center \
  -extent 600x600 -quality 88 -strip img/foto.jpg
```

3. Sube con el ciclo normal

**El origen importa mucho.** La foto se muestra grande: si partes de una
imagen pequeña se verá borrosa por mucho que se procese. Busca al menos
800×800 px. La foto actual viene de una de 100×100 y se nota.

Para tomar una nueva: junto a una ventana de día, luz de lado, fondo liso,
encuadre de pecho hacia arriba, cámara a la altura de los ojos, y que la
tome otra persona.

## Cambiar las imágenes de los proyectos

Mismo procedimiento, pero con proporción apaisada (16:10):

```bash
magick /ruta/a/captura.png -resize 1000x -strip img/rexams.png
optipng -quiet -o3 img/rexams.png
```

Los nombres de archivo deben mantenerse (`rexams.png`, `manim.png`) o hay que
actualizar también la referencia en `index.html`.

## Añadir un proyecto nuevo

1. Localiza un bloque existente:

```bash
grep -n 'class="project-item reveal"' index.html
```

2. Copia un bloque completo, desde `<div class="project-item reveal"…>` hasta
   su `</div>` de cierre
3. Pégalo justo después del último
4. Cambia: título (`<h3>`), descripción (`<p>`), etiquetas de tecnología
   (`<span>`), el enlace de GitHub (`href`), la imagen (`src`) y el número
   (`project-number`)

> El diseño alterna automáticamente la posición izquierda/derecha de la imagen
> según si el proyecto es par o impar. No hay que configurarlo.

## Eliminar un proyecto

Borra su bloque `project-item` completo y **renumera los siguientes**
(`project-number`) para que la secuencia no salte.

## Cambiar el correo de contacto

Una sola línea, en el botón "Enviar correo":

```bash
grep -n 'mailto:' index.html
```

---

## Deshacer un cambio

Ver el historial:

```bash
git log --oneline
```

Deshacer un commit concreto (crea uno nuevo que lo revierte; no borra nada):

```bash
git revert <hash>
git push
```

Volver el archivo al estado del último commit, descartando ediciones locales:

```bash
git checkout index.html
```

---

## Verificar que un cambio llegó

```bash
curl -sI https://alvaro-portfolio.pages.dev | head -1     # debe decir HTTP/2 200
curl -s https://alvaro-portfolio.pages.dev | grep "texto que cambiaste"
```

Si el cambio no aparece, espera un minuto: Cloudflare tarda en propagar. Si
tras varios minutos sigue igual, revisa que el `git push` haya funcionado
(`git status` debe decir que estás al día con `origin/master`).

---

## Problemas frecuentes

| Síntoma | Causa probable | Solución |
|---|---|---|
| El cambio no aparece tras varios minutos | El push no se completó | `git status` y repetir `git push` |
| Error 522 al abrir el sitio | Propagación tras un despliegue reciente | Esperar 1 minuto y recargar |
| Una imagen no carga | Nombre de archivo distinto al del HTML | Comparar `ls img/` con el `src` del HTML |
| El sitio se ve roto tras editar | Etiqueta HTML mal cerrada | `git checkout index.html` y reintentar |

Si algo se rompe y no sabes por qué, el estado siempre es recuperable:
`git log --oneline` te da el último punto bueno.
