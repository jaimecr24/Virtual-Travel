# Virtual-Travel

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
    Inicialmente se registran en la base de datos autobuses para cada día del mayo del 2022, a las 8h, 12h, 16h y 20h
  
  POST /api/v0/login
  
    Obtiene un token si los datos son correctos. Entrada:
    user: <string> (header)
    password: <string> (header)
    Salida: <string> (token)
    Usuario y contraseña disponible para pruebas: "usuario1:123456"
