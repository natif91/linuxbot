# LinuxBot (prototipo local)

Chatbot educativo que recomienda una distribución de Linux mediante un árbol de decisiones explícito (sin puntajes ni IA).

## Cómo ejecutarlo
Descomprimí la carpeta y abrí `index.html` en el navegador (doble clic). No requiere servidor ni Internet; solo el botón «Visitar sitio oficial» necesita conexión.

## Archivos
- `index.html`: estructura (encabezado, área de chat, botón «Volver a empezar»).
- `styles.css`: diseño responsivo, modo claro/oscuro automático.
- `script.js`: árbol (`TREE`), resultados (`R`) y lógica del chat.
- `README.md`: este documento.

## Organización del árbol
Cada línea de `TREE` es una pregunta: `ID|Texto|Opción>Destino|Opción>Destino`. El destino es otro ID de pregunta o un resultado `R0`–`R10`. Los módulos compartidos son preguntas más: **S** (`S1`–`S3`), **M-OFI** (`O1`–`O2`), **M-DIS** (`DIS1`–`DIS5`); otros recorridos entran a ellos como a cualquier pregunta. Prefijos: `E/A/B/C/D` Estudiar, `P` Programar, `G` Juegos, `F` Familiar, `W` Profesional, `X` Experimentar. El estado (`state`) guarda solo el recorrido actual y se borra al reiniciar.

## Agregar o modificar una pregunta
Editá o agregá una línea en `TREE` con un ID nuevo y apuntá a ella desde la opción que corresponda. Verificá que todos los destinos existan (ver «Pruebas»).

## Agregar un resultado
Agregá una entrada `R.R11 = [...]` (o dentro del objeto `R`) con 10 campos: nombre, sitio oficial, color, perfil, justificación, ventajas, limitaciones, requisitos, alternativa, motivo de la alternativa. Luego usá `R11` como destino en `TREE`.

## Pruebas
1. Lógica: con Node instalado, ejecutá un script que haga `require('./script.js')` (exporta `Q` y `R`) y compruebe que cada destino exista, que toda pregunta sea alcanzable desde `Q0` y que cada recorrido termine en un resultado.
2. Manual: probá un recorrido por cada perfil de `Q0`, los módulos S, M-OFI y M-DIS desde distintos recorridos (por ejemplo A→S, G→S, D→M-DIS, W→M-OFI), el bucle `G2 → G2a → G2`, el botón «Volver a empezar» y la navegación con Tab/Enter y en pantalla angosta.

## Observaciones sobre los documentos (no aplicadas, a decidir)
1. `C2` no tiene opción «Otro» (sí `C3`). Un principiante con otro interés no encuentra su opción. Se implementó tal como está en la tabla.
2. `F6` Limitada → `R10` es tajante para videollamadas en navegador. Implementado según la tabla.
3. El módulo S nunca devuelve R6, R7 ni R8 (intencional).

## Incorporarlo en Google Sites (más adelante)
Google Sites no permite subir archivos JS/CSS sueltos. Opciones: alojar la carpeta (por ejemplo GitHub Pages) e incrustarla con *Insertar › Incorporar › Por URL*, o unir los tres archivos en un solo HTML y pegarlo en *Incorporar › Código*.

## Cambios de la versión 2 (contenido de resultados)
- Perfiles R0–R10 reescritos a partir de las rutas reales que llegan a cada resultado (el campo «Perfil» admite una lista de textos).
- Se quitó la insignia con la inicial y la etiqueta con el ID del resultado de la tarjeta.
- Pendientes: verificar requisitos y soporte de cada versión en los sitios oficiales antes de publicar; el prefijo «[Módulo …]» en las preguntas S1, O1 y DIS1 es visible para el usuario; C7 decide Debian por RAM tras la experiencia (ver informe).

## Cambios de la versión 3 (revisión final)
- **R10:** sin botón de sitio oficial; nueva sección «Qué podés hacer» (campo opcional `r[10]` del resultado). Si `r[1]` está vacío, la tarjeta no muestra el botón.
- **C7 → C11:** «Menos de 8 GB» ya no lleva directo a Debian; la nueva pregunta C11 (prioridades y necesidades de software) deriva a R8, R5, R6 o R7.
- **F6 → F7/F8/F8a:** «Limitada» ya no lleva directo a R10. F7 evalúa el tipo de videollamadas, F8 la RAM y los periféricos (F8a ayuda a consultarla). El umbral de 2 GB es un criterio orientativo.
- **C2:** nueva opción «Otro» → C4.
- Se eliminaron los prefijos «[Módulo …]» del texto visible de S1, O1 y DIS1.
- Resueltos los pendientes anteriores sobre C2 y F6. Siguen pendientes: W3 → R8 (decide por RAM menor a 4 GB), verificar requisitos y soporte en los sitios oficiales, y probar en un navegador real.

## Cambios de la versión 4
- **F7/F8/F9:** el recorrido de videollamadas ya no decide R2 o R10 por «2 GB de RAM». Ahora evalúa el tipo de videollamada (F7), las características del equipo (F8) y, si hay dudas, una videollamada de prueba (F9, con F8a y F9a como pasos aclaratorios).
- **W3 → W7:** «Menos de 4 GB» ya no lleva directo a Debian; W7 pregunta las prioridades (estabilidad → R8, documentación → R5, tecnologías recientes → R6).
- **C11:** se ajustaron las etiquetas (mismos destinos).
- **R0–R10:** justificaciones reescritas y requisitos reorganizados (mínimo, uso cómodo, aplicaciones propias).
- Pendientes: confirmar requisitos y soporte en los sitios oficiales; probar en un navegador real.

## Si se ve sin estilos y no responde
Significa que `index.html` se abrió sin `styles.css` y `script.js` al lado (por ejemplo, desde dentro del ZIP sin descomprimir). Descomprimí la carpeta completa y abrí `index.html` desde ahí, o usá `linuxbot-un-solo-archivo.html`, que lleva todo adentro.

## Cambios de la versión 5
- **R0:** ahora se titula «Compatibilidad: conservar el sistema necesario y evaluar una alternativa con Linux». La tarjeta describe cada situación (programa de Windows, programa de macOS, juego con antitrampas, dispositivo incompatible) con su propio camino en «Qué podés hacer». No tiene botón de sitio oficial. Las rutas hacia R0 no cambiaron.

## Cambios de la versión 6
- Cada tarjeta incluye «Cómo se relaciona con tus respuestas»: la respuesta decisiva del usuario, la combinación de criterios que lleva a ese resultado (campo `r[11]`) y la lista de preguntas con las respuestas que dio. Los pasos aclaratorios no se listan.

## Cambios de la versión 7 (contenido técnico de las fichas)
- «Por qué se eligió» ahora tiene cuatro partes: ¿Qué es y qué la caracteriza?, Ventajas, Desventajas o limitaciones y ¿Por qué se recomienda? Datos en `r[4]` (dos textos), `r[5]` (ventajas) y `r[6]` (limitaciones).
- Requisitos verificados con documentación oficial para Linux Mint (blog de Linux Mint), Ubuntu (help.ubuntu.com), Fedora (docs.fedoraproject.org) y Pop!_OS (guía de System76). Lubuntu, Zorin, Debian y Arch remiten al sitio oficial: sus cifras no se verificaron.
