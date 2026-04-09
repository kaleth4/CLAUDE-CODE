<div align="center">

```
  ██████╗██╗      █████╗ ██╗   ██╗██████╗ ███████╗     ██████╗ ██████╗ ██████╗ ███████╗
 ██╔════╝██║     ██╔══██╗██║   ██║██╔══██╗██╔════╝    ██╔════╝██╔═══██╗██╔══██╗██╔════╝
 ██║     ██║     ███████║██║   ██║██║  ██║█████╗      ██║     ██║   ██║██║  ██║█████╗  
 ██║     ██║     ██╔══██║██║   ██║██║  ██║██╔══╝      ██║     ██║   ██║██║  ██║██╔══╝  
 ╚██████╗███████╗██║  ██║╚██████╔╝██████╔╝███████╗    ╚██████╗╚██████╔╝██████╔╝███████╗
  ╚═════╝╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚═════╝ ╚══════╝     ╚═════╝ ╚═════╝ ╚═════╝ ╚══════╝

           ██╗    ██╗██╗███╗   ██╗██████╗  ██████╗ ██╗    ██╗███████╗
           ██║    ██║██║████╗  ██║██╔══██╗██╔═══██╗██║    ██║██╔════╝
           ██║ █╗ ██║██║██╔██╗ ██║██║  ██║██║   ██║██║ █╗ ██║███████╗
           ██║███╗██║██║██║╚██╗██║██║  ██║██║   ██║██║███╗██║╚════██║
           ╚███╔███╔╝██║██║ ╚████║██████╔╝╚██████╔╝╚███╔███╔╝███████║
            ╚══╝╚══╝ ╚═╝╚═╝  ╚═══╝╚═════╝  ╚═════╝  ╚══╝╚══╝ ╚══════╝
```

# Solución: Claude Code en Windows — Error Git Bash

**Documentación del problema · Diagnóstico · Solución paso a paso**

[![OS](https://img.shields.io/badge/Sistema-Windows%2010%2F11-0078d4?style=flat-square&logo=windows)](https://windows.com)
[![Tool](https://img.shields.io/badge/Herramienta-Claude%20Code-cc785c?style=flat-square)](https://claude.ai)
[![Requires](https://img.shields.io/badge/Requiere-Git%20for%20Windows-f05032?style=flat-square&logo=git)](https://git-scm.com)
[![Shell](https://img.shields.io/badge/Shell-PowerShell%205.1%2B-5391fe?style=flat-square&logo=powershell)]()
[![Status](https://img.shields.io/badge/Estado-Resuelto%20✅-2e7d32?style=flat-square)]()

</div>

---

## Índice

- [¿Qué es este documento?](#-qué-es-este-documento)
- [El error original](#-el-error-original)
- [¿Por qué ocurre?](#-por-qué-ocurre)
- [Diagnóstico](#-diagnóstico)
- [Solución completa paso a paso](#-solución-completa-paso-a-paso)
- [Verificación](#-verificación)
- [Problemas adicionales](#-problemas-adicionales)
- [Referencia rápida](#-referencia-rápida)

---

## 📋 ¿Qué es este documento?

Este README documenta los errores encontrados al instalar **Claude Code** en Windows y la solución que los resolvió: instalar y vincular correctamente **Git for Windows (Git Bash)**.

> **Claude Code** es la CLI de Anthropic para interactuar con Claude desde la terminal. En Windows, **depende obligatoriamente de Git Bash** como entorno de ejecución. Sin él, Claude Code no puede arrancar.

---

## 🔴 El Error Original

Al ejecutar el comando de instalación de Claude Code en PowerShell:

```powershell
irm https://claude.ai/install.ps1 | iex
```

Se presentó el siguiente error:

```
Setting up Claude Code...

Claude Code on Windows requires git-bash (https://git-scm.com/downloads/win).
If installed but not in PATH, set environment variable pointing to your bash.exe,
similar to: CLAUDE_CODE_GIT_BASH_PATH=C:\Program Files\Git\bin\bash.exe

✅ Installation complete!
```

Y al intentar ejecutar Claude Code directamente:

```powershell
PS C:\Users\WolvesTI> claude
```

```
Claude Code was unable to find CLAUDE_CODE_GIT_BASH_PATH path
"C:\Program Files\Git\bin\bash.exe"

PS C:\Users\WolvesTI>
```

> ⚠️ El instalador marca `✅ Installation complete!` pero **Claude Code no funciona** — la instalación en sí termina, pero no puede ejecutarse por la dependencia faltante.

---

## 🔍 ¿Por Qué Ocurre?

Claude Code necesita **Git Bash** para ejecutar comandos en Windows de la misma forma que lo haría en Linux/macOS. Sin este intérprete, no puede operar.

```
Claude Code (CLI)
       │
       │ necesita para ejecutar comandos
       ▼
  Git Bash (bash.exe)
       │
       │ busca en variable de entorno
       ▼
  CLAUDE_CODE_GIT_BASH_PATH
       │
       │ apunta a
       ▼
  C:\...\Git\bin\bash.exe
       │
       ├── ✅ Si existe → Claude Code funciona
       └── ❌ Si no existe → Error al iniciar
```

**Las dos causas posibles del error:**

| Causa | Síntoma | Solución |
|---|---|---|
| Git no está instalado | `where.exe git` no devuelve nada | Instalar Git for Windows |
| Git está instalado pero no en PATH | `where.exe git` devuelve una ruta pero Claude sigue fallando | Configurar variable de entorno manualmente |

---

## 🩺 Diagnóstico

Antes de aplicar la solución, ejecuta este comando para determinar cuál es tu caso:

```powershell
where.exe git
```

### Resultado A — Git NO está instalado

```
PS C:\Users\WolvesTI> where.exe git
INFORMACIÓN: no se pudo encontrar ningún archivo para los patrones dados.
```

→ **Ir directamente al Paso 1 de la solución.**

### Resultado B — Git SÍ está instalado

```
PS C:\Users\WolvesTI> where.exe git
C:\Users\WolvesTI\AppData\Local\Programs\Git\cmd\git.exe
```

→ **Git existe pero Claude no lo detecta. Ir al Paso 3 de la solución.**

---

## ✅ Solución Completa Paso a Paso

### Paso 1 — Descargar Git for Windows

Ir a la página oficial de Git:

```
https://git-scm.com/downloads/win
```

Descargar **"64-bit Git for Windows Setup"** (o 32-bit según tu sistema).

---

### Paso 2 — Instalar Git correctamente

Durante la instalación, el asistente presentará varias pantallas. Las opciones críticas son:

```
┌─────────────────────────────────────────────────────────────┐
│  Select Components                                          │
│                                                             │
│  [✓] Git Bash Here          ← OBLIGATORIO                   │
│  [✓] Git GUI Here                                           │
│  [✓] Associate .sh files...                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  Adjusting your PATH environment                            │
│                                                             │
│  ( ) Use Git from Git Bash only                             │
│  (●) Git from the command line and also from 3rd-party      │
│      software                              ← SELECCIONAR    │
│  ( ) Use Git and optional Unix tools...                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

> Dejar el resto de opciones en sus valores por defecto y completar la instalación.

---

### Paso 3 — Localizar bash.exe

Una vez instalado Git, encuentra la ruta exacta de `bash.exe`:

```powershell
# Opción A: Usando where.exe después de instalar
where.exe bash

# Opción B: Verificación directa de rutas comunes
Test-Path "C:\Program Files\Git\bin\bash.exe"
Test-Path "$env:USERPROFILE\AppData\Local\Programs\Git\bin\bash.exe"
Test-Path "C:\Program Files (x86)\Git\bin\bash.exe"
```

**Rutas comunes según dónde instala Git:**

| Tipo de instalación | Ruta típica de bash.exe |
|---|---|
| Para todos los usuarios (default) | `C:\Program Files\Git\bin\bash.exe` |
| Solo para el usuario actual | `C:\Users\<TuUsuario>\AppData\Local\Programs\Git\bin\bash.exe` |
| Instalación en 32-bit | `C:\Program Files (x86)\Git\bin\bash.exe` |
| Ruta personalizada | La que elegiste durante la instalación |

---

### Paso 4 — Configurar la Variable de Entorno

Abre **PowerShell** y ejecuta el comando correspondiente a tu ruta:

#### Opción A — Ruta estándar (instalación para todos los usuarios)

```powershell
[System.Environment]::SetEnvironmentVariable(
    "CLAUDE_CODE_GIT_BASH_PATH",
    "C:\Program Files\Git\bin\bash.exe",
    "User"
)
```

#### Opción B — Ruta de usuario (instalación solo para el usuario actual)

```powershell
$bashPath = "C:\Users\$($env:USERNAME)\AppData\Local\Programs\Git\bin\bash.exe"

[System.Environment]::SetEnvironmentVariable(
    "CLAUDE_CODE_GIT_BASH_PATH",
    $bashPath,
    "User"
)
```

#### Opción C — Detección automática (recomendado si no sabes la ruta)

```powershell
# Busca bash.exe automáticamente y configura la variable
$posibles_rutas = @(
    "C:\Program Files\Git\bin\bash.exe",
    "$env:USERPROFILE\AppData\Local\Programs\Git\bin\bash.exe",
    "C:\Program Files (x86)\Git\bin\bash.exe"
)

$ruta_encontrada = $posibles_rutas | Where-Object { Test-Path $_ } | Select-Object -First 1

if ($ruta_encontrada) {
    [System.Environment]::SetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", $ruta_encontrada, "User")
    Write-Host "✅ Variable configurada: $ruta_encontrada" -ForegroundColor Green
} else {
    Write-Host "❌ No se encontró bash.exe. ¿Está Git instalado?" -ForegroundColor Red
}
```

---

### Paso 5 — Reiniciar PowerShell

> ⚠️ **Este paso es obligatorio.** Las variables de entorno solo se cargan al iniciar una nueva sesión.

```
1. Cerrar TODAS las ventanas de PowerShell abiertas
2. Cerrar TODAS las terminales y símbolo del sistema
3. Abrir una NUEVA ventana de PowerShell
```

---

### Paso 6 — Reinstalar / Ejecutar Claude Code

```powershell
# Reinstalar Claude Code (si no lo habías instalado aún)
irm https://claude.ai/install.ps1 | iex

# O simplemente ejecutar si ya estaba instalado
claude
```

---

## 🔎 Verificación

Confirma que todo está correcto con estos comandos:

```powershell
# 1. Verificar que la variable de entorno está configurada
[System.Environment]::GetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "User")
# → Debe devolver la ruta completa a bash.exe

# 2. Verificar que bash.exe existe en esa ruta
$ruta = [System.Environment]::GetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "User")
Test-Path $ruta
# → Debe devolver: True

# 3. Verificar que Git está en PATH
where.exe git
# → Debe devolver una ruta válida

# 4. Ejecutar Claude Code
claude
# → Debe iniciar sin errores
```

**Output esperado después de la solución:**

```
PS C:\Users\WolvesTI> claude

╭─────────────────────────────────────────╮
│ ✻ Welcome to Claude Code!               │
│                                         │
│   /help for help, /exit to quit         │
╰─────────────────────────────────────────╯

>
```

---

## 🔧 Problemas Adicionales

### Error: Variable configurada pero Claude sigue fallando

```powershell
# La variable puede estar en sesión actual pero no persistida
# Verificar en ambos niveles:
[System.Environment]::GetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "User")
[System.Environment]::GetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "Machine")

# Si ambos devuelven null, configurar de nuevo y cerrar la terminal
```

### Error: Git instalado pero `where.exe git` no devuelve nada

```powershell
# Agregar Git al PATH manualmente
$gitPath = "C:\Program Files\Git\cmd"
$currentPath = [System.Environment]::GetEnvironmentVariable("PATH", "User")
[System.Environment]::SetEnvironmentVariable("PATH", "$currentPath;$gitPath", "User")
# Reiniciar terminal y volver a verificar
```

### Error: Extensión de VS Code no detecta Git Bash

Si usas el **plugin de Claude Code para VS Code** y sigue sin funcionar:

```json
// settings.json de VS Code
{
    "terminal.integrated.defaultProfile.windows": "Git Bash",
    "terminal.integrated.profiles.windows": {
        "Git Bash": {
            "path": "C:\\Program Files\\Git\\bin\\bash.exe",
            "args": []
        }
    }
}
```

---

## 📋 Referencia Rápida

**Comandos esenciales de un vistazo:**

```powershell
# Diagnóstico
where.exe git
where.exe bash

# Configurar variable (ajusta la ruta a tu instalación)
[System.Environment]::SetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "C:\Program Files\Git\bin\bash.exe", "User")

# Verificar configuración
[System.Environment]::GetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "User")

# Reinstalar Claude Code
irm https://claude.ai/install.ps1 | iex

# Ejecutar Claude Code
claude
```

---

## 📌 Resumen del Problema y Solución

```
PROBLEMA
────────
Claude Code requiere Git Bash en Windows.
Git no estaba instalado → bash.exe no existía → Claude Code no podía iniciar.

CAUSA
─────
El instalador de Claude Code marca ✅ al terminar, pero la ejecución
depende de una variable de entorno (CLAUDE_CODE_GIT_BASH_PATH) que
apunte a bash.exe. Sin Git instalado, esa ruta no existe.

SOLUCIÓN
────────
1. Instalar Git for Windows desde git-scm.com
2. Localizar bash.exe (por defecto: C:\Program Files\Git\bin\bash.exe)
3. Configurar CLAUDE_CODE_GIT_BASH_PATH con SetEnvironmentVariable
4. Reiniciar PowerShell
5. Ejecutar: claude
```

---

<div align="center">

**Documentado por WolvesTI · Problema resuelto instalando Git for Windows**

[git-scm.com](https://git-scm.com/downloads/win) · [Claude Code Docs](https://docs.claude.ai) · [Reporte de Bug #8674](https://github.com/anthropics/claude-code/issues/8674)

</div>
