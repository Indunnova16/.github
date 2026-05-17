# Guía para abrir issues en repos Indunnova

Esta guía es para todo el que abra issues en cualquier repo del portafolio Indunnova (Colorplastic, RGDAire, Arcopack, Consof, NovaIAsistente, etc.). Si los issues llegan con esta estructura, el ciclo **reporte → fix → validación** se acorta a horas en vez de días.

## Por qué importa

En Indunnova usamos un agente automatizado (Claude Code) que trabaja los issues siguiendo un protocolo estándar: analiza, encuentra causa raíz en BD/logs de producción, arregla, despliega, prueba en prod y comenta el estado. **El protocolo solo es tan bueno como el issue que recibe.** Un issue sin contexto cuesta 2 horas de ida y vuelta para entender qué hay que arreglar.

---

## Plantilla básica (copy-paste)

Cuando abras un issue nuevo, usa esta estructura:

```markdown
## Qué pasa (síntoma observable)
Una frase clara de lo que ves mal.
Ej: "Al guardar una novedad de molido sin seleccionar motivo, aparece la pantalla en blanco con error 500."

## Dónde ocurre
- **Módulo / pantalla**: Producción → Molido → botón "Registrar Novedad"
- **URL exacta** (si la conoces): https://colorplastic-app-...run.app/gestion/produccion/molido/
- **Usuario que lo reportó**: Heidi (calidad) / Alexandra (gerencia) / yo mismo
- **Fecha del primer reporte**: 2026-05-15

## Pasos para reproducir
1. Login con mi usuario.
2. Entrar a Producción → Molido.
3. Click en "Registrar Novedad" arriba a la derecha.
4. Dejar el campo motivo en blanco.
5. Click en "Guardar".

## Qué esperaba que pasara
Que aparezca un mensaje "Debe seleccionar un motivo" y el formulario se quede abierto para corregir.

## Qué pasa en realidad
El servidor devuelve error 500 y pierdo todo lo que había escrito.

## Adjuntos
- Screenshot del error (arrastrá la imagen aquí)
- Si hay PDF / Excel de ejemplo, adjuntarlo también
- Link a grabación si se discutió en reunión
```

---

## Reglas de oro

### ✅ Hacer

1. **Un bug = un issue.** Si encontraste 3 bugs distintos, abrir 3 issues. Permite trabajarlos en paralelo y cerrarlos por separado.
2. **Screenshots con contexto.** Adjuntar la captura **y** describir qué muestra ("se ve el formulario con el campo Motivo vacío y el error 500 abajo").
3. **Etiquetas desde el inicio.** Mínimo una etiqueta de prioridad (ver abajo).
4. **Referenciar issues relacionados.** Si este bug se relaciona con #45 que ya fue arreglado, escribir "Relacionado con #45 — el fix de ese issue no cubre el caso de Y".
5. **Linkear grabaciones de reunión.** Si el acuerdo viene de una reunión con cliente (Loom, Drive, Teams), pegar el link. Mucho contexto crítico sale de ahí y NO está en el body del issue.
6. **Actualizar con comentarios cuando aparezca info nueva.** "Probé X y sigue fallando con Y mensaje" → comentario, NO issue nuevo.

### ❌ Evitar

1. **Issues con solo título.** "No funciona inventario" sin body = imposible de trabajar sin ir a preguntar.
2. **"Mira la imagen" sin más.** El screenshot ayuda pero no explica qué se hizo para llegar ahí.
3. **Múltiples bugs en un mismo issue.** Confunde los hilos de discusión y bloquea el cierre del que ya está listo.
4. **Cerrar el issue tú mismo antes de validar el fix.** El agente NO cierra los issues — los asigna a Indunnova y espera tu validación. Si lo cerrás vos sin probar, el ciclo de feedback se rompe.
5. **Borrar comentarios viejos "porque ya no aplican".** Son historia útil: muestran qué se intentó antes y por qué se descartó.

---

## Etiquetas

Asigná **al menos una de prioridad**. Las de área son útiles si hay backlog grande.

### Prioridad
| Etiqueta | Cuándo usarla |
|----------|---------------|
| `Urgente` | Bloquea operación diaria (operarios no pueden registrar producción, despachos se caen, etc.) |
| _(sin etiqueta especial)_ | Bug importante pero hay workaround |
| `Futuro` | Mejora deseable, no urgente |
| `documentation` | Solo cambios de docs internos (no se notifica al cliente al cerrar) |

### Área (opcional, custom por repo)
`produccion`, `inventario`, `fichas-tecnicas`, `despachos`, `crm`, `calidad`, etc. Mantener nombres consistentes dentro de cada repo.

---

## Cuándo agregar comentarios

Cada vez que aparezca información nueva sobre el mismo bug:

- ✅ "Probé desde el celular y también pasa."
- ✅ "Después del último deploy ya funciona el alta, pero falta el detalle."
- ✅ Screenshot adicional con un caso que no estaba cubierto.
- ✅ "Conversé con [persona] y confirmó que el comportamiento esperado es X."

El agente lee **todos los comentarios** antes de tocar código. Lo que esté ahí, lo va a considerar.

---

## Referenciar entre issues y commits

| Frase | Significado |
|-------|-------------|
| `Bloqueado por #45` | No se puede arrancar hasta cerrar #45 |
| `Relacionado con #45` | Comparten causa raíz pero son independientes |
| `Cierra #45` (en un PR) | El PR termina #45 cuando se merge |
| `Fix aplicado en commit abc1234` (en comentario) | Da rastreabilidad al próximo que toque |

---

## Flujo esperado tras abrir el issue

1. **Vos** (o quien reporte) abre el issue siguiendo la plantilla.
2. **El agente** comenta:
   - Causa raíz encontrada (en BD/logs).
   - Qué se arregló (commit + revisión Cloud Run).
   - Estado por sub-item con emojis:
     - 🟢 validado en prod
     - 🟡 deployado, pendiente validación funcional
     - 🔵 implementado, no deployado
     - ⚠️ parcial / ❌ no hecho / ℹ️ decisión de scope
   - Acción esperada del cliente.
3. **El agente asigna a `Indunnova`** (NO cierra).
4. **Vos validás en pantalla** lo que el agente reportó.
5. Si funciona: cerrás el issue.
6. Si NO funciona: comentás "Probé y sigue mal porque X" → el agente vuelve a trabajarlo.

---

## Ejemplos reales (buenos y malos)

### ❌ Issue mal escrito
> **Título**: No funciona
>
> **Body**: Cuando le doy guardar no me deja.

Problemas:
- Qué módulo, qué pantalla, qué botón.
- Qué error muestra exactamente.
- Sin screenshot.
- Tiempo perdido pidiendo aclaraciones.

### ✅ Issue bien escrito
> **Título**: Error 500 al guardar novedad de molido sin motivo
>
> **Body**:
> ## Qué pasa
> En la pantalla de Producción → Molido, al click en "Registrar Novedad" sin seleccionar motivo ni operario, sale error 500 (pantalla en blanco "Internal Server Error").
>
> ## Dónde
> - Módulo: Producción Molido
> - URL: `/gestion/produccion/molido/` → modal "Registrar Novedad"
> - Reportado por: Heidi (calidad)
> - Fecha: 2026-05-15
>
> ## Pasos
> 1. Login en prod.
> 2. Click "Producción" → "Molido".
> 3. Click botón "Registrar Novedad".
> 4. Dejar Motivo y Operario en blanco.
> 5. Click "Guardar".
>
> ## Esperado
> Mensaje "Debe seleccionar un motivo".
>
> ## Real
> Error 500 (screenshot adjunto).
>
> ## Adjunto
> [imagen]

Con este issue, el agente arranca directo en el Paso 2 (causa raíz) sin tener que adivinar.

---

## Casos especiales

### Issue del cliente que reporta varios bugs en un audio/video
Si el cliente mandó un audio/Loom mencionando 4 cosas distintas:
1. Transcribir o resumir el audio en el body.
2. Crear **4 issues separados**, uno por cada bug.
3. Linkear todos al issue "origen" o al video para preservar contexto.

### Issue que viene de una reunión con cliente
1. Crear el issue con la plantilla.
2. En el body agregar:
   ```
   ## Contexto de reunión (YYYY-MM-DD con [personas])
   Link a grabación: [URL]
   Min XX:XX-YY:YY — discusión clave: "..."
   Acuerdos:
   - Punto 1 acordado
   - Punto 2 acordado
   ```
3. El agente va a leer esto antes de arrancar.

### Issue que ya tiene un fix previo que no funcionó
Abrir un comentario nuevo en el issue existente, NO uno nuevo. Decir:
> "El fix del commit X no resolvió porque [síntoma específico]. Adjunto screenshot del caso que sigue fallando."

---

## Resumen ultra-corto

Si solo recordás una cosa: **un issue bien escrito tiene síntoma + dónde + pasos para reproducir + screenshot**. Todo lo demás es bonus.

---

*Versión 1.0 — 2026-05-17. Actualizar cuando cambie el protocolo del agente.*
