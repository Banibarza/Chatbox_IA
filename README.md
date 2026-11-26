# 🤖 Chatbot IA con Spring Boot + Ollama

Chatbot que usa un **modelo de lenguaje que corre en tu propia computadora** (con [Ollama](https://ollama.com/)) a través de **Spring AI**. Las respuestas llegan en **streaming**: el texto aparece en pantalla mientras el modelo lo va generando, igual que en ChatGPT.

![Java](https://img.shields.io/badge/Java_17-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_3.3-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring AI](https://img.shields.io/badge/Spring_AI-6DB33F?style=flat-square&logo=spring&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?style=flat-square&logo=ollama&logoColor=white)

<!-- Agrega aquí un GIF o captura del chat respondiendo:
![Demo](docs/demo.gif)
-->

## ✨ Características

- **Modelo local:** no depende de una API de pago ni envía tus mensajes a internet.
- **Respuesta en streaming** con `Flux<String>` (Project Reactor), leída en el navegador con `ReadableStream`.
- **Dos modos** para comparar:
  - `/stream.html`: la respuesta aparece palabra por palabra.
  - `/index.html`: espera la respuesta completa y la muestra de una vez.

## 🔌 Endpoints

| Método | Ruta | Respuesta |
|---|---|---|
| `GET` | `/chat?message=...` | Respuesta completa como texto |
| `GET` | `/stream?message=...` | Respuesta en streaming (`text/plain`) |

```java
@GetMapping(value = "/stream", produces = "text/plain;charset=UTF-8")
public Flux<String> stream(@RequestParam String message) {
    return chatClient.prompt().user(message).stream().content();
}
```

## ▶️ Cómo ejecutarlo

**Requisitos:** Java 17 y [Ollama](https://ollama.com/download).

1. Descarga el modelo y deja Ollama corriendo:
   ```bash
   ollama run llama3
   ```
2. Inicia el backend:
   ```bash
   ./mvnw spring-boot:run
   ```
3. Abre en el navegador: **http://localhost:8080/stream.html**

Para usar otro modelo (por ejemplo `phi` o `mistral`), cámbialo en `src/main/resources/application.properties`:

```properties
spring.ai.ollama.base-url=http://localhost:11434
spring.ai.ollama.chat.options.model=llama3
```

## 📁 Estructura

```
src/main/
├── java/chatbot/streaming/
│   ├── StreamingApplication.java   # Punto de entrada
│   └── ChatController.java         # Endpoints /chat y /stream
└── resources/
    ├── application.properties      # Configuración de Ollama
    └── static/
        ├── index.html              # Chat con respuesta completa
        └── stream.html             # Chat con streaming
```

---

Hecho por **Bani Rafael Juárez Barza** · [LinkedIn](https://www.linkedin.com/in/bani-rafael-juarez-barza-67711b2b2/)
