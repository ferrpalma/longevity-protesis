# Deploy Landing Longevity ForCell · Cirugía Prótesis

**Estado:** aprobado por Dr. Rodrigo Mardones · listo para producción
**Fecha:** 23 abril 2026

---

## Resumen de lo que queda

1. Mergear el PR en GitHub (ferrpalma/longevity-protesis #1)
2. Subir los archivos al hosting de WordPress en subdirectorio `/protesis/`
3. Verificar que todo carga
4. Avisar a Rodrigo

No requiere editar código. Solo merge + upload.

---

## Pre-requisitos en esta PC

- Acceso al repo local `longevity-protesis` (o clonarlo fresco de GitHub)
- `gh` CLI autenticado en tu cuenta `ferrpalma`
- Credenciales FTP / cPanel del hosting de `longevityforcell.cl`
- Cliente FTP (FileZilla) o acceso al cPanel del hosting

---

## Paso 1 · Sincronizar repo local y mergear PR

```bash
cd ~/Documents/longevity-protesis        # ajustar ruta si es otra
git checkout main
git pull origin main
gh pr merge 1 --merge --repo ferrpalma/longevity-protesis
git pull origin main
```

Después del merge, tu rama `main` en `ferrpalma/longevity-protesis` tiene la versión final.

GitHub Pages redeploya automáticamente en ~1 min → `https://ferrpalma.github.io/longevity-protesis/` refleja el cambio.

---

## Paso 2 · Revisar localmente antes de subir

```bash
python3 -m http.server 8765
```

Abrir `http://localhost:8765/` en el browser.

**Checklist visual rápido:**
- [ ] Hero: 4 médicos con cabezas visibles (panorámica 16/9, sin cortes)
- [ ] Eyebrow hero dice "PROGRAMA 2026 · 100 CUPOS DISPONIBLES"
- [ ] H1 dice "...Equipo liderado por el Dr. Rodrigo Mardones, Fellowship Mayo Clinic."
- [ ] Bloque "CLÍNICA MONTEBLANCO" (fondo teal): 4 bullets, números en blanco (no dorado), sin overflow del "3.000+"
- [ ] Pricing: borde gris teal, tag "PROGRAMA 2026" teal oscuro con texto blanco
- [ ] FAQ recuperación dice "aproximadamente 3 meses" (no "3-6 meses")
- [ ] FAQ Isapre/Fonasa menciona PAD suspendido
- [ ] **NO hay** banner amarillo "Versión preliminar para Dr. Mardones"
- [ ] **NO hay** bloque `[POR INTEGRAR]` antes del botón final
- [ ] **NO hay** mención a "99% casos exitosos"
- [ ] Botones dorados con texto negro (no verde oscuro)
- [ ] `Ctrl+Shift+R` para evitar cache si ves algo raro

Si todo OK → siguiente paso.

---

## Paso 3 · Subir al hosting

**Destino:** `longevityforcell.cl/protesis/` → carpeta `public_html/protesis/` en el hosting.

### Opción A — FTP / SFTP (FileZilla)
1. Conectar con credenciales del hosting
2. Navegar a `/public_html/` (o `/www/`, depende del hosting)
3. Crear carpeta `protesis/`
4. Subir dentro:
   - `index.html`
   - Carpeta `img/` completa

### Opción B — cPanel File Manager
1. Login cPanel
2. File Manager → `public_html/`
3. `+Folder` → nombre: `protesis`
4. Entrar a `protesis/`
5. Upload → seleccionar `index.html` y la carpeta `img/` (o subir un zip y extraerlo dentro)

### ⚠️ NO subir estos archivos
- `.git/` (carpeta oculta)
- `CONTEXT-CLIENTE.md`
- `DEPLOY.md`
- `README.md`
- `.gitignore`

Solo necesitas `index.html` + `img/`.

---

## Paso 4 · Verificación post-deploy

Abrir en navegador incógnito (para evitar cache):

- [ ] `https://longevityforcell.cl/protesis/` responde 200 y carga
- [ ] Logo Longevity arriba izquierda visible
- [ ] Foto del equipo (4 médicos) carga en el hero
- [ ] Las 5 fotos individuales de doctores cargan en la sección Equipo
- [ ] Click en cualquier botón "Agendar evaluación gratuita" → abre WhatsApp con `+56 9 4415 7542` y mensaje pre-escrito
- [ ] DevTools → Network → recargar → sin 404 en ningún recurso
- [ ] Vista mobile (DevTools → responsive, iPhone 13): sección completa navegable, sin overflow horizontal, botones tocables

### Test del Open Graph (preview al compartir)
1. Abrir `https://www.opengraph.xyz/url/https%3A%2F%2Flongevityforcell.cl%2Fprotesis%2F`
2. Debe mostrar la foto del equipo + título + descripción

Si algún test falla, **NO pasar el link a Rodrigo** hasta resolverlo.

---

## Paso 5 · Avisar a Rodrigo

Mensaje sugerido (WhatsApp):

```
Rodrigo, la landing ya está publicada con todos los ajustes:
https://longevityforcell.cl/protesis/

Incluye:
- Cambio de recuperación a ~3 meses
- Aclaración Isapre/Fonasa (todos como particular, PAD suspendido)
- Los 100 cupos como ancla del Programa 2026

Cualquier último ajuste me avisas. Si está OK, seguimos con la parte de captación (formulario y pixel Meta si vas a correr campaña paga).
```

---

## Pendientes (no bloquean el live)

- **Formulario de captura** en CTA final (hoy solo WhatsApp). Necesario si se corre Meta Ads.
- **Pixel Meta + Conversions API** instalados en el `<head>`. Sin esto, Meta no optimiza.
- **Decisión sobre tracking**: ¿GHL? ¿Typeform? ¿form nativo con Zapier?

---

## Troubleshooting rápido

- **Imágenes no cargan post-upload** → revisar permisos (644 archivos, 755 carpetas) y nombres exactos con mayúsculas/minúsculas.
- **CSS roto** → el HTML tiene CSS inline, no debería fallar. Si pasa, probablemente el upload se cortó — re-subir `index.html` completo.
- **Cache de Cloudflare / CDN** → purgar desde el panel del hosting o usar `?v=2` al final de la URL para forzar refresh.
- **SSL / HTTPS** → si `http://` funciona pero `https://` no, el hosting requiere configurar certificado del subdirectorio (suele ser automático con Let's Encrypt, pero a veces hay que reclamar).

---

## URLs de referencia

- **Preview actual aprobado:** https://ferr-xmkt.github.io/longevity-protesis/
- **Repo canónico:** https://github.com/ferrpalma/longevity-protesis
- **PR con cambios finales:** https://github.com/ferrpalma/longevity-protesis/pull/1
- **Destino producción:** https://longevityforcell.cl/protesis/

---

**Tiempo estimado total:** 30-45 min (10 min repo/merge + 15-20 min upload + 5-10 min verificación).
