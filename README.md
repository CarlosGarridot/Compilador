# Compiler - Compilers Course

Complete compiler implementation that translates high-level source code into executable machine code (IA32). The project covers every phase of the compilation pipeline: lexical, syntactic, and semantic analysis, plus assembly code generation.

## Features

- **Front-End (Analysis)**
  - Lexical analysis with JFlex (scanner generator)
  - Syntactic analysis with CUP (LALR(1) parser)
  - Abstract Syntax Tree (AST) construction
  - Semantic analysis with an Underlying Basic Type System (TSB)
  - Symbol table with scope management

- **Back-End (Synthesis)**
  - Intermediate code generation (C3@ - Three-Address Code)
  - Assembly code generation (IA32, AT&T syntax)
  - Integration with a C runtime for I/O
  - Automatic linking via GCC

- **Error Recovery**
  - Panic Mode for syntactic recovery
  - Detailed error messages (in Spanish)
  - Multiple error detection in a single pass

## Architecture

```
Source Code (.txt)
        ↓
    Scanner (JFlex)
        ↓
    Parser (CUP)
        ↓
    AST (Abstract Syntax Tree)
        ↓
    Semantic Analysis
        ↓
    Intermediate Code Generator (C3@)
        ↓
    Assembly Generator (IA32)
        ↓
    GCC Linker
        ↓
    Executable
```

## Supported Language

### Data Types
- `int` - 32-bit integers
- `char` - ASCII characters
- `bool` - Boolean values (true/false)
- One-dimensional arrays: `int arr[10];`

### Control Structures
- `if/else` - Conditionals
- `while` - Iterative loops
- `for` - Full for-loops
- `switch/case` - Multi-way selection

### Operators
- **Arithmetic**: `+`, `-`, `*`, `/`, `%`
- **Relational**: `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Logical**: `&&`, `||`, `!`
- **Unary**: `-`, `!`

### Input/Output
```
leer(variable);      // Reads from standard input
imprimir(expresion); // Prints to standard output
```

## Requirements

### Linux (x86, 32/64-bit)
- **Java JDK 8+** - to build the compiler
  - Ubuntu/Debian: `sudo apt install openjdk-11-jdk`
- **GCC (GNU Compiler Collection)** - for assembling and linking
  - Ubuntu/Debian: `sudo apt install gcc`

### macOS Intel (x86-64)
- **Java JDK 8+** - to build the compiler
  - `brew install openjdk@11`
- **GCC (GNU Compiler Collection)** - for assembling and linking
  - `brew install gcc`

### macOS on Apple Silicon (ARM)

**Requirements:**
- **Java JDK 8+** - to build the compiler
  - `brew install openjdk@11`
- **Docker Desktop** - for the final build step (gcc + linking)
  - Download from: https://www.docker.com/products/docker-desktop
  - Docker Desktop must be open/running in the background

**Why Docker:**
The compiler emits IA32 (Intel 32-bit) code, which is incompatible with ARM processors. The `run.sh` script detects this automatically and uses Docker only for the final phase (assembling and linking with gcc).

**Automatic flow:**
```
1. Java builds the compiler (on ARM)
2. A .s assembly file is generated (on ARM)
3. Docker Desktop assembles it with gcc (in an x86-compatible container)
4. Final executable
```

### Windows (x86-64)
- **Java JDK 8+** - to build the compiler
  - Download from: https://www.oracle.com/java/technologies/downloads/
- **GCC (GNU Compiler Collection)** - for assembling and linking
  - Option 1: MinGW - https://www.mingw-w64.org/
  - Option 2: TDM-GCC - https://jmeubank.github.io/tdm-gcc/
  - **Important:** add GCC's `bin` folder to the system PATH

### -m32 flag (automatic)
- The `run.sh` and `run.bat` scripts include `-m32` automatically to ensure compatibility on 64-bit systems

## Building and Running

### Linux and macOS Intel (x86-64)

```bash
# First time only: grant execute permission
chmod +x run.sh

# Build and run
./run.sh programName.txt
```

### macOS on Apple Silicon (ARM)

The script handles this automatically.

```bash
# 1. Open Docker Desktop (once)
# 2. In the terminal:
chmod +x run.sh
./run.sh programName.txt
```

### Windows (x86-64)

```cmd
REM No special permissions required
.\run.bat programName.txt
```

## Project Structure

```
Practica-Compiladores-2025/
├── Compilador/              # Compiler's Java source
│   ├── AnalizadorSemantico.java
│   ├── GeneradorCodigo.java
│   ├── GeneradorEnsamblador.java
│   ├── GestorErrores.java
│   ├── TablaSimbolos.java
│   └── ...
├── ArbolSintactico/         # AST node classes
│   ├── NodoBase.java
│   ├── NodoExpresion.java
│   ├── NodoSentencia.java
│   └── ...
├── CodigoIntermedio/        # Intermediate representation
│   ├── Instruccion.java
│   ├── GeneradorCodigo.java
│   └── TipoInstruccion.java
├── Simbolos/                # Symbol management
│   ├── TablaSimbolos.java
│   ├── Descripcion.java
│   └── Categoria.java
├── TESTS/                   # Test cases
│   ├── testTipos.txt
│   ├── testOperaciones.txt
│   ├── testErroresSemanticos.txt
│   └── ...
├── scanner.flex             # JFlex specification
├── parser.cup               # CUP specification
├── runtime.c                # C I/O library
├── run.sh                   # Script for Linux/macOS
├── run.bat                  # Script for Windows
└── DOCUMENTACION.pdf        # Full documentation, 38 pages (in Spanish)
```

## Test Cases

The project includes multiple test cases validating:

### Valid Cases
- **testTipos.txt** - Type system and data structures
- **testOperaciones.txt** - Control flow and functions
- **testOperadores.txt** - Complex expressions
- **testExpresionesAritmeticasLogicas.txt** - Expression evaluation
- **testCalculadora.txt** - Full interactive program

### Error Cases
- **testErroresSemanticos.txt** - Undeclared variables, type mismatches
- **testErroresMixtos.txt** - Cascading error recovery
- **testErrorCalculadora.txt** - Robustness against syntax errors

Running a test:
```bash
./run.sh TESTS/testTipos.txt
```

## Detailed Documentation

For in-depth coverage of:
- Grammar design
- Symbol table implementation
- Code generation strategy
- Semantic analysis and the type system
- Error recovery

See **DOCUMENTACION.pdf** (Spanish, 38 pages).

## Main Components

### 1. Lexical Analysis (JFlex)
- Token recognition: keywords, identifiers, literals
- Line/column tracking for error reporting
- Escape sequences in character literals

### 2. Syntactic Analysis (CUP)
- LALR(1) parser with operator precedence
- Panic Mode recovery
- On-the-fly AST construction

### 3. Semantic Analysis
- Strict type validation
- Scope management (Global, Local, Parameters)
- Memory address (offset) computation
- Error detection: undeclared variables, incompatible types, etc.

### 4. Intermediate Code Generation
- Three-Address Code (C3@) representation
- Temporary variables and symbolic labels
- Automatic temporary-variable management

### 5. Assembly Generation
- Translation to IA32 (AT&T syntax)
- Stack frame management
- Integration with runtime.c for I/O

## Implemented Optimizations

- **AST flattening**: removes unnecessary nesting
- **Smart error recovery**: prevents cascades of false-positive errors
- **x64 compatibility**: `-m32` flag for 64-bit environments
- **Automatic cleanup**: scripts clear compiled artifacts before each run

## Author

Carlos Garrido del Toro
Computer Engineering - UIB (University of the Balearic Islands)
Course: Compilers - 2025-2026

Academic project

---
**For further detail, see DOCUMENTACION.pdf**
