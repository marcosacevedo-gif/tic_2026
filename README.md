

# CICD de APPScript

## Configuración del repositorio y codespace

- **Configurar el entorno:** crea un Codespace o prepara tu máquina local con Node.js y `clasp`.
- **Instalaciones necesarias:** `npm install -g @google/clasp` y (opcional) `npm install` si tu proyecto usa dependencias.
- **Habilitar la Google Apps Script API:** ve a https://script.google.com/home/settings y habilita la API en tu cuenta de Google.

### Configurar el entorno

- Asegúrate de tener Node.js (>=14) y `clasp` instalados.
- Inicia sesión con `clasp login` y sigue las instrucciones para autorizar tu cuenta.

## Autenticación de clasp

- **Archivo de Credenciales de Usuario:** si necesitas usar credenciales de servicio o flujos sin interacción, guarda las credenciales en el directorio del proyecto y añade variables/secretos en GitHub Actions.
- Para login manual ejecuta:

```
clasp login --no-localhost
```

Si usas autentificación en CI, configura los secrets en GitHub (`GAS_CREDENTIALS`, `CLASP_TOKEN` u otros) y restaura esos archivos en el workflow.

## Estructura del proyecto

Ejemplo mínimo de estructura:

- .clasp.json  (configuración de `clasp`)
- package.json
- src/
	- code.js    (script de Apps Script)
- README.md

### Ejemplo de código para generar el documento (src/code.js)

Ejemplo de función de Apps Script que crea un documento de Google Docs:

```
function createDocument() {
	const doc = DocumentApp.create('Mi Documento desde Apps Script');
	const body = doc.getBody();
	body.appendParagraph('Hola desde Apps Script.');
	body.appendParagraph('Generado por `src/code.js`.');
	return doc.getId();
}
```

Guarda este archivo en `src/code.js` y usa `clasp push` para subirlo al proyecto de Apps Script.

## Pipeline de CI/CD (GitHub Actions)

### Pasos para el Workflow

1. Configurar Node.js en el runner.
2. Restaurar credenciales/secretos (guardar como archivos necesarios para `clasp`).
3. Instalar `clasp` y dependencias (`npm install -g @google/clasp`).
4. Hacer `clasp push` para desplegar el código al proyecto de Apps Script.
5. (Opcional) Ejecutar tests o verificaciones antes del despliegue.

Puedes implementar un workflow que haga `clasp push` en `push` a la rama `main` y use secrets para la autenticación.

---




