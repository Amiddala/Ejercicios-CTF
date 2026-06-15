# Timestamped Secrets - Cryptography

## Descripción

El reto indicaba que un mensaje fue cifrado con AES en modo ECB, pero la clave fue derivada de algo tan simple como el tiempo actual. Se debía descubrir la clave y desencriptar la flag.

## Archivos proporcionados

- `message` — archivo con el ciphertext en hexadecimal
- `code` — script de encriptación que revela el esquema utilizado

## Evidencia

### Reto

<p align="center">
  <img src="https://i.postimg.cc/brpxsP9c/Captura-desde-2026-06-14-23-36-23.png" width="1000">
</p>

### Código fuente y ejecución

<p align="center">
  <img src="https://i.postimg.cc/hvcLfnb5/Captura-desde-2026-06-14-23-40-47.png" width="1000">
</p>




<p align="center">
  <img src="https://i.postimg.cc/bwW11kRc/Captura-desde-2026-06-14-23-44-12.png" width="1000">
</p>



## Análisis del código de encriptación

Al revisar el script proporcionado se identificó el esquema completo:

```python
from hashlib import sha256
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import time

def encrypt(plaintext: str, timestamp: int) -> str:
    key: bytes = sha256(str(timestamp).encode()).digest()[:16]
    cipher = AES.new(key, AES.MODE_ECB)
    padded: bytes = pad(data_to_pad=plaintext.encode(), block_size=AES.block_size)
    ciphertext: bytes = cipher.encrypt(padded)
    return ciphertext.hex()
```

La clave AES se generaba así:
1. Se tomaba el timestamp Unix como entero (`int(time.time())`)
2. Se convertía a string y se codificaba en bytes
3. Se calculaba el SHA-256 de ese string
4. Se tomaban los primeros 16 bytes del hash como clave AES-128

Además, el script imprimía un hint con el timestamp aproximado de encriptación (`"The encryption was done around {timestamp} UTC"`), lo que acotaba el espacio de búsqueda a una ventana de segundos.

## Procedimiento

1. Extraje el ciphertext del archivo `message`.
2. Tomé el timestamp del hint provisto por el reto.
3. Escribí un script que itera sobre una ventana de ±60 segundos alrededor del hint para probar cada posible clave derivada.
4. Por cada timestamp candidato, generé la clave con SHA-256 y traté de desencriptar y remover el padding PKCS7.
5. Si el resultado decodificaba como UTF-8 y contenía `picoCTF`, se imprimía la flag.

```python
from hashlib import sha256
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

ciphertext = bytes.fromhex("dcc2a6a4cf3dbc69a929aa7c4e3c33e7558eef1f2244bde76e450b065188db38")

hint_timestamp = 1770242624

for ts in range(hint_timestamp - 60, hint_timestamp + 61):
    key = sha256(str(ts).encode()).digest()[:16]
    cipher = AES.new(key, AES.MODE_ECB)
    try:
        decrypted = unpad(cipher.decrypt(ciphertext), AES.block_size)
        texto = decrypted.decode('utf-8')
        if 'picoCTF' in texto:
            print(f"¡Encontrado! timestamp={ts}")
            print(f"FLAG: {texto}")
            break
    except:
        continue
```

El timestamp exacto fue `1770242624`, que coincidió directamente con el hint.

## Herramientas utilizadas

- Python 3
- pycryptodome (`pip install pycryptodome`)

## Flag

```text
picoCTF{sa3S_sEc9t_5e67da97}
```

## Conclusión

El fallo de seguridad consistía en derivar la clave AES directamente del timestamp Unix, un valor predecible y de espacio muy reducido. Al conocer el momento aproximado de cifrado gracias al hint, fue posible hacer fuerza bruta sobre la ventana de ±60 segundos y recuperar la clave exacta que desencriptaba el mensaje.
