# Assets de Rutástrofe (pygame, vista desde arriba)

Todos los dibujos son PNG con fondo transparente (excepto los fondos de `ui/`). Los sprites vienen ya ampliados ×3 con
borde oscuro (pixel art), así que se cargan y se dibujan sin escalar. `ejemplo_escena.png` muestra cómo encajan.

## Medidas del juego (ventana 640 × 720)
| Parte | Ancho |
|---|---|
| Andén | 60 px (x = 26 a 86) |
| Berma izquierda | 54 px (x = 86 a 140) |
| 3 carriles | 120 px cada uno (x = 140 a 500); centros en x = 200, 320 y 440 |
| Berma derecha | 54 px (x = 500 a 554) |
| Andén derecho | 60 px (x = 554 a 614) |

Los carriles se repiten hacia abajo: para que la carretera "avance", dibuja los tiles con un desplazamiento vertical que crece
(`y = (y0 + velocidad) % 120`). `asfalto`, `berma_*`, `anden_*`, `pared_tunel_*` y `linea_discontinua` repiten sin costuras.

## Contenido
- `jugador/`: `moto_normal`, `moto_izquierda`, `moto_derecha`, `moto_golpe` (silueta blanca para el parpadeo al chocar).
- `obstaculos/`: 6 carros (`gris`, `verde`, `azul`, `rojo`, `taxi`, `oxidado`), `bus`, `camion`, `cono`, `barricada`, `hueco`, `charco_aceite`, `escombros`, `puesto_mercado`.
- `semaforos/`: `semaforo_rojo`, `semaforo_amarillo`, `semaforo_verde`, `semaforo_apagado` (se ponen sobre el piso, de modo que se puede pasar por encima o rodearlos), `cebra_carril`, `linea_pare`.
- `items/`: `caja_comida`.
- `efectos/`: `alarma_1` a `alarma_4` (anillos que crecen cuando suena la alarma), `choque_1` a `choque_4`, `brillo_1` a `brillo_4` (al recoger una caja), `flash_rojo` y `oscuridad_tunel` (capas de 640×720 para poner encima).
- `escenario/`: `asfalto`, `linea_discontinua`, `berma_izq/der`, `anden_barrio_izq/der`, `anden_mercado_izq/der`, `pared_tunel_izq/der`, `techo_1` a `techo_3`, `porton_meta_cerrado`, `porton_meta_abierto`, `linea_meta`.
- `ui/`: `corazon_lleno`, `corazon_vacio`, `icono_caja`, `icono_jax`, `icono_cabezota`, `barra_marco`, `barra_verde/amarillo/rojo`, `boton_normal`, `boton_hover`, `panel_oscuro`, `logo`, `fondo_menu`, `fondo_ganaste`, `fondo_perdiste`.
- `personajes/`: arte original de Jax, Lía, los dos gatos, el perro, el plato, la Cabezota (`cabezota` y `cabezota_oscura`) y `jax_busto`.
- `fuentes/`: Fredoka One (títulos) y Patrick Hand (textos), ambas con licencia libre (OFL).

## Sugerencia por tramo
- Barrio: `anden_barrio_*`, carros, conos, barricadas, huecos.
- Avenida: lo mismo, más semáforos y `cebra_carril`.
- Mercado: `anden_mercado_*`, `puesto_mercado`, `charco_aceite`, `escombros`.
- Túnel: `pared_tunel_*` con `oscuridad_tunel` encima.
- Recta final: barricadas y semáforos, y al final `porton_meta_*` y `linea_meta`.

## Cómo cargarlos en pygame sin usar clases
```python
import pygame
def cargar(ruta):
    return pygame.image.load("assets/" + ruta).convert_alpha()

imagenes = {
    "moto": cargar("jugador/moto_normal.png"),
    "caja": cargar("items/caja_comida.png"),
    "carro_azul": cargar("obstaculos/carro_azul.png"),
}
# Cada objeto del juego puede ser un diccionario: {"imagen": "carro_azul", "x": 200, "y": -150, "carril": 0}
```
Si ven que algún sprite queda grande o pequeño, pueden usar `pygame.transform.scale` una sola vez al cargar (nunca en cada cuadro).
