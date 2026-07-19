# Accesos y cuentas

Inventario de qué cuentas intervienen y dónde se guarda cada credencial.

> 🔒 **Este archivo no contiene contraseñas, tokens ni claves, y no debe
> contenerlas nunca.** Solo dice qué existe y dónde buscarlo.

← Volver al [README](../README.md) · Ver también:
[Guía de uso](GUIA-DE-USO.md) · [Documentación técnica](TECNICA.md)

---

## Cuentas implicadas

| Servicio | Identidad | Para qué | Titular |
|---|---|---|---|
| **Cloudflare** | `luisernestomarceloberni@gmail.com` | Aloja el sitio (proyecto `alvaro-portfolio`) | Álvaro Ángel Molina |
| **GitHub** | `alvaretto` | Repositorio `alvaretto/portfolio` | Álvaro Ángel Molina |
| **Correo público del sitio** | `alvaroangelm@gmail.com` | Botón de contacto del portafolio | Álvaro Ángel Molina |

**Identificador de cuenta Cloudflare:** `64dbf6fe7bf78171224cd954b43878ac`
(no es secreto; aparece en las URL del panel)

### ⚠️ Los tres correos son distintos y es intencional

- El de **Cloudflare** es administrativo, nunca se muestra en el sitio
- El del **botón de contacto** es el que ven los visitantes
- El de los **commits de git** es `alvaroangelm@gmail.com`

No hace falta unificarlos para que nada funcione. Sí conviene, algún día,
cambiar el correo de la cuenta de Cloudflare para que coincida con la
identidad del proyecto: *panel → My Profile → Authentication → Change Email*.

## Credenciales locales

| Qué | Dónde | Notas |
|---|---|---|
| Token OAuth de Wrangler | `~/.config/.wrangler/config/default.toml` | Generado por `wrangler login`. **No versionar.** |
| Caché de cuenta de Wrangler | `~/Proyectos-2026/Proyectos-Varios/Emprendimientos/MiTercerMillon/portfolio/.wrangler/` | Ignorado por `.gitignore` |
| Credencial de GitHub | Gestionada por `gh` (keyring del sistema) | `gh auth status` para verificar |

**No existe ningún token de API de Cloudflare de larga vida.** La integración
con GitHub usa una app de GitHub autorizada, no un token guardado. Esto es
deliberado: menos secretos que rotar.

## Integración GitHub ↔ Cloudflare

Cloudflare Pages accede al repositorio mediante una **GitHub App** instalada en
la cuenta `alvaretto`.

- Verla o revocarla: <https://github.com/settings/installations>
- Verla desde Cloudflare: *proyecto → Settings → Build → Git repository*

Si el despliegue automático deja de funcionar, revisar primero que la app siga
instalada y con acceso al repositorio `portfolio`.

## Cómo recuperar el control

**Si se pierde el acceso a Cloudflare:** el sitio sigue en línea, pero no se
puede desplegar. El contenido íntegro está en GitHub; se puede recrear el
proyecto en cualquier hosting estático en minutos.

**Si se pierde el acceso a GitHub:** el repositorio local en `~/Proyectos-2026/Proyectos-Varios/Emprendimientos/MiTercerMillon/portfolio`
conserva todo el historial. Basta crear otro remoto y reconectar Cloudflare.

**Si se pierden ambos:** mientras exista `~/Proyectos-2026/Proyectos-Varios/Emprendimientos/MiTercerMillon/portfolio` en disco, el proyecto
está completo. Es un directorio de menos de 300 KB: conviene incluirlo en
cualquier copia de seguridad.

## Cómo transferirlo a un cliente

1. Transferir el repositorio en GitHub (*Settings → Transfer ownership*)
2. Que el cliente cree su propia cuenta de Cloudflare y conecte el repo
   siguiendo [Documentación técnica](TECNICA.md#arquitectura)
3. Entregarle este directorio `docs/` completo
4. **No compartir el token OAuth de Wrangler** — es personal e intransferible

## Limpieza pendiente del historial

Los primeros commits incluyeron `.wrangler/cache/`, que contiene el correo
`luisernestomarceloberni@gmail.com`. Ya está fuera del repositorio y añadido
al `.gitignore`, pero **sigue presente en el historial público de git**.

No es una credencial y el riesgo es bajo (spam, principalmente). Para
eliminarlo del todo hay que reescribir el historial y forzar la subida:

```bash
cd ~/Proyectos-2026/Proyectos-Varios/Emprendimientos/MiTercerMillon/portfolio
FILTER_BRANCH_SQUELCH_WARNING=1 git filter-branch -f \
  --index-filter 'git rm -r --cached --ignore-unmatch .wrangler' \
  --prune-empty -- --all
git push --force origin master
```

> ⚠️ Reescribe el historial. Es seguro aquí porque nadie más ha clonado el
> repositorio, pero conviene hacer una copia del directorio antes.

## Buenas prácticas

- Activar 2FA en Cloudflare y en GitHub
- No añadir nunca tokens ni contraseñas a este repositorio
- Revisar cada cierto tiempo las apps autorizadas en
  <https://github.com/settings/installations>
- Si algún día se añade un token de API, guardarlo como *secret* de GitHub o
  como variable de entorno, nunca en un archivo versionado
