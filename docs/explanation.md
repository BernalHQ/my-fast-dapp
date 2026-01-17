# 🎯 Guía EXPLICADA: Construye tu Primera dApp en Base
## Para principiantes que quieren ENTENDER qué están haciendo

---

## 🤔 ¿Qué vas a construir y por qué?

Vas a crear una **aplicación descentralizada (dApp)** para rastrear ayuno intermitente. Pero no es solo una app normal:

- **Los datos NO están en un servidor** → Están en la blockchain (Base)
- **Nadie puede borrar tu historial** → Es permanente e inmutable
- **No necesitas crear cuenta** → Tu wallet es tu identidad
- **Todo es transparente** → Puedes ver el código que se ejecuta

**Analogía del mundo real:**
Imagina que en lugar de anotar tus ayunos en una libreta (que puedes perder) o en una app de Google (que puede cerrar), los grabas en una **pared pública e indestructible** que todos pueden ver pero solo tú puedes escribir en tu sección.

---

## 📋 Requisitos Previos

### ¿Qué necesitas instalar?

1. **Node.js (v18 o superior)** - [nodejs.org](https://nodejs.org)
   - **¿Qué es?** Un entorno para ejecutar JavaScript fuera del navegador
   - **¿Por qué?** Las herramientas de blockchain (Hardhat, ethers.js) están hechas en JavaScript
   - **Analogía:** Es como instalar Python para ejecutar scripts de Python

2. **MetaMask** - Extensión de navegador
   - **¿Qué es?** Una billetera digital para criptomonedas
   - **¿Por qué?** Necesitas firmar transacciones en la blockchain
   - **Analogía:** Es como tu cuenta bancaria, pero tú tienes control total (nadie más puede acceder)

3. **Editor de código** - VS Code recomendado
   - **¿Por qué?** Para escribir y editar el código cómodamente
   - **Alternativas:** Sublime Text, Atom, cualquier editor de texto

---

## FASE 1: Configuración del Entorno

### 🎯 Objetivo de esta fase
Preparar tu computadora con las herramientas necesarias para desarrollar en blockchain.

### Paso 1.1: Crear el proyecto

```bash
mkdir mi-fasting-dapp
cd mi-fasting-dapp
npm init -y
```

**¿Qué estás haciendo?**
- `mkdir` = Crear una carpeta nueva
- `cd` = Entrar a esa carpeta
- `npm init -y` = Crear un archivo `package.json` (lista de dependencias del proyecto)

**¿Por qué?**
Cada proyecto de Node.js necesita un `package.json` para saber qué librerías usar. El `-y` acepta todas las opciones por defecto (para ir más rápido).

---

### Paso 1.2: Instalar Hardhat y dependencias

```bash
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox dotenv
```

**¿Qué estás haciendo?**
Instalando 3 herramientas esenciales:

1. **hardhat** 
   - **¿Qué es?** Un entorno de desarrollo para smart contracts
   - **¿Por qué?** Te permite compilar, probar y desplegar contratos
   - **Analogía:** Es como tener un "servidor local" pero para blockchain

2. **@nomicfoundation/hardhat-toolbox**
   - **¿Qué es?** Un paquete que incluye muchas herramientas útiles
   - **¿Por qué?** Trae todo lo necesario: ethers.js, herramientas de testing, etc.

3. **dotenv**
   - **¿Qué es?** Una librería para manejar variables de entorno
   - **¿Por qué?** Para guardar tu clave privada de forma SEGURA (sin subirla a GitHub)

**El flag `--save-dev`**
Significa que estas herramientas solo se usan para desarrollo, no en producción.

---

### Paso 1.3: Inicializar Hardhat

```bash
npx hardhat init
```

**¿Qué es `npx`?**
Es un comando que ejecuta paquetes de npm sin instalarlos globalmente.

**¿Qué hace este comando?**
Crea la estructura básica de carpetas:
```
mi-fasting-dapp/
├── contracts/     ← Aquí van tus smart contracts
├── scripts/       ← Scripts para desplegar
├── test/          ← Tests automatizados
└── hardhat.config.js  ← Configuración
```

**Selecciona: "Create a JavaScript project"**
Porque es más fácil para principiantes (vs TypeScript).

---

### Paso 1.4: Crear archivo de configuración

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: {
    version: "0.8.20",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
  networks: {
    baseSepolia: {
      url: "https://sepolia.base.org",
      chainId: 84532,
      accounts: process.env.PRIVATE_KEY ? [process.env.PRIVATE_KEY] : []
    }
  }
};
```

**Desglosando cada parte:**

#### `solidity: { version: "0.8.20" }`
- **¿Qué es Solidity?** El lenguaje de programación para smart contracts
- **¿Por qué 0.8.20?** Es una versión estable y moderna
- **Analogía:** Como especificar Python 3.10 vs Python 2.7

#### `optimizer: { enabled: true, runs: 200 }`
- **¿Qué hace?** Optimiza el código para usar menos gas (gasolina de blockchain)
- **¿Por qué?** Cada operación en blockchain cuesta dinero (gas)
- **`runs: 200`** significa "optimiza asumiendo que se ejecutará ~200 veces"
- **Analogía:** Como comprimir un archivo ZIP - ocupa menos espacio pero tarda más en crearse

#### `networks: { baseSepolia: {...} }`
- **¿Qué es Base Sepolia?** Una red de prueba (testnet) de Base
- **¿Por qué testnet?** Para probar sin gastar dinero real
- **`chainId: 84532`** es el identificador único de Base Sepolia
- **`accounts:`** aquí pondrás tu clave privada (desde .env)

---

### Paso 1.5: Crear archivo .env

```bash
touch .env
```

**Contenido del archivo:**
```
PRIVATE_KEY=tu_clave_privada_sin_0x
```

**⚠️ SÚPER IMPORTANTE - LEE ESTO:**

**¿Qué es la clave privada?**
Es como la contraseña de tu billetera. Con ella, cualquiera puede gastar tu dinero.

**¿Dónde la consigo?**
1. Abre MetaMask
2. Click en los 3 puntos → Detalles de cuenta
3. "Exportar clave privada"
4. Ingresa tu contraseña de MetaMask
5. Copia la clave (sin el `0x` del inicio)

**¿Por qué en .env?**
Para NO subirla accidentalmente a GitHub.

**¡NUNCA COMPARTAS TU CLAVE PRIVADA!**
- No la pegues en Discord/Telegram
- No la subas a GitHub
- No la envíes por email

**Agrégala a .gitignore:**
Crea o edita el archivo `.gitignore` y agrega:
```
.env
node_modules/
```

---

## FASE 2: Smart Contract (El Corazón de la dApp)

### 🎯 Objetivo de esta fase
Escribir el código que vivirá en la blockchain y manejará toda la lógica de tu aplicación.

---

### Paso 2.1: Entender qué es un Smart Contract

**Definición simple:**
Un smart contract es un programa que vive en la blockchain.

**Características clave:**

1. **Inmutable** 
   - Una vez desplegado, NO puedes cambiarlo
   - **¿Por qué?** Para que los usuarios confíen en que las reglas no cambiarán
   - **Analogía:** Como grabar algo en piedra vs escribir en lápiz

2. **Público**
   - Todo el código es visible
   - Todas las transacciones son públicas
   - **¿Por qué?** Transparencia total
   - **Analogía:** Como un libro de contabilidad que todos pueden auditar

3. **Sin intermediarios**
   - Se ejecuta automáticamente
   - Nadie puede detenerlo
   - **¿Por qué?** Descentralización
   - **Analogía:** Como una máquina expendedora (pones dinero → recibes producto, sin cajero)

4. **Cuesta dinero ejecutarlo**
   - Cada operación consume "gas"
   - **¿Por qué?** Para pagar a los validadores que procesan las transacciones
   - **Analogía:** Como pagar luz por usar la computadora

---

### Paso 2.2: Crear el contrato básico

Crea `contracts/FastingTracker.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;

contract FastingTracker {
    // ... (el código viene después)
}
```

**Desglosando la estructura:**

#### `// SPDX-License-Identifier: MIT`
- **¿Qué es?** Especifica la licencia del código
- **¿Por qué?** Porque el código en blockchain es público, necesitas especificar cómo otros pueden usarlo
- **MIT = Open Source:** Cualquiera puede usar/modificar tu código

#### `pragma solidity 0.8.20;`
- **¿Qué es `pragma`?** Una instrucción para el compilador
- **¿Por qué?** Especifica qué versión de Solidity usar
- **Analogía:** Como `<!DOCTYPE html>` en HTML

#### `contract FastingTracker { }`
- **¿Qué es?** Como una `class` en JavaScript/Python
- **Dentro van:** Variables, funciones, eventos
- **Analogía:** Como un objeto que tiene propiedades y métodos

---

### 📊 Estructuras de Datos

```solidity
struct Fast {
    uint48 startTime;      // Cuándo empezó
    uint48 endTime;        // Cuándo terminó
    uint16 durationHours;  // Duración en horas
    bool completed;        // ¿Llegó a 24h?
    string note;           // Nota del usuario
}
```

**¿Qué es un `struct`?**
Una estructura que agrupa varios datos relacionados.
**Analogía:** Como un objeto en JavaScript `{ startTime: ..., endTime: ... }`

**¿Por qué usar `uint48` en lugar de `uint256`?**

Los números en Solidity pueden ser de diferentes tamaños:
- `uint8` → 0 a 255
- `uint16` → 0 a 65,535
- `uint48` → 0 a 281 trillones (suficiente para timestamps hasta el año 8921485)
- `uint256` → Un número gigantesco (el predeterminado)

**Ventaja de usar tipos más pequeños:**
¡AHORRAS GAS! En blockchain, el espacio cuesta dinero.

**¿Por qué `uint48` es suficiente para timestamps?**
Los timestamps de Unix cuentan segundos desde 1970. Actualmente estamos en ~1.7 mil millones. Con uint48 tienes espacio para 281 billones de segundos (miles de años).

---

```solidity
struct UserState {
    uint48 currentFastStart;  // 0 si no está ayunando
    uint48 lastFastEnd;       // Último ayuno terminado
    uint32 totalFasts;        // Total de ayunos
    uint32 completedFasts;    // Ayunos de 24h+
}
```

**¿Para qué sirve?**
Guardar el "estado actual" del usuario sin tener que recorrer todo su historial.

**Optimización importante:**
En lugar de leer 100 ayunos para contar cuántos completó, guardamos el contador directamente.

---

### 🗄️ Almacenamiento

```solidity
mapping(address => Fast[]) private _userFasts;
mapping(address => UserState) private _userState;
```

**¿Qué es un `mapping`?**
Es como un diccionario o HashMap: `clave → valor`

**Ejemplo:**
```
_userFasts[0x123...abc] = [ayuno1, ayuno2, ayuno3]
_userFasts[0x456...def] = [ayuno1]
```

**¿Qué es `address`?**
El tipo de dato para direcciones de Ethereum (0x1234...abcd).

**¿Qué es `Fast[]`?**
Un array (lista) de ayunos.

**¿Por qué `private`?**
Solo el contrato puede leer estas variables directamente. Los usuarios deben usar funciones `view` para leerlas.

**¿Por qué el `_` al inicio?**
Es una convención: las variables privadas llevan `_` para distinguirlas.

---

### 📌 Constantes

```solidity
uint256 public constant WEEK_SECONDS = 7 days;
uint256 public constant TARGET_HOURS = 24;
uint256 public constant MIN_FAST_HOURS = 1;
```

**¿Qué es `constant`?**
Un valor que NUNCA cambia. Se define al compilar.

**Ventaja:**
¡Más barato en gas! El compilador reemplaza la variable por su valor directamente.

**¿Qué es `7 days`?**
Solidity tiene unidades de tiempo built-in:
- `1 seconds` = 1
- `1 minutes` = 60
- `1 hours` = 3600
- `1 days` = 86400
- `1 weeks` = 604800

---

### 📡 Eventos

```solidity
event FastStarted(address indexed user, uint256 startTime);
event FastEnded(address indexed user, uint256 duration, bool completed);
```

**¿Qué es un evento?**
Es como un "log" que la blockchain guarda y que el frontend puede escuchar.

**¿Para qué sirven?**
1. Notificar al frontend cuando algo sucede
2. Crear un historial auditable
3. Son MÁS BARATOS que guardar en `storage`

**¿Qué es `indexed`?**
Permite filtrar eventos por ese campo.

**Ejemplo:**
```javascript
// En el frontend puedes hacer:
contract.on("FastStarted", (user, startTime) => {
  if (user === myAddress) {
    console.log("¡Iniciaste un ayuno!");
  }
});
```

---

### ⚠️ Errores Personalizados

```solidity
error AlreadyFasting();
error NotFasting();
error WeeklyLimitReached(uint256 nextAvailableTime);
error FastTooShort(uint256 minimumHours);
```

**¿Por qué usar errores personalizados?**
Son MÁS BARATOS en gas que `require("mensaje")`.

**Antes (costoso):**
```solidity
require(!isFasting, "Ya estas ayunando");  // Guarda el string
```

**Ahora (barato):**
```solidity
if (isFasting) revert AlreadyFasting();  // Solo guarda un ID
```

**Ventaja adicional:**
Puedes pasar parámetros para dar más contexto.

---

### Paso 2.3: Función para iniciar ayuno

```solidity
function startFast() external {
    UserState storage state = _userState[msg.sender];
    
    // Verificar que no está ya ayunando
    if (state.currentFastStart != 0) {
        revert AlreadyFasting();
    }
    
    // Verificar límite semanal (1 ayuno por semana)
    if (state.lastFastEnd != 0) {
        uint256 nextAvailable = state.lastFastEnd + WEEK_SECONDS;
        if (block.timestamp < nextAvailable) {
            revert WeeklyLimitReached(nextAvailable);
        }
    }
    
    // Iniciar el ayuno
    state.currentFastStart = uint48(block.timestamp);
    
    emit FastStarted(msg.sender, block.timestamp);
}
```

**Desglosando línea por línea:**

#### `function startFast() external`
- **`external`** = Solo se puede llamar desde FUERA del contrato
- Alternativas: `public` (dentro y fuera), `internal` (solo dentro), `private` (solo en este contrato)
- **¿Por qué `external`?** Es más barato en gas que `public`

#### `UserState storage state = _userState[msg.sender];`

**¿Qué es `msg.sender`?**
La dirección de quien llamó esta función (tu wallet).

**¿Qué es `storage`?**
Significa que `state` es una REFERENCIA a los datos en blockchain, no una copia.

**Importante:**
```solidity
storage // Modificas el original
memory  // Trabajas con una copia
calldata // Solo lectura (para parámetros)
```

**Analogía:**
```javascript
// JavaScript
const state = userState[msg.sender];  // Copia
state.totalFasts++;  // Modifica la copia, NO el original

// Solidity con storage
UserState storage state = _userState[msg.sender];  // Referencia
state.totalFasts++;  // Modifica el original ✓
```

#### `if (state.currentFastStart != 0) { revert AlreadyFasting(); }`

**¿Por qué comparar con 0?**
Porque el valor predeterminado de `uint` es 0.
- `0` = No está ayunando
- `> 0` = Timestamp de cuándo empezó

**¿Qué hace `revert`?**
1. Cancela la transacción
2. Revierte TODOS los cambios
3. Devuelve el gas no usado
4. Lanza el error

#### `uint256 nextAvailable = state.lastFastEnd + WEEK_SECONDS;`

**Lógica:**
Si tu último ayuno terminó el 1 de enero a las 12:00, no puedes empezar otro hasta el 8 de enero a las 12:00.

#### `block.timestamp`

**¿Qué es?**
El timestamp de Unix del bloque actual (segundos desde 1970).

**¿De dónde viene?**
Los validadores de la red lo establecen al crear el bloque.

**⚠️ Nota de seguridad:**
Los validadores pueden manipularlo ~30 segundos. Para tiempos críticos (como subastas), usa `block.number` en su lugar.

#### `emit FastStarted(msg.sender, block.timestamp);`

Emite el evento para que el frontend pueda reaccionar.

---

### Paso 2.4: Función para terminar ayuno

```solidity
function endFast(string calldata note) external {
    UserState storage state = _userState[msg.sender];
    
    if (state.currentFastStart == 0) {
        revert NotFasting();
    }
    
    uint256 duration = block.timestamp - state.currentFastStart;
    uint256 durationHours = duration / 1 hours;
    
    if (durationHours < MIN_FAST_HOURS) {
        revert FastTooShort(MIN_FAST_HOURS);
    }
    
    bool completed = durationHours >= TARGET_HOURS;
    
    _userFasts[msg.sender].push(Fast({
        startTime: state.currentFastStart,
        endTime: uint48(block.timestamp),
        durationHours: uint16(durationHours),
        completed: completed,
        note: note
    }));
    
    state.lastFastEnd = uint48(block.timestamp);
    state.currentFastStart = 0;
    state.totalFasts++;
    if (completed) {
        state.completedFasts++;
    }
    
    emit FastEnded(msg.sender, durationHours, completed);
}
```

**Conceptos nuevos:**

#### `string calldata note`

**¿Qué es `calldata`?**
Un área de solo lectura donde se guardan los parámetros de la función.

**¿Por qué `calldata` en lugar de `memory`?**
¡Es más barato! Como solo leemos el string (no lo modificamos), `calldata` es perfecto.

**Comparación de costos:**
```solidity
string memory note   // ~3000 gas
string calldata note // ~100 gas
```

#### División de enteros

```solidity
uint256 durationHours = duration / 1 hours;
```

**¿Cómo funciona?**
- `duration` = 90000 segundos (25 horas)
- `1 hours` = 3600 segundos
- `90000 / 3600` = 25 (Solidity solo usa enteros, no decimales)

**⚠️ Importante:**
Solidity NO tiene decimales. `7 / 2 = 3` (no 3.5).

#### `.push()` en arrays

```solidity
_userFasts[msg.sender].push(Fast({...}));
```

**¿Qué hace?**
Agrega un nuevo elemento al final del array.

**Analogía JavaScript:**
```javascript
array.push(nuevoElemento);
```

#### Sintaxis de struct

```solidity
Fast({
    startTime: state.currentFastStart,
    endTime: uint48(block.timestamp),
    // ...
})
```

Esto es como crear un objeto en JavaScript con propiedades nombradas.

---

### Paso 2.5: Funciones de lectura (`view`)

```solidity
function getCurrentFastStatus(address user) external view returns (
    bool isActive,
    uint256 startTime,
    uint256 elapsedHours
) {
    UserState storage state = _userState[user];
    isActive = state.currentFastStart != 0;
    startTime = state.currentFastStart;
    
    if (isActive) {
        elapsedHours = (block.timestamp - startTime) / 1 hours;
    }
}
```

**¿Qué es `view`?**
Significa "esta función solo LEE datos, no los modifica".

**Ventajas:**
1. **NO cuesta gas** cuando se llama desde el frontend
2. Puede ser ejecutada localmente (no necesita transacción)

**¿Cuándo usar `view`?**
- Para obtener datos
- Para cálculos que no cambian el estado

**¿Cuándo NO usar `view`?**
- Si modificas variables de estado
- Si emites eventos

#### Retornos múltiples

```solidity
returns (bool isActive, uint256 startTime, uint256 elapsedHours)
```

Solidity puede retornar múltiples valores nombrados.

**En el frontend:**
```javascript
const [isActive, startTime, elapsedHours] = 
    await contract.getCurrentFastStatus(userAddress);
```

---

```solidity
function getFastHistory(address user, uint256 offset, uint256 limit) 
    external view returns (Fast[] memory) 
{
    Fast[] storage fasts = _userFasts[user];
    uint256 total = fasts.length;
    
    if (offset >= total) {
        return new Fast[](0);  // Array vacío
    }
    
    uint256 remaining = total - offset;
    uint256 count = remaining < limit ? remaining : limit;
    
    Fast[] memory result = new Fast[](count);
    for (uint256 i = 0; i < count; i++) {
        // Devolver en orden inverso (más reciente primero)
        result[i] = fasts[total - 1 - offset - i];
    }
    
    return result;
}
```

**¿Qué es la paginación?**
En lugar de devolver TODO el historial (costoso), devuelves solo una "página".

**Parámetros:**
- `offset` = Desde dónde empezar
- `limit` = Cuántos devolver

**Ejemplo:**
- Total de ayunos: 100
- `offset=0, limit=10` → Devuelve ayunos 90-100 (los 10 más recientes)
- `offset=10, limit=10` → Devuelve ayunos 80-90

**¿Por qué en orden inverso?**
Para que los más recientes aparezcan primero.

**`new Fast[](count)`**
Crea un array nuevo en memoria con tamaño `count`.

---

### Paso 2.6: Compilar

```bash
npx hardhat compile
```

**¿Qué hace?**
1. Lee tu código Solidity
2. Lo convierte a bytecode (código de máquina para la EVM)
3. Genera el ABI (interfaz para que el frontend se comunique)

**¿Qué es el ABI?**
Application Binary Interface - Un JSON que describe las funciones del contrato.

**Analogía:**
Como un "manual de instrucciones" que le dice al frontend:
- Qué funciones existen
- Qué parámetros aceptan
- Qué retornan

**Archivos generados:**
```
artifacts/
└── contracts/
    └── FastingTracker.sol/
        ├── FastingTracker.json  ← ABI + bytecode aquí
        └── FastingTracker.dbg.json
```

---

## FASE 3: Tests (Asegurar que funciona)

### 🎯 Objetivo
Verificar que tu contrato se comporta como esperas ANTES de desplegarlo (porque después no puedes cambiarlo).

```javascript
const { expect } = require("chai");
const { ethers } = require("hardhat");
const { time } = require("@nomicfoundation/hardhat-network-helpers");

describe("FastingTracker", function () {
  let tracker;
  let owner;
  
  beforeEach(async function () {
    [owner] = await ethers.getSigners();
    const FastingTracker = await ethers.getContractFactory("FastingTracker");
    tracker = await FastingTracker.deploy();
  });
  
  // ... tests
});
```

**Desglosando:**

#### `const { expect } = require("chai");`
**Chai** es una librería de aserciones (para verificar resultados).

**Ejemplo:**
```javascript
expect(2 + 2).to.equal(4);  // ✓ Pasa
expect(2 + 2).to.equal(5);  // ✗ Falla
```

#### `beforeEach(async function () { ... });`
**¿Qué hace?**
Se ejecuta ANTES de cada test, creando un entorno limpio.

**¿Por qué?**
Para que cada test sea independiente (un test no debe afectar a otro).

#### `await ethers.getSigners()`
**¿Qué devuelve?**
Una lista de wallets de prueba con ETH fake.

**Hardhat te da 20 wallets automáticamente:**
- `accounts[0]` → Owner (tú)
- `accounts[1]` → Usuario de prueba #1
- `accounts[2]` → Usuario de prueba #2
- etc.

#### `await ethers.getContractFactory("FastingTracker")`
Obtiene el "molde" para crear instancias del contrato.

#### `await FastingTracker.deploy()`
Despliega una NUEVA instancia del contrato en la blockchain de prueba.

---

### Test de ejemplo

```javascript
it("Debería iniciar un ayuno", async function () {
  await tracker.startFast();
  
  const status = await tracker.getCurrentFastStatus(owner.address);
  expect(status.isActive).to.be.true;
});
```

**¿Qué verifica?**
1. Llama a `startFast()`
2. Lee el estado
3. Verifica que `isActive` sea `true`

---

```javascript
it("No debería permitir dos ayunos simultáneos", async function () {
  await tracker.startFast();
  
  await expect(tracker.startFast())
    .to.be.revertedWithCustomError(tracker, "AlreadyFasting");
});
```

**¿Qué verifica?**
Que llamar `startFast()` dos veces lance el error `AlreadyFasting`.

---

### Manipular el tiempo en tests

```javascript
await time.increase(25 * 60 * 60);  // Avanzar 25 horas
```

**¿Cómo funciona?**
Hardhat tiene una blockchain local donde TÚ controlas el tiempo.

**¿Por qué es útil?**
Puedes probar escenarios de "¿qué pasa en 1 semana?" sin esperar 1 semana.

---

### Ejecutar tests

```bash
npx hardhat test
```

**¿Qué hace?**
1. Levanta una blockchain local
2. Despliega el contrato
3. Ejecuta cada test
4. Muestra resultados:
```
  FastingTracker
    startFast
      ✓ Debería iniciar un ayuno
      ✓ No debería permitir dos ayunos simultáneos
    endFast
      ✓ Debería marcar como completado si pasan 24+ horas

  3 passing (2s)
```

---

## FASE 4: Despliegue a Base Sepolia

### 🎯 Objetivo
Subir tu contrato a una blockchain REAL (aunque de prueba).

### Paso 4.1: Obtener ETH de prueba

**¿Por qué necesitas ETH?**
Porque desplegar el contrato es una transacción, y las transacciones cuestan gas.

**Base Sepolia = Red de prueba**
- El ETH es falso (no tiene valor)
- Puedes obtenerlo gratis en faucets
- Es idéntica a la mainnet (pero sin riesgo)

**Opciones de faucets:**
1. [Alchemy Base Sepolia Faucet](https://sepoliafaucet.com/)
2. [Coinbase Wallet Faucet](https://www.coinbase.com/faucets/base-ethereum-goerli-faucet)

**Proceso:**
1. Conecta tu wallet MetaMask
2. Resuelve el CAPTCHA
3. Click "Send Me ETH"
4. Espera 1-2 minutos

---

### Paso 4.2: Script de despliegue

```javascript
const hre = require("hardhat");

async function main() {
  console.log("Desplegando FastingTracker...");
  
  const FastingTracker = await hre.ethers.getContractFactory("FastingTracker");
  const tracker = await FastingTracker.deploy();
  
  await tracker.waitForDeployment();
  
  const address = await tracker.getAddress();
  console.log("✅ FastingTracker desplegado en:", address);
}

main().catch((error) => {
  console.error(error);
  process.exit(1);
});
```

**¿Qué hace `waitForDeployment()`?**
Espera a que la transacción de despliegue sea confirmada en la blockchain.

**¿Cuánto tarda?**
En Base Sepolia: ~2 segundos.
En mainnet: ~30 segundos - 2 minutos.

---

### Paso 4.3: Desplegar

```bash
npx hardhat run scripts/deploy.js --network baseSepolia
```

**¿Qué sucede internamente?**

1. **Hardhat lee tu configuración:**
   ```javascript
   networks: {
     baseSepolia: {
       url: "https://sepolia.base.org",
       accounts: [process.env.PRIVATE_KEY]
     }
   }
   ```

2. **Crea la transacción de despliegue:**
   - Incluye el bytecode del contrato
   - Firma con tu clave privada
   - Estima el gas necesario

3. **Envía la transacción:**
   - A través del RPC endpoint de Base Sepolia
   - Los validadores la procesan

4. **Devuelve la dirección del contrato:**
   ```
   ✅ FastingTracker desplegado en: 0x1234567890abcdef...
   ```

**⚠️ GUARDA ESTA DIRECCIÓN**
La necesitarás para conectar el frontend.

**¿Dónde está tu contrato ahora?**
En la blockchain de Base Sepolia, en esa dirección específica.

**Puedes verlo en el explorador:**
https://sepolia.basescan.org/address/TU_DIRECCION

---

## FASE 5: Frontend (La Cara Visible)

### 🎯 Objetivo
Crear una interfaz web para que los usuarios interactúen con tu contrato.

### Arquitectura Frontend ↔ Blockchain

```
┌─────────────────────────────┐
│    Tu Frontend (HTML/JS)    │
│                              │
│  - Muestra datos             │
│  - Captura eventos de usuario│
│  - Llama funciones           │
└──────────┬──────────────────┘
           │
           │ ethers.js
           ▼
┌─────────────────────────────┐
│       MetaMask              │
│                              │
│  - Administra clave privada  │
│  - Firma transacciones       │
│  - Se conecta a la red       │
└──────────┬──────────────────┘
           │
           │ RPC (JSON-RPC)
           ▼
┌─────────────────────────────┐
│    Base Sepolia Network     │
│                              │
│  - Procesa transacciones     │
│  - Ejecuta contratos         │
│  - Mantiene estado           │
└──────────┬──────────────────┘
           │
           ▼
    ┌──────────────┐
    │ Tu Contrato  │
    │ en 0x123...  │
    └──────────────┘
```

---

### HTML básico

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Fasting Tracker</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/ethers/6.7.0/ethers.min.js"></script>
</head>
```

**¿Qué es ethers.js?**
Una librería de JavaScript para interactuar con Ethereum y blockchains compatibles.

**Funciones principales:**
- Conectar con wallets (MetaMask)
- Leer datos del contrato
- Enviar transacciones
- Formatear datos

**Alternativa:** Web3.js (más antigua, más compleja)

---

### Conectar con MetaMask

```javascript
async function connectWallet() {
    if (!window.ethereum) {
        alert("Instala MetaMask!");
        return;
    }
    
    try {
        // Solicitar conexión
        provider = new ethers.BrowserProvider(window.ethereum);
        signer = await provider.getSigner();
        userAddress = await signer.getAddress();
        
        // ... resto del código
    } catch (error) {
        console.error(error);
    }
}
```

**Desglosando:**

#### `window.ethereum`
**¿Qué es?**
Un objeto que MetaMask inyecta en el navegador.

**¿Para qué?**
Para que las dApps puedan comunicarse con la wallet.

**Si no existe:**
El usuario no tiene MetaMask instalado.

#### `new ethers.BrowserProvider(window.ethereum)`
Crea una instancia de provider que puede comunicarse con la blockchain a través de MetaMask.

**¿Qué es un provider?**
Una conexión a la blockchain (solo lectura).

#### `await provider.getSigner()`
Obtiene el "signer" (firmante) - esto puede ENVIAR transacciones.

**Diferencia:**
- **Provider:** Solo lectura (gratis)
- **Signer:** Puede escribir (cuesta gas)

#### `await signer.getAddress()`
Obtiene la dirección pública del usuario.

**MetaMask pregunta al usuario:**
"¿Permitir que este sitio vea tu dirección?"

---

### Verificar la red correcta

```javascript
const network = await provider.getNetwork();
if (network.chainId !== 84532n) {
    alert("Cambia a Base Sepolia en MetaMask");
    return;
}
```

**¿Por qué verificar?**
Tu contrato está en Base Sepolia (chainId 84532). Si el usuario está en otra red (ej. Ethereum mainnet), las transacciones fallarán.

**El sufijo `n`:**
Indica que es un BigInt (número muy grande). Los chainIds son BigInts en ethers v6.

---

### Crear instancia del contrato

```javascript
const CONTRACT_ADDRESS = "0xTU_DIRECCION_AQUI";

const ABI = [
    "function startFast() external",
    "function endFast(string note) external",
    "function getCurrentFastStatus(address user) view returns (bool, uint256, uint256)",
    "function getUserStats(address user) view returns (uint256, uint256, uint256)"
];

contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, signer);
```

**¿Qué está pasando?**

1. **Defines la dirección:** Dónde vive tu contrato
2. **Defines el ABI:** Qué funciones tiene
3. **Pasas el signer:** Para poder enviar transacciones

**ABI simplificada:**
No necesitas poner TODO el ABI, solo las funciones que usarás.

**Sintaxis de "Human-Readable ABI":**
ethers.js permite escribir funciones como strings legibles en lugar de JSON complejo.

---

### Llamar a funciones del contrato

#### Funciones que modifican estado (cuestan gas):

```javascript
async function startFast() {
    try {
        const tx = await contract.startFast();
        await tx.wait();  // Esperar confirmación
        
        alert("¡Ayuno iniciado!");
    } catch (error) {
        alert("Error: " + error.message);
    }
}
```

**Proceso paso a paso:**

1. **`await contract.startFast()`**
   - ethers.js crea la transacción
   - MetaMask muestra popup pidiendo aprobación
   - Usuario confirma
   - Transacción se envía a la blockchain
   - Retorna un objeto `tx`

2. **`await tx.wait()`**
   - Espera a que la transacción sea incluida en un bloque
   - Retorna el recibo de la transacción

**¿Por qué esperar?**
Porque las transacciones no son instantáneas. Toman ~2 segundos en ser confirmadas.

**Sin `wait()`:**
```javascript
await contract.startFast();
// La transacción está pendiente, los datos aún no cambiaron
const status = await contract.getCurrentFastStatus(userAddress);
// ❌ Todavía muestra el estado VIEJO
```

**Con `wait()`:**
```javascript
const tx = await contract.startFast();
await tx.wait();
// ✓ Ahora la blockchain ya actualizó el estado
const status = await contract.getCurrentFastStatus(userAddress);
// ✓ Muestra el estado NUEVO
```

---

#### Funciones de solo lectura (gratis):

```javascript
async function loadUserData() {
    const status = await contract.getCurrentFastStatus(userAddress);
    
    if (status.isActive) {
        // El usuario está ayunando
        fastStartTime = Number(status.startTime);
        startLocalTimer();
    }
    
    const stats = await contract.getUserStats(userAddress);
    document.getElementById("totalFasts").textContent = stats.totalFasts;
}
```

**¿Por qué no cuesta gas?**
Porque las funciones `view` solo LEEN. El navegador ejecuta la función localmente (no necesita enviar transacción).

**`Number(status.startTime)`:**
Convierte BigInt a número normal de JavaScript.

---

### Timer local (optimización UX)

```javascript
function startLocalTimer() {
    timerInterval = setInterval(updateTimer, 1000);
}

function updateTimer() {
    const elapsed = Math.floor(Date.now() / 1000) - fastStartTime;
    const hours = Math.floor(elapsed / 3600);
    const minutes = Math.floor((elapsed % 3600) / 60);
    const seconds = elapsed % 60;
    
    document.getElementById("timer").textContent = 
        `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
}
```

**¿Por qué hacer esto?**
En lugar de leer la blockchain cada segundo (imposible), calculamos el tiempo transcurrido localmente.

**¿Es confiable?**
Sí, porque:
1. Guardamos `fastStartTime` de la blockchain
2. Calculamos la diferencia con el tiempo actual
3. El tiempo de la blockchain no cambia, así que el cálculo es exacto

---

## 🔄 Flujo completo de una transacción

Cuando el usuario hace clic en "Iniciar Ayuno":

```
1. Click en botón
   └─> Llama a startFast()

2. Frontend (ethers.js)
   └─> Crea transacción
   └─> Estima gas
   └─> Envía a MetaMask

3. MetaMask
   └─> Muestra popup al usuario
   └─> Usuario confirma
   └─> Firma con clave privada
   └─> Envía a Base Sepolia

4. Base Sepolia
   └─> Transacción llega al mempool
   └─> Validador la incluye en un bloque
   └─> Se ejecuta startFast() en el contrato
   └─> Emite evento FastStarted

5. Blockchain actualizada
   └─> currentFastStart = ahora
   └─> Evento grabado

6. Frontend
   └─> tx.wait() detecta confirmación
   └─> Actualiza UI
   └─> Inicia timer local
```

**Tiempo total:** ~2-5 segundos en Base Sepolia.

---

## 💰 Entendiendo el Gas

### ¿Qué es el gas?

**Definición:**
Una unidad que mide el costo computacional de una operación.

**Analogía:**
Como la gasolina que consume tu auto:
- Operaciones simples = Poco gas
- Operaciones complejas = Mucho gas

### Costos típicos

```solidity
// Operaciones baratas
uint256 x = 5;                    // ~20 gas
x + 1;                             // ~3 gas
if (x > 0) { ... }                 // ~3 gas

// Operaciones caras
guardarEnStorage = 123;            // ~20,000 gas
nuevoMapping[user] = data;         // ~20,000 gas
crearNuevoContrato();              // ~200,000+ gas

// Eventos (baratos)
emit MiEvento(data);               // ~400 gas
```

### Precio del gas

**Gas Price** = Cuánto pagas por unidad de gas

En Base Sepolia: ~0.001 Gwei
En Base Mainnet: Variable (0.01-1 Gwei típicamente)

**Costo total de transacción:**
```
Gas Used × Gas Price = Costo en ETH
```

**Ejemplo:**
```
startFast() usa 50,000 gas
Gas Price = 0.001 Gwei
Costo = 50,000 × 0.001 Gwei = 0.05 Gwei = 0.00000005 ETH
```

En mainnet a $3000/ETH: ~$0.00015 USD

---

## 🧠 Conceptos importantes de Seguridad

### 1. Reentrancy

**Problema:**
Un contrato malicioso podría llamar a tu función repetidamente antes de que termine.

**Solución:**
Patrón "Checks-Effects-Interactions"

```solidity
function endFast() external {
    // 1. CHECKS: Verificar condiciones
    if (state.currentFastStart == 0) revert();
    
    // 2. EFFECTS: Modificar estado
    state.currentFastStart = 0;
    
    // 3. INTERACTIONS: Llamadas externas (si las hay)
    // externalContract.notify();
}
```

### 2. Integer Overflow (resuelto en 0.8.x)

**Antes de Solidity 0.8:**
```solidity
uint8 x = 255;
x = x + 1;  // Daba 0 (overflow)
```

**Desde 0.8:**
```solidity
uint8 x = 255;
x = x + 1;  // ❌ Revierte con error
```

### 3. Front-running

**Problema:**
Los validadores ven tu transacción ANTES de confirmarla y pueden insertar la suya primero.

**Ejemplo:**
1. Ves que un NFT está en venta por 1 ETH
2. Envías transacción para comprarlo
3. Un bot ve tu transacción
4. El bot envía la misma transacción con más gas
5. El bot compra primero

**Mitigación:**
- Usar commit-reveal schemes
- Añadir timeouts
- Flashbots (en mainnet)

---

## 📚 Recursos Adicionales

### Documentación oficial
- [Solidity Docs](https://docs.soliditylang.org)
- [Hardhat Docs](https://hardhat.org/docs)
- [ethers.js Docs](https://docs.ethers.org)
- [Base Docs](https://docs.base.org)

### Tutoriales
- [CryptoZombies](https://cryptozombies.io) - Aprende Solidity jugando
- [Solidity by Example](https://solidity-by-example.org)
- [Ethereum.org Developers](https://ethereum.org/developers)

### Herramientas
- [Remix IDE](https://remix.ethereum.org) - Editor online
- [OpenZeppelin Contracts](https://openzeppelin.com/contracts) - Contratos seguros pre-hechos
- [Tenderly](https://tenderly.co) - Debugging de transacciones

### Comunidad
- [r/ethdev](https://reddit.com/r/ethdev)
- [Ethereum Stack Exchange](https://ethereum.stackexchange.com)
- [Base Discord](https://discord.gg/buildonbase)

---

## 🎯 Ejercicios Propuestos (con explicaciones)

### 1. Función `cancelFast()`

**Objetivo:** Permitir cancelar un ayuno en progreso sin registrarlo.

**Pistas:**
```solidity
function cancelFast() external {
    // 1. Verificar que está ayunando
    // 2. Resetear currentFastStart a 0
    // 3. NO incrementar contadores
    // 4. Emitir evento FastCancelled
}
```

**¿Por qué es útil?**
Si el usuario empieza un ayuno por error, puede cancelarlo sin que cuente en sus estadísticas.

---

### 2. Mostrar historial en el frontend

**Objetivo:** Cargar y mostrar los últimos 5 ayunos.

**Pistas JavaScript:**
```javascript
async function loadHistory() {
    const history = await contract.getFastHistory(userAddress, 0, 5);
    
    const listHTML = history.map(fast => `
        <li>
            ${fast.durationHours} horas
            ${fast.completed ? '✅' : '❌'}
            ${fast.note}
        </li>
    `).join('');
    
    document.getElementById("history").innerHTML = listHTML;
}
```

**Conceptos a aprender:**
- Trabajar con arrays en ethers.js
- Formatear fechas desde timestamps
- Manipular el DOM

---

### 3. Verificar el contrato en BaseScan

**Objetivo:** Hacer el código fuente público en el explorador.

**¿Por qué?**
Para que otros puedan:
- Leer el código
- Verificar que hace lo que dice
- Interactuar directamente desde BaseScan

**Comando:**
```bash
npx hardhat verify --network baseSepolia DIRECCION_DEL_CONTRATO
```

---

### 4. Barra de progreso hacia las 24h

**Objetivo:** Mostrar visualmente cuánto falta para completar.

**HTML:**
```html
<div class="progress-bar">
    <div class="progress-fill" id="progressBar" style="width: 0%"></div>
</div>
```

**JavaScript:**
```javascript
function updateProgress() {
    const elapsed = (Date.now() / 1000) - fastStartTime;
    const hours = elapsed / 3600;
    const percentage = Math.min((hours / 24) * 100, 100);
    
    document.getElementById("progressBar").style.width = percentage + "%";
}
```

**CSS:**
```css
.progress-bar {
    width: 100%;
    height: 20px;
    background: #333;
    border-radius: 10px;
}

.progress-fill {
    height: 100%;
    background: linear-gradient(90deg, #00d26a, #00ff88);
    border-radius: 10px;
    transition: width 0.5s;
}
```

---

## ✅ Checklist Final

Antes de considerar tu dApp completa, verifica:

- [ ] El contrato compila sin errores
- [ ] Todos los tests pasan
- [ ] El contrato está desplegado en Base Sepolia
- [ ] El frontend se conecta correctamente
- [ ] Puedes iniciar y terminar un ayuno
- [ ] Las estadísticas se actualizan
- [ ] El código está en GitHub (con .env en .gitignore)
- [ ] Has documentado cómo usar la dApp

---

## 🚀 Próximos Pasos

Una vez domines este proyecto, puedes:

1. **Desplegar en Base Mainnet**
   - Cambiar a chainId 8453
   - Usar ETH real (ten cuidado)
   - Auditar el código primero

2. **Añadir características avanzadas**
   - Sistema de logros/badges (NFTs)
   - Compartir progreso en redes sociales
   - Crear una comunidad (grupos de ayuno)

3. **Mejorar el contrato**
   - Permitir diferentes duraciones objetivo
   - Sistema de racha (streaks)
   - Función de donación a caridad

4. **Aprender más conceptos**
   - Proxies y actualizaciones
   - Tokens ERC-20/721
   - Oráculos (Chainlink)
   - Layer 2s y rollups

---

## ❓ Preguntas Frecuentes

### ¿Puedo cambiar el contrato después de desplegarlo?

**No.** Los smart contracts son inmutables.

**Soluciones:**
1. Desplegar una nueva versión (los usuarios deben migrar)
2. Usar patrón Proxy (avanzado)
3. Diseñar bien desde el inicio

---

### ¿Cuánto cuesta desplegar en mainnet?

**Depende de:**
- Tamaño del contrato
- Gas price del momento
- Complejidad del constructor

**Estimación para FastingTracker:**
- Base mainnet: ~$0.50 - $2 USD
- Ethereum mainnet: ~$50 - $200 USD

---

### ¿Es seguro guardar datos personales en blockchain?

**Considera:**
- Todo es PÚBLICO y PERMANENTE
- No guardes información sensible
- Los ayunos son relativamente inocuos
- Para datos privados, usa encriptación o soluciones off-chain

---

### ¿Puedo ganar dinero con esto?

**Opciones:**
1. Cobrar una pequeña fee por ayuno
2. Crear un token para recompensar usuarios
3. Vender NFTs de logros
4. Patrocinios/publicidad en el frontend

**Pero primero:** Aprende, construye, comparte.

---

## 🙏 Conclusión

¡Felicidades por llegar hasta aquí! Has aprendido:

✅ Qué es una dApp y cómo funciona
✅ Programación en Solidity
✅ Desarrollo con Hardhat
✅ Testing de smart contracts
✅ Despliegue en blockchain
✅ Integración con ethers.js
✅ Interacción wallet-dApp

**Este es solo el comienzo.** La tecnología blockchain está en constante evolución, y hay infinitas posibilidades para crear aplicaciones descentralizadas innovadoras.

**Sigue construyendo. Sigue aprendiendo. Sigue innovando.**

---

¿Necesitas ayuda con algún concepto específico? ¡No dudes en preguntar!