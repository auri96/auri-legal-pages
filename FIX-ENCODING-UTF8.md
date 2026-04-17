# Problema de Codificación UTF-8 - Documentos Legales HTML

**Fecha:** 15 de abril de 2026  
**Problema:** Los archivos HTML muestran caracteres incorrectos (tildes mal codificadas)

---

## 🔴 Problema Identificado

Los archivos HTML en `auri-legal-pages/` tienen problemas de codificación UTF-8:

**Caracteres mal mostrados:**
- "PolÃ­tica" → debería ser "Política"
- "VersiÃ³n" → debería ser "Versión"
- "Ãšltima actualizac" →  debería ser "Última actualización"  
- "IdentificaciÃ³n" → debería ser "Identificación"
- "CÃ©dula de CiudadanÃ­a" → debería ser "Cédula de Ciudadanía"
- "BogotÃ¡" → debería ser "Bogotá"
- "TelÃ©fono" → debería ser "Teléfono"

---

## 🔍 Causa del Problema

Cuando PowerShell ejecutó los comandos de actualización de emails, guardó los archivos con codificación incorrecta (probablemente Windows-1252 interpretado como UTF-8), causando que los caracteres especiales se corrompieran.

---

## ✅ Soluciones Disponibles

### **Opción 1: Usar VS Code (RECOMENDADA) ⭐**

1. Abre cada archivo HTML en Visual Studio Code
2. En la barra inferior derecha, haz clic donde dice "UTF-8"
3. Selecciona "Reopen with Encoding"
4. Elige "Windows 1252" o "ISO 8859-1"
5. El archivo debería mostrar las tildes correctamente
6. Luego haz clic nuevamente en la codificación
7. Selecciona "Save with Encoding" → "UTF-8"
8. Guarda el archivo (Ctrl+S)

Repite para: `terms.html`, `privacy.html`, `support.html`

---

### **Opción 2: Regenerar desde Markdown**

Los archivos Markdown en `docs/legal/` tienen la codificación correcta. Puedes:

1. Usar un convertidor Markdown → HTML online
2. Copiar el contenido de los `.md` corregidos
3. Aplicar el mismo diseño HTML que tienes actualmente

---

### **Opción 3: Edición Manual**

Si quieres una solución rápida, puedes editar directamente los archivos HTML y usar buscar y reemplazar:

**Buscar → Reemplazar:**
```
Ã¡ → á
Ã© → é
Ã­ → í
Ã³ → ó
Ãº → ú
Ã± → ñ
Ã → Á
Ã‰ → É
Ã → Í
Ã" → Ó
Ãš → Ú
Ã' → Ñ
Â¿ → ¿
Â¡ → ¡
```

**Importante:** Después de hacer los reemplazos, guarda el archivo con codificación UTF-8 (sin BOM).

---

## 📝 Archivos Afectados

1. ✅ **terms.html** - Términos y Condiciones (necesita corrección)
2. ✅ **privacy.html** - Política de Privacidad (necesita corrección)
3. ✅ **support.html** - Centro de Soporte (necesita corrección)

---

## 🔧 Script de Corrección Automática

Si prefieres hacerlo automáticamente, aquí está el script correcto:

```powershell
# Ejecutar desde: auri-legal-pages/

$files = @("terms.html", "privacy.html", "support.html")

foreach ($file in $files) {
    # Leer como Windows-1252 y guardar como UTF-8
    $content = [IO.File]::ReadAllText($file, [Text.Encoding]::GetEncoding("Windows-1252"))
    $utf8 = New-Object System.Text.UTF8Encoding($false)
    [IO.File]::WriteAllText($file, $content, $utf8)
    Write-Host "✅ $file corregido"
}
```

---

## ✅ Verificación

Después de aplicar cualquier solución, abre los archivos en un navegador y verifica que se vean así:

- ✅ "Política de Privacidad"
- ✅ "Versión: 2.0"
- ✅ "Última actualización"
- ✅ "Cédula de Ciudadanía"
- ✅ "Bogotá D.C., Colombia"
- ✅ "Teléfono: +57 321 241 9690"

---

## 📚 Archivos de Referencia (Correctos)

Los siguientes archivos Markdown tienen la codificación correcta y pueden usarse como referencia:

- `/docs/legal/terminos-y-condiciones.md` ✅
- `/docs/legal/politica-privacidad.md` ✅  
- `/docs/legal/politica-pagos-suscripciones.md` ✅

---

## 🚨 Recomendación Final

**OPCIÓN MÁS SIMPLE:** Usa Visual Studio Code con la "Opción 1" descrita arriba. Es la forma más rápida y segura de corregir el problema sin riesgo de introducir más errores.

Una vez corregido, haz commit y push:
```bash
git add terms.html privacy.html support.html
git commit -m "fix: Corregir codificación UTF-8 en documentos legales HTML"
git push origin main
```

---

**Estado:** ⚠️ PENDIENTE DE CORRECCIÓN  
**Prioridad:** Alta (afecta presentación en producción)  
**Tiempo estimado:** 10-15 minutos
