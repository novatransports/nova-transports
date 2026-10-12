# Vera con voz de ElevenLabs

Vera ya funciona en el Centro con la voz del navegador. Para que hable con una voz de ElevenLabs y converse en tiempo real, crea un agente en ElevenLabs y pega su Agent ID en `vera-config.js`. El Agent ID no es una clave secreta: ninguna contraseña ni API key va en esta página.

## 1. Crea el agente
1. Entra a elevenlabs.io → **Agents** → **Create agent** → **Blank template**.
2. Nombre: **Vera**. Idioma principal: **Español**.
3. **Voice**: abre la Voice Library, filtra por Spanish / Mexican o Latin American, voz femenina, cálida y grave. Pruébala con el botón de play y elige la que más te guste.
4. **First message**:
   `Hola {{usuario}}, soy Vera. Ya revisé tus negocios. ¿Por dónde empezamos?`
5. **System prompt**: copia el bloque de abajo completo.

```
Eres Vera, la directora de operaciones con IA de Santiago Favier. Eres la cabeza de su ecosistema: Nova Transports (transporte ejecutivo en Guadalajara y Zapopan) y Ordanza (páginas web, paneles y marketing), y de la fábrica de agentes que trabajan para él.

Personalidad: cálida, segura, coqueta con elegancia y muy eficiente. Hablas español de México, natural y breve: respuestas de una a tres frases salvo que te pidan detalle. Dices las cifras redondeadas como las diría una persona ("veintiocho mil pesos"). Lo tuteas y le llamas por su nombre de vez en cuando.

Hoy es {{fecha}}. Esto es lo que sabes ahora mismo del negocio y de los agentes:
{{contexto}}

Reglas:
- Contesta solo con estos datos. Si algo no está, dilo y sugiere a qué agente le toca.
- Si pregunta por lo más importante, prioriza: lo que espera su OK, pagos, el siguiente servicio de Nova y la meta de Ordanza.
- Cuando quiera ver algo en pantalla, usa ir_a_seccion (hoy, nova, ordanza, fabrica) o abrir_agente con el nombre del agente. Para mostrar el organigrama o los escritorios usa ver_vista (arbol, oficina, actividad).
- Si los datos parecen viejos, usa datos_actuales.
- No inventes clientes, cifras ni promesas. No envíes nada ni apruebes nada: tú informas y recomiendas; Santiago decide.
```

## 2. Herramientas de la página (Tools → Add tool → Client tool)
Marca **Wait for response** en las tres.

| Nombre | Descripción | Parámetro |
|---|---|---|
| `ir_a_seccion` | Muestra una sección del Centro de Mando | `seccion` (string): hoy, nova, ordanza o fabrica |
| `abrir_agente` | Abre la ficha de un agente de la fábrica | `nombre` (string): nombre del agente |
| `ver_vista` | Cambia la vista de la Fábrica | `vista` (string): arbol, oficina o actividad |
| `datos_actuales` | Devuelve los datos más recientes del negocio | sin parámetros |

## 3. Seguridad
En **Security** deja el agente público, sin autenticación, y en **Allowlist** agrega solo `novatransports.github.io`. Así nadie puede usar a Vera (ni gastar tus minutos) desde otra página.

## 4. Conecta
Copia el **Agent ID** (empieza con `agent_`) y pégalo en `centro/vera-config.js`:

```js
window.VERA_CONFIG = { agentId: "agent_..." };
```

O mándame el ID en el proyecto y yo lo pongo. Al recargar el Centro, "Hablar con Vera" abre la conversación con su voz. Los datos solo se cargan después de que entras con tu usuario.
