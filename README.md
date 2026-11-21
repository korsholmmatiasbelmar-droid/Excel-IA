================================================================================
           IA PARA MICROSOFT EXCEL — ASISTENTE DE CONTABILIDAD (MCP)
================================================================================

Un servidor MCP (Model Context Protocol) que conecta asistentes de Inteligencia 
Artificial (como opencode, Claude Desktop, Cursor o Antigravity) directamente 
con Microsoft Excel en tiempo real.

Diseñado especialmente para estudiantes, contadores y profesionales que 
necesitan resolver ejercicios contables y financieros: permite que la IA actúe 
como un copiloto al lado de tu planilla, capaz de inspeccionar datos, verificar 
fórmulas, registrar asientos contables (Libro Diario, Mayor, Balances) y 
recalcular resultados automáticamente sin salir de la conversación.


--------------------------------------------------------------------------------
1. CARACTERÍSTICAS PRINCIPALES
--------------------------------------------------------------------------------

- Control nativo y en tiempo real:
  Interactúa directamente con la ventana de Excel abierta sin requerir 
  plugins externos ni servicios en la nube.

- Lectura y auditoría inteligente:
  Inspecciona valores visibles y fórmulas subyacentes (=SUMA, =BUSCARV, etc.) 
  para explicar qué hace una planilla o detectar errores contables.

- Creación y apertura automática:
  Abre planillas existentes o crea nuevos libros .xlsx estructurados con el 
  nombre de hoja que elijas.

- Escritura protegida con confirmación:
  Inserta tablas, filas y asientos contables completos, exigiendo siempre 
  confirmación explícita para evitar sobreescrituras accidentales.

- Recálculo instantáneo:
  Fuerza a Excel a recalcular todas las fórmulas tras ingresar asientos o 
  transacciones, asegurando balances cuadrados al momento.


--------------------------------------------------------------------------------
2. REQUISITOS DEL SISTEMA
--------------------------------------------------------------------------------

1. Sistema Operativo:
   macOS (compatible con procesadores Apple Silicon M1/M2/M3/M4 e Intel).

2. Aplicación:
   Microsoft Excel para Mac instalado en /Applications/Microsoft Excel.app.

3. Entorno de Ejecución:
   Node.js versión 18 o superior instalado en el equipo.

4. Cliente de IA:
   Cualquier software compatible con el protocolo MCP (opencode, Claude Desktop, 
   Antigravity, Cursor, etc.).

5. Permisos de macOS:
   Autorización de Automatización (Ajustes del Sistema -> Privacidad y seguridad 
   -> Automatización) la primera vez que se ejecute, permitiendo controlar 
   Microsoft Excel.


--------------------------------------------------------------------------------
3. INSTALACIÓN PASO A PASO
--------------------------------------------------------------------------------

Paso 1: Abrir la carpeta del proyecto en la terminal
---------------------------------------------------
  cd "IA PARA EXCEL"

Paso 2: Instalar dependencias del proyecto
-----------------------------------------
  npm install

Paso 3: Compilar el código TypeScript a JavaScript
--------------------------------------------------
  npm run build

Paso 4: Ejecutar la prueba de verificación (Smoke Test)
------------------------------------------------------
  npm test

  * Deberás ver el mensaje:
    "OK: 7 herramientas -> ... Prueba de humo superada."


--------------------------------------------------------------------------------
4. CÓMO USARLO Y CONECTAR TU ASISTENTE DE IA
--------------------------------------------------------------------------------

OPCIÓN A: Arranque rápido con un clic (Recomendado)
---------------------------------------------------
El proyecto incluye un lanzador automático:
- Haz doble clic sobre el archivo "Abrir-microsoft-excel.command".
- Si macOS advierte sobre permisos, ábrelo desde la terminal con:
  bash Abrir-microsoft-excel.command


OPCIÓN B: Configuración manual en clientes MCP
----------------------------------------------

1) En opencode:
   Ejecuta en la terminal del proyecto:
     npm run configurar -- --nombre=microsoft-excel

   O agrégalo en tu archivo opencode.json:
   {
     "mcp": {
       "microsoft-excel": {
         "type": "local",
         "command": ["node", "./dist/index.js"],
         "enabled": true,
         "timeout": 30000
       }
     }
   }

2) En Claude Desktop (claude_desktop_config.json):
   Abre ~/Library/Application Support/Claude/claude_desktop_config.json y agrega:
   {
     "mcpServers": {
       "microsoft-excel": {
         "command": "node",
         "args": [
           "/RUTA/ABSOLUTA/A/IA PARA EXCEL/dist/index.js"
         ]
       }
     }
   }

3) En Antigravity CLI (agy) o Cursor:
   Registra el servidor stdio con:
     agy mcp add --type stdio microsoft-excel node "/RUTA/ABSOLUTA/A/IA PARA EXCEL/dist/index.js"


--------------------------------------------------------------------------------
5. LISTA DE HERRAMIENTAS DISPONIBLES (TOOLS)
--------------------------------------------------------------------------------

El servidor expone 7 herramientas especializadas:

1. listar_libros_abiertos
   - Qué hace: Inspecciona y devuelve todos los libros de Excel abiertos, sus 
     rutas en el disco y las hojas que contienen.
   - Parámetros: Ninguno.
   - ¿Modifica datos?: NO (Solo lectura).

2. leer_rango
   - Qué hace: Lee los valores visibles de un rango de celdas en notación A1.
   - Parámetros: libro (texto), hoja (texto), rango (ej. "A1:D20").
   - ¿Modifica datos?: NO (Solo lectura).

3. leer_formulas
   - Qué hace: Extrae el texto exacto de las fórmulas de un rango (ej. =SUMA(...)), 
     ideal para auditar y revisar cálculos contables.
   - Parámetros: libro (texto), hoja (texto), rango (ej. "E2:E30").
   - ¿Modifica datos?: NO (Solo lectura).

4. crear_libro
   - Qué hace: Crea un nuevo libro de trabajo en blanco y opcionalmente lo guarda 
     como archivo .xlsx con el nombre de hoja que elijas.
   - Parámetros: guardar_como (opcional, ruta absoluta), nombre_hoja (opcional).
   - ¿Modifica datos?: SÍ (Crea archivo nuevo, no sobrescribe existentes).

5. abrir_libro
   - Qué hace: Abre en Microsoft Excel un archivo existente (.xlsx, .xlsm, .xls o .csv).
   - Parámetros: ruta (ruta absoluta del archivo a abrir).
   - ¿Modifica datos?: NO (Solo abre el archivo).

6. escribir_rango
   - Qué hace: Escribe una matriz de valores o fórmulas en el rango especificado. 
     Exige confirmación explícita para evitar errores de sobreescritura.
   - Parámetros: libro, hoja, rango, valores (matriz de datos), confirmar (debe ser true).
   - ¿Modifica datos?: SÍ (Sobrescribe celdas).

7. recalcular_libro
   - Qué hace: Fuerza a Microsoft Excel a recalcular todas las fórmulas de todas 
     las hojas del libro activo para actualizar balances y totales.
   - Parámetros: libro (nombre del libro abierto).
   - ¿Modifica datos?: NO (Actualiza resultados de fórmulas).

* Nota de seguridad: La lectura y escritura tienen un límite de 5.000 celdas por llamada.


--------------------------------------------------------------------------------
6. EJEMPLOS PRÁCTICOS DE USO CON LA IA
--------------------------------------------------------------------------------

1) Auditoría y revisión contable:
   "Revisa el libro abierto 'Balance_General.xlsx' en la hoja 'Activos'. Lee el 
   rango A1:E25, explícame qué cuentas están registradas y revisa si la fórmula 
   del total de activos está sumando todas las filas correctamente."

2) Estructuración de un Libro Diario:
   "Crea un libro nuevo llamado 'Contabilidad_2026.xlsx' en mi Escritorio con una 
   hoja llamada 'Libro Diario'. Escribe en la primera fila los encabezados: 
   Fecha, Código Cuenta, Glosa / Detalle, Debe y Haber."

3) Registro de asientos y cuadre de balance:
   "En el libro 'Contabilidad_2026.xlsx', hoja 'Libro Diario', ingresa el asiento 
   de apertura con Caja al Debe por $1.500.000 y Capital al Haber por $1.500.000 
   en el rango A2:E3. Luego añade la fila de sumas totales y recalcula el libro."

4) Detección de errores en fórmulas:
   "Usa leer_formulas en la hoja 'Resultados', rango C10:C15. Dime qué fórmulas 
   se usaron para calcular el Margen Operacional y si hay alguna referencia 
   circular o error en los rangos."


--------------------------------------------------------------------------------
7. ESTRUCTURA DEL PROYECTO
--------------------------------------------------------------------------------

IA PARA EXCEL/
├── src/
│   ├── index.ts                 # Servidor stdio MCP principal
│   ├── lib/
│   │   ├── excel.ts             # Conexión nativa con Excel (AppleScript / JXA)
│   │   ├── exec.ts              # Ejecución segura de comandos del sistema
│   │   └── respuestas.ts        # Respuestas formateadas del protocolo MCP
│   └── tools/
│       └── microsoft-excel.ts   # Implementación de las 7 herramientas
├── dist/                        # Código JavaScript compilado
├── scripts/
│   ├── smoke-test.mjs           # Verificación de conexión y herramientas
│   └── configurar-opencode.mjs  # Utilidad de registro automático
├── Abrir-microsoft-excel.command # Lanzador de un clic para macOS
├── package.json                 # Metadatos y scripts npm
└── tsconfig.json                # Configuración de compilación TypeScript


--------------------------------------------------------------------------------
8. LICENCIA
--------------------------------------------------------------------------------

Distribuido bajo licencia MIT. Libre para uso educativo, personal y profesional.
================================================================================
