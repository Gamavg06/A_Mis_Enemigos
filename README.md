# A mis enemigos - Parodia interactiva sobre robo de codigo

Proyecto web autocontenido (`index.html` + pista principal `corrido.mp3`) con escena 3D en Canvas a 60 FPS, reproductor conectado a `Web Audio API`, sintetizador de banda sinaloense como alternativa en vivo, soporte para audio local (MP3) y mensajes personalizables por URL, enfocado como parodia sobre el plagio de proyectos y las copias sin credito.

## Caracteristicas principales

- **Pista principal (`corrido.mp3`)**: Elemento HTMLAudio oculto sin controles nativos ni opciones de descarga (`preload="metadata"`), conectado mediante `createMediaElementSource` -> `masterGain` -> `AnalyserNode` -> `audioCtx.destination`. Solo inicia la reproduccion y el `AudioContext` tras una accion del usuario ("Iniciar show" o "Reproducir"). Muestra duracion inicial de `03:16` (`196` s) actualizada dinamicamente con la duracion real al cargar metadatos, tiempo actual, barra de progreso interactiva para saltar a cualquier segundo y bucle opcional.
- **Alternativas de audio en el reproductor**:
  - **Sintetizador de banda sinaloense (Web Audio API)**: Boton `"Usar sintetizador"` / `"Usar pista principal"` con icono SVG propio para alternar en directo entre `corrido.mp3` y la composicion instrumental original de 72.0 s (`01:12`) en Fa Mayor a 108 BPM (compas de 2/4, 7 instrumentos sintetizados, reverberacion `ConvolverNode` y scheduler lookahead).
  - **Carga de audio local ("Cargar MP3")**: Boton vinculado a `<input type="file" accept="audio/*">` para reproducir un archivo local conectado al mismo `AnalyserNode` 3D.
- **Pantalla de inicio**: Presenta el titulo, la frase superior editable y el boton "Iniciar show" para inicializar el `AudioContext` solo tras accion del usuario, respetando las politicas de audio de los navegadores modernos.
- **Escena 3D de Flor Cosmica en Canvas (`CosmicFlower3D`)**:
  - Renderizado matematico en 3D a 60 FPS con proyeccion de perspectiva (`project(x, y, z, scale, distance)`), rotaciones en los ejes X/Y/Z (`rotateX`, `rotateY`, `rotateZ`) y composicion aditiva (`globalCompositeOperation = "lighter"`) sobre fondo negro obsidiana calido.
  - Secuencia continua de florecimiento progresivo sincronizada de `0.0` a `26.5 s` (y completa de inmediato con `prefers-reduced-motion: reduce`):
    1. Etapa 1 (`0.0 a 4.5 s`, desvanecimiento `4.5 a 6.3 s`): brazos espirales desde el centro con cabezas tipo cometa y estela luminosa.
    2. Etapa 2 (`4.5 a 13.0 s`): 12 petalos rojos interiores simetricos con curvatura en `z`, trazos concentricos y puntas rosadas.
    3. Etapa 3 (`13.0 a 21.0 s`): 16 lazos de seda fina y petalos blancos exteriores con cabezas trazadoras.
    4. Etapa 4 (`21.0 a 26.5 s`): capas densas de galaxia exterior, estrias interiores, centelleo estelar (`twinkle`), cometas orbitales y show completo al entrar la voz en `26.5 s` (con rotacion suave, respiracion organica y estelas al mover el puntero o tocar pantalla).
  - Buffers planos precalculados (`Float32Array` y `Uint8Array`) optimizados con 3000 particulas en escritorio (1020 de petalos interiores, 420 de arcos de seda y 1560 de galaxia exterior) y 1800 particulas en movil cuando el ancho es menor a 768 px (612 interiores, 252 de arcos y 936 de galaxia exterior), manteniendo `devicePixelRatio` acotado a 2.
  - Reactividad directa al audio (`bassEnergy`, `midEnergy`, `highEnergy` via `AnalyserNode` o envolvente ritmica a 108 BPM en `file://`) y boton "Ráfaga de código" que dispara ondas de cometas luminosos desde el centro por los petalos junto con chispas de codigo duplicables (`{ }`, `[ ]`, `( )`, `< />`, `const`, `commit`, `fork`, `copy()`, `git log`, `return`).
  - Cuatro acciones superiores con iconos SVG propios: "Oro y brasas" (flor carmesi y galaxia blanca con destellos dorados), "Palenque neón" (petalos carmesi/magenta y galaxia blanca/cian), "Código en llamas" (petalos rojo fuego/ambar y galaxia blanco dorado) y "Dedicar y compartir".
- **Versos sincronizados (`VERSOS_TIEMPOS`)**: Letra completa de 28 bloques (versos y pasajes instrumentales) calibrada al ritmo real de `corrido.mp3` (`03:16` / `196.176 s`), mostrando en pantalla la etiqueta de seccion, el bloque actual, el estilo visual diferenciado para pasajes instrumentales (`.is-instrumental`) y el siguiente bloque. Si se activa el sintetizador de `72` s (`SYNTH_TOTAL_DURATION`), los tiempos se escalan proporcionalmente usando `MAIN_REF_DURATION = 196` como referencia base para recorrer toda la pista.
- **Personalizar mensaje**: Modal "Personalizar mensaje" para editar la frase superior y la firma, generar un enlace con parametros (`?para=...&de=...`) y copiarlo al portapapeles. Tambien admite `?frase=...` al cargar la pagina.
- **Seccion "Cómo proteger tu trabajo"**: Tres consejos practicos con iconos SVG propios sobre licencias claras (archivo LICENSE explicito), historial de commits (atomicos, fechados y firmados) y atribucion y avisos en encabezados y documentacion tecnica.
- **Rendimiento y accesibilidad**: Densidad de pixeles acotada a un maximo de 2 (`devicePixelRatio`), pools preasignados de particulas y ondas, diseno responsivo y tactil de alto contraste, y respeto a `prefers-reduced-motion`.

## Configuracion editable (`VERSOS_TIEMPOS` y `APP_CONFIG`)

Al inicio de la etiqueta `<script>` dentro de `index.html` se encuentran el arreglo `VERSOS_TIEMPOS` y el objeto `APP_CONFIG` para ajustar facilmente los tiempos de los versos, los textos principales, los enlaces y los creditos del audio.

### Tabla completa de `VERSOS_TIEMPOS` (0 a 191.0 s)

Calibrada al ritmo real medido sobre `corrido.mp3` (`03:16` / `196.176 s`), con comparacion decimal directa (`currentTime >= segundo`).

| Indice | Segundo | Etiqueta | Instrumental | Texto |
| --- | --- | --- | --- | --- |
| 1 | 0 | Intro musical | Si | Intro de requinto y bajo |
| 2 | 14 | Intro musical | Si | Copiar es fácil, crear es lo que cuesta |
| 3 | 26.5 | Estrofa 1 | No | Siguen tirando en el chat, señal que voy jalando. |
| 4 | 30.0 | Estrofa 1 | No | Así es el rollo en el jale pa' los que me andan copiando, |
| 5 | 33.5 | Estrofa 1 | No | bajando mis repos viejos y mi chamba pirateando. |
| 6 | 37.5 | Estrofa 2 | No | Que no se les haga raro verme subiendo a prod. |
| 7 | 41.5 | Estrofa 2 | No | Yo sí me parto en el code, no vivo de lo que otro armó. |
| 8 | 47.5 | Estrofa 2 | No | Si no les cuadra mi stack, pos' qué chingados me importó. |
| 9 | 53.5 | Adorno musical | Si | Adorno de guitarra y bajo |
| 10 | 58.5 | Estrofa 3 | No | No me agüita el burnout ni el que me abrió de volada. |
| 11 | 62.0 | Estrofa 3 | No | Le sé bien a esta madre, no ocupo quién me respalde. |
| 12 | 65.5 | Estrofa 3 | No | Nomás le dan Ctrl+C y ni así les sale el jale. |
| 13 | 69.5 | Puente musical | Si | Puente musical |
| 14 | 73.5 | Estrofa 4 | No | De los proyectos volados ni me voy a agüitar. |
| 15 | 79.5 | Estrofa 4 | No | El coco no se los presto, eso no van a clonar. |
| 16 | 85.5 | Estrofa 4 | No | Hoy me ven bien parado y les toca aguantar. |
| 17 | 92.5 | Solo de requinto | Si | Puente musical y solo de requinto |
| 18 | 104.5 | Solo de requinto | Si | Requinto en vivo, prepárate para la siguiente estrofa |
| 19 | 115.5 | Estrofa 5 | No | Pa' los que se hicieron güeyes cuando tocó levantar, |
| 20 | 119.5 | Estrofa 5 | No | aquí seguimos al cien, volvimos a empezar. |
| 21 | 123.5 | Estrofa 5 | No | Sigan robando el branch, que yo sigo sin parar. |
| 22 | 127.5 | Cierre cantado | No | Siguen tirando en el chat, señal que voy jalando. |
| 23 | 132.0 | Cierre cantado | No | Sigan clonando el jale, que yo sigo cobrando. |
| 24 | 136.5 | Cierre cantado | No | Y aunque me dieron la espalda, aquí sigo coronando. |
| 25 | 142.5 | Outro musical | Si | Cierre instrumental con requinto y bajo |
| 26 | 158.0 | Outro musical | Si | Solo final de guitarra |
| 27 | 175.0 | Outro musical | Si | Copiar es fácil, crear es lo que cuesta |
| 28 | 191.0 | Final | Si | Acorde final |

Para ajustar los tiempos de sincronizacion:
1. Reproduce `corrido.mp3` en la pagina y anota en la barra de tiempo el segundo exacto en el que comienza cada frase.
2. Sustituye el valor numerico `segundo` de cada objeto en `VERSOS_TIEMPOS` manteniendo el orden cronologico ascendente.
3. Si utilizas la alternativa del sintetizador (`72` s), la aplicacion escala automaticamente estos tiempos de forma proporcional sobre `SYNTH_TOTAL_DURATION` (`72` s) usando `MAIN_REF_DURATION` (`196` s) para recorrer la letra completa.

### Objeto `APP_CONFIG`

```javascript
const APP_CONFIG = {
  FRASE_CORTA: "Copiar es fácil, crear es lo que cuesta",
  MIS_DATOS: {
    autor: "Autor Original del Codigo",
    portafolio: "#portafolio",
    github: "#github",
    tiktok: "#tiktok"
  },
  VERSOS_TIEMPOS: VERSOS_TIEMPOS,
  CREDITO_AUDIO: {
    titulo: "Aquí Sigo Coronando",
    autor: "Letra original propia",
    enlace: "https://treblo.com",
    licencia: "Pista generada con Treblo"
  }
};
```

- `FRASE_CORTA`: Frase superior mostrada por defecto en la pantalla de inicio y en el escenario principal cuando la URL no incluye parametros personalizados.
- `MIS_DATOS`: Objeto con `autor`, `portafolio`, `github` y `tiktok` utilizado para generar dinamicamente el pie de pagina.
- `VERSOS_TIEMPOS`: Referencia al arreglo editable `VERSOS_TIEMPOS`.
- `CREDITO_AUDIO`: Datos de atribucion mostrados en el pie de pagina sin boton ni atributo de descarga.

## Creditos de audio

La pista principal `corrido.mp3` (`03:16`) incluida en la raiz del proyecto cuenta con la siguiente atribucion mostrada en `#site-footer`:

- **Titulo**: Aquí Sigo Coronando
- **Autor**: Letra original propia
- **Enlace**: https://treblo.com
- **Licencia**: Pista generada con Treblo

## Parametros de URL soportados

Puedes compartir enlaces personalizados agregando parametros a la direccion del sitio:

- `?para=Tu+frase+aqui`: Define la frase superior del mensaje.
- `?frase=Tu+frase+aqui`: Alias alternativo compatible para la frase superior.
- `?de=Tu+firma`: Muestra la firma debajo de la frase superior.

Ejemplo:

```text
https://ejemplo.local/?para=Copiar+es+facil,+crear+es+lo+que+cuesta&de=Autor+Original
```

## Publicacion estatica

1. Verifica que `index.html`, `corrido.mp3` y `README.md` esten actualizados en la rama principal del proyecto.
2. Publica la carpeta raiz en cualquier servicio de alojamiento web estatico o servidor HTTP (no uses rutas locales absolutas; `index.html` carga `"corrido.mp3"` como ruta relativa).
3. Abre la URL resultante en un navegador de escritorio o movil y pulsa "Iniciar show" para activar el audio y la escena 3D.
