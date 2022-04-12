# Virtual-Travel

Para clonar el proyecto: "git clone --recurse-submodules <url> <target_directory>"
  
Una vez clonado ejecutar "docker-compose up -d"
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
zookeeper es accesible desde el exterior en el puerto 22181 y kafka en el 29092
Para eliminar este acceso, simplemente eliminar el mapeo de puertos de kafka y zookeeper en docker-compose.yml
  
Se puede acceder a las aplicaciones en los siguientes puertos:
  - backempresa: localhost:8085
  - backweb: localhost:8090
  - backweb-2: localhost:8091
  
  
