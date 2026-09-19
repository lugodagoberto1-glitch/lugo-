# lugo-
# Actividad 7 — Criptografía, Computación Cuántica y Blockchain

**SENA · Formación en Ciberseguridad**
Instructor: Ing. Diego Alejandro Barragán Vargas — Ingeniero Electrónico, Magíster en Ingeniería, Doctorando UDFJC

---

## Estructura sugerida del repositorio

```
.
├── README.md                     <- este archivo (tabla comparativa)
├── docs/
│   ├── Actividad7_Parte1_Conceptual.pdf
│   ├── Actividad7_Parte2_Diseno.pdf
│   └── Actividad7_Parte3_Chatbot.pdf
├── img/
│   ├── fig1_timeline.png         <- línea de tiempo
│   ├── fig2_tipos.png            <- tipos de criptografía
│   ├── fig3_infografia.png       <- infografía cuántica / poscuántica
│   └── fig4_blockchain.png       <- arquitectura blockchain
└── web/                          <- chatbot de ciberseguridad (Parte 3)
```

---

## 1. Ventajas, desventajas y ejemplos actuales de cada tipo de criptografía

| Tipo | Ventajas | Desventajas | Ejemplos actuales de uso |
|---|---|---|---|
| **Simétrica por bloques**<br>AES, Camellia, 3DES | Muy rápida, con aceleración en hardware (AES-NI). Bajo consumo de CPU y energía. Madura y ampliamente auditada. | Requiere compartir la clave por un canal seguro. El número de claves crece de forma cuadrática: n(n−1)/2. No ofrece no repudio. | BitLocker, FileVault y LUKS. Cifrado en reposo en AWS S3 y Azure. Fase de datos de TLS 1.3 y WPA3. |
| **Simétrica de flujo**<br>ChaCha20, RC4 (obsoleto) | Excelente rendimiento sin hardware dedicado. Ideal para móviles e IoT. No necesita relleno. | La reutilización del nonce rompe totalmente la seguridad. RC4 quedó obsoleto por sesgos estadísticos. | TLS 1.3 en Android y Chrome. WireGuard VPN. Mensajería en dispositivos de gama baja. |
| **AEAD**<br>AES-GCM, ChaCha20-Poly1305 | Confidencialidad e integridad en una sola operación. Evita el error clásico de cifrar sin autenticar. Alto rendimiento. | Extremadamente sensible a la repetición del nonce. Límites en el volumen de datos por clave. | TLS 1.3 (suites obligatorias). QUIC y HTTP/3. Signal y WhatsApp. |
| **Asimétrica RSA** | Resuelve la distribución de claves. Permite firma digital y no repudio. Soporte universal. | Lenta con datos grandes. Claves largas (3072+ bits). Vulnerable al algoritmo de Shor. | Certificados TLS/SSL y PKI corporativa. Firma de código y de PDF. Firma electrónica avanzada. |
| **Asimétrica ECC**<br>ECDSA, EdDSA, X25519 | Misma seguridad con claves mucho más cortas (256 bits ≈ RSA 3072). Menor ancho de banda y batería. Firmas compactas. | Implementación delicada: un nonce mal generado revela la clave privada. Desconfianza en algunas curvas NIST. Vulnerable a Shor. | Firmas de transacciones en Bitcoin y Ethereum. Claves SSH Ed25519. Apple Pay, passkeys y FIDO2/WebAuthn. |
| **Funciones hash**<br>SHA-256, SHA-3, BLAKE3 | Verificación de integridad muy rápida. Salida de tamaño fijo, no reversible. Sin gestión de claves. | No aportan confidencialidad. Vulnerables a colisiones si el diseño es débil (MD5, SHA-1). Su velocidad las hace malas para contraseñas. | Prueba de trabajo y enlace de bloques en Bitcoin. Verificación de ISO y paquetes. Huellas de certificados y commits de Git. |
| **Derivación de claves**<br>PBKDF2, bcrypt, scrypt, Argon2 | Lentitud deliberada que frena la fuerza bruta. Sal individual por usuario. Argon2 resiste GPU y ASIC. | Consumo alto de CPU y memoria en el servidor. Parámetros mal calibrados anulan la protección. | Almacenamiento de contraseñas en aplicaciones web. Bitwarden y 1Password. Cifrado de billeteras de criptomonedas. |
| **MAC / HMAC**<br>HMAC-SHA256, Poly1305 | Integridad y autenticación con clave compartida. Muy eficiente. Base de la autenticación de APIs. | No ofrece no repudio: ambas partes comparten la clave. Depende de la fortaleza del hash subyacente. | Tokens JWT (HS256). AWS Signature v4. Webhooks de GitHub y Stripe. |
| **Criptografía cuántica (QKD)**<br>BB84, E91 | Seguridad basada en leyes físicas, no en supuestos computacionales. Detecta la presencia del espía. | Costo muy elevado e infraestructura dedicada. Limitada por distancia. No resuelve firma ni autenticación. Requiere canal clásico autenticado. | Red troncal cuántica Pekín–Shanghái. Satélite Micius. Enlaces bancarios experimentales en Suiza y Corea. |
| **Poscuántica (PQC)**<br>ML-KEM, ML-DSA, SLH-DSA | Resiste a Shor y a Grover. Corre en hardware convencional. Estandarizada por el NIST en agosto de 2024. | Claves y firmas de mayor tamaño. Menor historial de criptoanálisis. Obliga a rediseñar protocolos y dispositivos limitados. | TLS híbrido X25519+ML-KEM en Chrome y Cloudflare. iMessage PQ3. Signal PQXDH. OpenSSH sntrup761x25519. |
| **Homomórfica**<br>BFV, CKKS, TFHE | Permite computar sobre datos cifrados sin descifrarlos. Habilita analítica en la nube preservando la privacidad. | Sobrecarga computacional de varios órdenes de magnitud. Alta complejidad de implementación. | Microsoft SEAL y OpenFHE. Analítica médica y financiera sobre datos cifrados. |
| **Umbral y multipartita**<br>MPC, Shamir, MuSig | Reparte el secreto: ningún participante lo conoce completo. Elimina el punto único de fallo. | Protocolos complejos y con alto tráfico entre las partes. Difícil de auditar. | Custodia institucional de criptoactivos. Billeteras multifirma. Firmas Schnorr y MuSig en Bitcoin Taproot. |

---

## 2. Criptografía cuántica vs. criptografía poscuántica

| Criterio | Criptografía cuántica | Criptografía poscuántica |
|---|---|---|
| Fundamento | Leyes de la física (incertidumbre, no clonación) | Problemas matemáticos difíciles (retículos, códigos, hash) |
| Hardware | Fotónico dedicado, fibra o satélite | Computadores convencionales |
| Función | Distribución de claves (QKD) | Intercambio de claves **y** firma digital |
| Madurez | Operativa pero costosa y limitada en distancia | Estandarizada (FIPS 203/204/205, 2024) y en despliegue |
| Detecta al espía | Sí | No |
| Escalabilidad en internet | Baja | Alta |

> **Regla mnemotécnica:** la criptografía cuántica *necesita* un computador cuántico; la poscuántica *se defiende* de un computador cuántico.

---

## 3. Impacto de los algoritmos cuánticos

| Algoritmo clásico | Estado frente al cómputo cuántico | Mitigación |
|---|---|---|
| RSA-2048/4096 | Roto (Shor) | Migrar a ML-KEM / ML-DSA |
| ECDSA / ECDH (P-256, secp256k1) | Roto (Shor) | Migrar a ML-DSA, SLH-DSA |
| Diffie-Hellman | Roto (Shor) | Híbrido X25519 + ML-KEM |
| AES-128 | Debilitado (Grover, ~64 bits efectivos) | Migrar a AES-256 |
| AES-256 | Seguro | Ninguna |
| SHA-256 / SHA-3 | Seguro con margen reducido | Preferir SHA-384/512 |

---

## 4. ¿El blockchain es un tipo de criptografía?

**No.** El blockchain es una **arquitectura de datos distribuida** que *utiliza* criptografía como capa fundacional, junto con la red P2P y el mecanismo de consenso.

Primitivas criptográficas que emplea Bitcoin:

| Primitiva | Algoritmo | Función |
|---|---|---|
| Hash | SHA-256 (doble), RIPEMD-160 | Enlaza bloques, sustenta la PoW, genera direcciones |
| Árbol de Merkle | SHA-256 | Resume transacciones y permite verificación ligera (SPV) |
| Firma digital | ECDSA y Schnorr sobre secp256k1 | Prueba de propiedad y autorización de gasto |
| Derivación de claves | BIP32 / BIP39 / BIP44 | Billeteras jerárquicas deterministas |
| Codificación con verificación | Base58Check, Bech32 | Detección de errores en direcciones |

### Arquitectura por capas

```
CAPA 5  Aplicación      wallets, exploradores, exchanges, dApps, API RPC
CAPA 4  Contratos       Script de Bitcoin, EVM, SVM
CAPA 3  Consenso        PoW / PoS, cadena más pesada, dificultad
CAPA 2  Red P2P         nodos, gossip, mempool, TCP/8333
CAPA 1  Datos           bloques encadenados, Merkle, UTXO, LevelDB
CAPA 0  Criptografía    SHA-256, ECDSA/Schnorr, BIP32, Bech32
```

### Encadenamiento por hash

```
┌──────────────┐      ┌──────────────┐      ┌──────────────┐
│  BLOQUE N-1  │      │   BLOQUE N   │      │  BLOQUE N+1  │
├──────────────┤      ├──────────────┤      ├──────────────┤
│ hash previo  │◄─────┤ hash previo  │◄─────┤ hash previo  │
│ raíz Merkle  │      │ raíz Merkle  │      │ raíz Merkle  │
│ timestamp    │      │ timestamp    │      │ timestamp    │
│ dificultad   │      │ dificultad   │      │ dificultad   │
│ nonce        │      │ nonce        │      │ nonce        │
├──────────────┤      ├──────────────┤      ├──────────────┤
│ transacciones│      │ transacciones│      │ transacciones│
└──────────────┘      └──────────────┘      └──────────────┘
```

Modificar una transacción cambia su hash → cambia la raíz de Merkle → cambia el hash del bloque → rompe todos los enlaces posteriores. Esa es la base de la inmutabilidad práctica.

---

## Referencias

- NIST. *FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), FIPS 205 (SLH-DSA)*, agosto de 2024.
- Shannon, C. E. *Communication Theory of Secrecy Systems*, 1949.
- Diffie, W. y Hellman, M. *New Directions in Cryptography*, 1976.
- Shor, P. W. *Algorithms for Quantum Computation*, 1994.
- Grover, L. K. *A Fast Quantum Mechanical Algorithm for Database Search*, 1996.
- Bennett, C. H. y Brassard, G. *Quantum Cryptography: Public Key Distribution and Coin Tossing*, 1984.
- Nakamoto, S. *Bitcoin: A Peer-to-Peer Electronic Cash System*, 2008.
- Antonopoulos, A. M. *Mastering Bitcoin*, 2.ª ed., O'Reilly, 2017.
- Katz, J. y Lindell, Y. *Introduction to Modern Cryptography*, 3.ª ed., CRC Press, 2020.
