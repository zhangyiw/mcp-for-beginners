<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "96e08a8c1049dab757deb64cce4ea1e8",
  "translation_date": "2025-05-16T15:15:35+00:00",
  "source_file": "03-GettingStarted/04-vscode/solution/README.md",
  "language_code": "es"
}
-->
# Ejecutando el ejemplo

Aquí asumimos que ya tienes un código de servidor funcionando. Por favor, localiza un servidor de uno de los capítulos anteriores.

## Configurar mcp.json

Aquí tienes un archivo que puedes usar como referencia, [mcp.json](../../../../../03-GettingStarted/04-vscode/solution/mcp.json).

Modifica la entrada del servidor según sea necesario para indicar la ruta absoluta a tu servidor, incluyendo el comando completo necesario para ejecutarlo.

En el archivo de ejemplo mencionado arriba, la entrada del servidor se ve así:

```json
"hello-mcp": {
    "command": "cmd",
    "args": [
        "/c", "node", "<absolute path>\\build\\index.js"
    ]
}
```

Esto corresponde a ejecutar un comando como este: `cmd /c node <ruta absoluta>\\build\index.js`. 

- Change this server entry to fit where your server file is located or to what's needed to startup your server depending on your chosen runtime and server location.

## Consume the features in the server

- Click the `play` icon, once you've added *mcp.json* to *./vscode* folder, 

    Observe the tooling icon change to increase the number of available tools. Tooling icon is located right above the chat field in GitHub Copilot.

## Run a tool

- Type a prompt in your chat window that matches the description of your tool. For example to trigger the tool `add` escribe algo como "add 3 to 20".

    Deberías ver que se presenta una herramienta encima del cuadro de texto del chat indicando que debes seleccionar para ejecutar la herramienta, como en esta imagen:

    ![VS Code indicando que quiere ejecutar una herramienta](../../../../../translated_images/vscode-agent.d5a0e0b897331060518fe3f13907677ef52b879db98c64d68a38338608f3751e.es.png)

    Seleccionar la herramienta debería producir un resultado numérico que diga "23" si tu mensaje fue como mencionamos anteriormente.

**Aviso legal**:  
Este documento ha sido traducido utilizando el servicio de traducción automática [Co-op Translator](https://github.com/Azure/co-op-translator). Aunque nos esforzamos por la precisión, tenga en cuenta que las traducciones automáticas pueden contener errores o inexactitudes. El documento original en su idioma nativo debe considerarse la fuente autorizada. Para información crítica, se recomienda la traducción profesional realizada por un humano. No nos hacemos responsables por malentendidos o interpretaciones erróneas derivadas del uso de esta traducción.