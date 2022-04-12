# Virtual-Travel

Backend de una página web para la agencia de autobuses ‘VIRTUAL-TRAVEL’. Las aplicaciones incluyen un juego de datos inicial para pruebas. Se permite reservar para viajes a Valencia, Madrid, Barcelona y Bilbao. Las horas de salida de los autobuses son: 8h, 12h, 16h y 20h. Sólo se pueden crear reservas para mayo del 2022.

Para clonar el proyecto: "git clone --recurse-submodules <url> <target_directory>"
  
Una vez clonado se deben construir los ficheros jar de cada aplicación, ejecutando "mvn package" en el directorio raíz de cada una
  
Después ejecutar "docker-compose up -d"

Se crearán los siguientes contenedores:
  - zookeeper
  - kafka
  - init-kafka (crea los tópicos necesarios para la comunicación entre aplicaciones)
  - pg_backempresa (base de datos de backempres en postgres)
  - pg_backweb (base de datos de backweb en postgres)
  - pg_backweb-2 (base de datos de backweb-2 en postgres)
  - backempresa (aplicación)
  - backweb (aplicación)
  - backweb-2 (aplicación)
  
Una vez creados los tópicos, se puede eliminar el contenedor init-kafka

zookeeper es accesible desde el exterior en el puerto 22181 y kafka en el 29092.
Para eliminar este acceso, simplemente eliminar el mapeo de puertos de kafka y zookeeper en docker-compose.yml
  
Se puede acceder a las aplicaciones en los siguientes puertos:
  - backempresa: localhost:8085
  - backweb: localhost:8090
  - backweb-2: localhost:8091
  
 
API BACKWEB:
  
  POST /api/v0/reserva
  
    Crea una nueva reserva. Entrada:
    ReservaInputDto { "idDestino":"string(3)", "nombre":"string", "apellido":"string", "telefono":"string", "email":"string", "fechaReserva":"yyyy-MM-dd", "horaSalida": 0}
    idDestino: valores posibles: "VAL","MAD","BIL","BAR"
    Salida: ReservaOutputDto
  
  POST /api/v0/login
  
    Obtiene un token si los datos son correctos. Entrada:
    user: <string> (header)
    password: <string> (header)
    Salida: <string> (token)
    Usuario y contraseña disponible para pruebas: "usuario1:123456"
  
  GET /api/v0/reserva/{ciudadDestino}
  
    Permite consultar la reservas realizadas para un destino. Entrada:
    ciudadDestino (path)
    token (body - texto simple)
    fechaInferior (param - requerido - string con formato ddMMyyyy)
    fechaSuperior (param - no requerido)
    horaInferior (param - no requerido - string con formato 00)
    horaSuperior (param - no requerido)
    Salida: Lista de ReservaOutputDto | Si el token no es válido devuelve 403

  GET /api/v0/disponible/{destino}
  
    Permite consultar las plazas disponibles para un destino. Entrada:
    destino (path)
    fechaInferior (param - requerido - string con formato ddMMyyyy)
    fechaSuperior (param - no requerido)
    horaInferior (param - no requerido - string con formato 00)
    horaSuperior (param - no requerido)
    Salida: Lista de ReservaDisponibleOutputDto { "ciudadDestino":"string", "fechaReserva":"string", "horaReserva":0, "plazasLibres":0 }
  
API BACKEMPRESA:
  
  POST /api/v0/login

    Obtiene un token si los datos son correctos. Entrada:
    user: <string> (header)
    password: <string> (header)
    Salida: <string> (token)
    Usuario y contraseña disponible para pruebas: "usuario1:123456"  
  
  GET /api/v0/token/{token}
  
    Comprueba la validez de un token. Entrada:
    token: <string> (path)
    Salida: 200 si es válido, sino 403.
  
  GET /api/v0/correos
  
    Obtiene la lista de reservas (correos) enviados. Entrada:
    Auth con token válido.
    ciudadDestino: <string> (param - requerido)
    fechaInferior (param - requerido - string con formato ddMMyyyy)
    fechaSuperior (param - no requerido)
    horaInferior (param - no requerido - string con formato 00)
    horaSuperior (param - no requerido)
    Salida: Lista de ReservaOutputDto | Si el token no es válido devuelve 403
  
  GET /api/v0/reservas
  
    Obtiene la lista de reservas para un destino, fecha y hora concretas. Entrada:
    Auth con token válido.
    ciudadDestino: <string> (param - requerido)
    fecha (param - requerido - string con formato ddMMyyyy)
    hora (param - requerido - string con formato 00)
    Salida: Lista de ReservaOutputDto | Si el token no es válido devuelve 403
  
  PUT /api/v0/correos
  
    Reenvía un correo. Entrada:
    Auth con token válido.
    CorreoInputDto { "ciudadDestino":"string", "email":"string", "fechaReserva":"yyyy-MM-dd", "horaReserva":0 } (body)
    Salida: ReservaOutputDto con los datos de la reserva | 403 si el token no es válido.
  
  POST /api/v0/reserva
  
    Crea directamente una reserva en la base de datos de backempresa. Entrada:
    Auth con token válido.
    ReservaInputDto { "idDestino":"string(3)", "nombre":"string", "apellido":"string", "telefono":"string", "email":"string", "fechaReserva":"yyyy-MM-dd", "horaSalida":0 } (body)
    Salida: ReservaOutputDto | 403 si el token no es válido.
  
  GET /api/v0/plazas/{destino}
  
    Obtiene el número de plazas libres para un destino. Entrada:
    Auth con token válido.
    destino: <string> (path)
    fecha: <string> (param - requerido - formato ddMMYYYY)
    hora: <string> (param - requerido - formato 00)
    Salida: plazas (Integer) | 403 si el token no es válido.
  
  
