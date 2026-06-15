
# Mod 26 - Cryptography

## Descripción

El reto indicaba que se trataba de un desafío de criptografía relacionado con ROT13.

## Archivo proporcionado

Se descargó el archivo `values.txt` y se obtuvo el siguiente texto cifrado:

```text
cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_45559noq}
```

## Evidencia

### Desafío

<p align="center">
  <img src="https://i.postimg.cc/L5bphCB4/Captura-desde-2026-06-14-22-56-19.png" width="1000">
</p>

### Decodificación en dCode

Se utilizó la herramienta ROT13 de dCode para descifrar el mensaje.

<p align="center">
  <img src="https://i.postimg.cc/v8qMPTRP/Captura-desde-2026-06-14-22-54-40.png" width="1000">
</p>

## Procedimiento

1. Descargué el archivo `values.txt`.
2. Observé que el reto mencionaba explícitamente ROT13.
3. Copié el texto cifrado en la herramienta ROT13 de dCode.
4. Ejecuté la decodificación.
5. Obtuvé la flag del desafío.

## Flag

```text
picoCTF{next_time_I'll_try_2_rounds_of_rot13_45559abd}
```

## Conclusión

El reto utilizaba el cifrado ROT13, un cifrado César con desplazamiento de 13 posiciones. Al aplicar ROT13 al texto proporcionado se recuperó directamente la flag.
