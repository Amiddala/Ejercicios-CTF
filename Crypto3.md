# 13 - Cryptography

## Descripción

El reto proporcionaba directamente un texto cifrado junto con una pista que mencionaba ROT13.

## Texto proporcionado

```text
cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}
```

## Evidencia

### Reto

<p align="center">
  <img src="https://i.postimg.cc/Twb7nk6w/Captura-desde-2026-06-14-23-00-40.png" width="500">
</p>

### Identificación del cifrado

Se copió el texto en el identificador de cifrados de dCode, el cual sugirió que el mensaje estaba cifrado con ROT13.
<p align="center">
  <img src="https://i.postimg.cc/nzmRqdJF/Captura-desde-2026-06-14-23-02-33.png" width="500">
</p>






<p align="center">
  <img src="https://i.postimg.cc/C18Pbvgd/Captura-desde-2026-06-14-23-02-19.png" width="500">
</p>

## Procedimiento

1. Copié el texto cifrado del reto.
2. Utilicé el identificador de cifrados de dCode.
3. La herramienta sugirió ROT13 como método más probable.
4. Abrí la herramienta ROT13 de dCode.
5. Decodifiqué el mensaje y obtuve la flag.

## Herramientas utilizadas

- dCode Cipher Identifier
- dCode ROT13 Decoder

## Flag

```text
picoCTF{not_too_bad_of_a_problem}
```

## Conclusión

El desafío utilizaba ROT13, un cifrado de sustitución con desplazamiento de 13 posiciones. Una vez identificado el método de cifrado, la flag se obtuvo de forma inmediata.
