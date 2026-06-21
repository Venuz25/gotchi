# Gotchi: Mascota Virtual Distribuida
Proyecto académico desarrollado para la materia de Sistemas Distribuidos en la Escuela Superior de Cómputo (ESCOM), Instituto Politécnico Nacional (IPN).

---

## Descripción Arquitectónica

Gotchi es un sistema web diseñado para gestionar la lógica y persistencia de estado de una entidad virtual interactiva ("mascota"). El propósito del repositorio es ilustrar la evolución del diseño de software mediante tres implementaciones que progresan desde un modelo monolítico básico hasta una arquitectura distribuida basada en microservicios.

## Versiones del Sistema

El proyecto está segmentado en tres carpetas que representan cada fase de desarrollo del sistema:

* **`api-gotchi/` (Versión API Básica):** Representa la arquitectura monolítica inicial. Centraliza la lógica de negocio, el control de variables vitales y los endpoints de interacción en una única aplicación REST construida con FastAPI.
* **`pwa-gotchi/` (Versión PWA):** Mantiene la arquitectura monolítica en el backend pero añade capacidades de *Progressive Web App* (PWA) en el cliente. Utiliza un manifiesto y *service workers* para permitir el almacenamiento en caché y la instalación simulada en dispositivos móviles.
* **`micro-gotchi/` (Versión Microservicios):** Constituye la infraestructura distribuida final. El dominio se descentraliza en cuatro microservicios independientes (Vitales, Economía, Inventario y Logros) coordinados sincrónicamente por un **API Gateway**. Esta versión implementa **Honcho** como herramienta de orquestación local para automatizar el ciclo de vida y arranque concurrente de todos los servicios.

## Tecnologías Utilizadas

* **Backend:** Python, FastAPI.
* **Gestión de Procesos:** Honcho.
* **Frontend:** HTML, CSS, JavaScript Vanilla, Vue.js.
* **Almacenamiento:** En memoria (volátil y aislado por cada entorno de servicio).
* **Comunicación:** RESTful API sobre protocolo HTTP (vía `httpx`).

## Instalación General

Antes de ejecutar cualquiera de las versiones, asegúrese de cumplir con los siguientes requisitos en su entorno local:

1. Clonar el repositorio
```bash
git clone [https://github.com/Venuz25/gotchi.git](https://github.com/Venuz25/gotchi.git)
cd gotchi
```

2. Instalar dependencias globalesBashpip install -r requirements.txt
```bash
Guía de Ejecución de las PrácticasA continuación se detallan las instrucciones específicas para iniciar y consumir cada una de las versiones del sistema de manera independiente.1. Versión API Básica (api-gotchi)Esta versión ejecuta únicamente el backend monolítico sin interfaz gráfica nativa, ideal para pruebas rápidas de endpoints.Bash# Navegar al directorio de la práctica
cd api-gotchi
```

### Iniciar práctica api-gotchi
```bash
# Navegar al directorio de la práctica
cd api-gotchi

# Iniciar el servidor que expone la API
uvicorn main:app --reload --port 8000
```
Acceso: Ingrese a http://127.0.0.1:8000/ en su navegador o acceda a http://127.0.0.1:8000/docs para interactuar con la documentación interactiva de Swagger.

### Iniciar práctica pwa-gotchi
Esta práctica arranca el backend monolítico configurado con soporte CORS y sirve estáticos de la interfaz interactiva móvil instalable.
```bash
# Navegar al directorio de la práctica
cd pwa-gotchi

# Iniciar el servidor que expone la API y el Frontend
uvicorn main:app --reload --port 8000
```
Acceso: Abra su navegador e ingrese a http://127.0.0.1:8000/.  
Desde ahí podrá visualizar la consola de juego y auditar el comportamiento del Service Worker en las herramientas de desarrollo.

### Iniciar práctica micro-gotchi
La infraestructura distribuida final utiliza un archivo Procfile para manejar la concurrencia. No requiere abrir terminales individuales para cada servicio; la herramienta Honcho automatiza todo el ecosistema.
```bash
# Navegar al directorio de la práctica distribuida
cd micro-gotchi

## Inicializar de manera simultánea la red de servicios y el Gateway
honcho start
```
Esto levantará en paralelo los siguientes componentes locales de forma automática: 
- Gateway Maestro: Puerto 8000
- MS Vitales: Puerto 8001
- MS Economía: Puerto 8002
- MS Inventario: Puerto 8003
- MS Logros: Puerto 8004

Acceso: Una vez que Honcho confirme la inicialización en la consola, ingrese a http://127.0.0.1:8000/ en su navegador.

---

## Referencia de API (Gateway Unificado)

Para la versión de microservicios, el Gateway centraliza las peticiones en el puerto `8000` exponiendo las siguientes rutas:

| Endpoint | Método | Descripción |
| --- | --- | --- |
| `/estado` | `GET` | Recupera el estado consolidado (vitales, saldo económico e historial de logros) consultando de forma interna la malla de servicios. |
| `/jugar` | `POST` | Orquesta la lógica distribuida: aplica desgaste en vitales, inyecta saldo en economía y audita la acción en logros. |
| `/comprar/{item}` | `POST` | Consulta costos en inventario, procesa el cobro en economía y altera los modificadores en vitales. |
| `/dormir` | `POST` | Restablece los niveles máximos de energía en vitales e incrementa los contadores del módulo de logros. |
| `/reiniciar` | `POST` | Envía una señal de purga síncrona para restablecer todas las bases de datos en memoria a sus valores iniciales. |

## Estructura de Directorios
``` Plaintext
gotchi/
├── api-gotchi/          # -> Versión 1: Monolito puro (Backend de prueba)
├── pwa-gotchi/          # -> Versión 2: Monolito + Progressive Web App integrado
├── micro-gotchi/        # -> Versión 3: Ecosistema distribuido multicanal
│   ├── frontend/        # Interfaz de usuario de estilo retro ciberpunk
│   ├── Procfile         # Configuración de topología de procesos para Honcho
│   ├── gateway.py       # API Gateway y Orquestador de peticiones HTTP
│   ├── servicio_economia.py
│   ├── servicio_inventario.py
│   ├── servicio_logros.py
│   └── servicio_vitales.py
├── requirements.txt
└── README.md
```

---

<div align="center">
  <p>
    <strong>Escuela Superior de Cómputo (ESCOM)</strong> | Instituto Politécnico Nacional (IPN)
  </p>
  <p>
    © 2026 - Repositorio académico de prácticas. 
    <br>
    Autor: <a href="https://github.com/[Tu_Usuario]">Areli Guevara</a>
  </p>
</div>
