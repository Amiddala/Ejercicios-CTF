# Shared Secrets - Cryptography

## Descripción

El reto indicaba que un mensaje fue cifrado usando un "shared secret", y que una de las partes del intercambio filtró información. Se debía reconstruir el secreto para obtener la flag.

## Archivos proporcionados

- `message` — archivo con los valores del intercambio (p, A, b, enc)
- Código fuente para entender el esquema de cifrado

## Evidencia

### Reto

<p align="center">
  <img src="https://i.postimg.cc/Hk5qC4nZ/Captura-desde-2026-06-14-23-19-29.png" width="1000">
</p>

### Código fuente y ejecución

<p align="center">
  <img src="https://i.postimg.cc/cLYpNMCF/Captura-desde-2026-06-14-23-32-44.png" width="1000">
</p>

## Procedimiento

El reto implementa un intercambio de claves tipo **Diffie-Hellman** donde se filtró el valor privado `b`. La encriptación consiste en un XOR simple con el shared secret módulo 256.

Los valores del archivo `message` eran:

```text
p = 2945889223405899717437265251282889237686559573793157647835455871476745968258470904453353424137291997100245176877...
A = 1925392662772808546939197421118358205835520399582911779574770525906575552129444284907344977079741952528900583391...
b = 2348305787882664061354385580996892615192009596530928729579256657766736150757668411620580993437412955274875319957...
enc = bytes.fromhex("6178727e5245576a75794e622272632265 4e23727028267372276c")
```

Los pasos para resolver el reto fueron:

1. Identifiqué que `A = g^a mod p` (clave pública) y `b` es la clave privada filtrada.
2. Calculé el **shared secret**: `shared = pow(base=A, exp=b, mod=p)`.
3. Derivé la clave de cifrado: `key = shared % 256`.
4. Desencripté el mensaje haciendo XOR byte a byte: `flag = bytes([x ^ key for x in enc])`.
5. Decodifiqué el resultado con `.decode()` para obtener la flag en texto plano.

El script `decrypt.py` ejecutado fue:

```python
p = 2945889223405...
A = 1925392662772...
b = 2348305787882...
enc: bytes = bytes.fromhex("6178727e5245576a75794e622272632265 4e23727028267372276c")

shared: int = pow(base=A, exp=b, mod=p)
key: int = shared % 256
flag: bytes = bytes([x ^ key for x in enc])
print(flag.decode())
```

## Herramientas utilizadas

- Python 3 (función nativa `pow()` con tres argumentos para exponenciación modular eficiente)

## Flag

```text
picoCTF{dh_s3cr3t_2ca97bc6}
```

## Conclusión

El desafío simulaba un intercambio Diffie-Hellman donde la clave privada `b` fue "filtrada" por una de las partes. Con `b` conocido y la clave pública `A`, fue posible recalcular el shared secret y, a partir de él, la clave XOR que desencriptaba el mensaje cifrado.
