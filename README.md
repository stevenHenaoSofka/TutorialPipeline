# TutorialPipeline
Este repositorio contiene un tutorial de como crear un pipeline en Jenkins para un proyecto tipo Maven y con análisis estático de código SonarQube


Tutorial para crear un pipeline CI en Jenkins con SonarQube y un proyecto Maven

Requisitos:
Instalar Docker previamente
Instalar Ngrok
Tener una imagen de Jenkins y SonarQube en Docker


1. Instalar plugins necesarios en Jenkins

  1.1 Ingresar Administrar jenkins → Administrar Plugins → Todos los Plugins.
      Buscar los siguientes plugins e instalarlos:
        - Maven Integration plugin
        - SonarQube Scanner for Jenkins


2. Añadir instalador para Sonar Scanner

  2.1 Ingresar Administrar Jenkins → Global Tool Configuration
  
  2.2 En el apartado de SonarQube Scanner dar click en el botón de instalaciones de sonar scanner → Añadir SonarQube Scanner


3. Generar Token en SonarQube

  3.1 Ingresar a SonarQube y realizar el login como administrador
  
  3.2 Click en Administration → security → users
  
  3.3 Click en token → ingresa nombre y Generate → Copiar el token
  
  3.4 Abrir Ngrok y usar el comando ngrok http (agregar la ruta donde esta sonarQube EJ: ngrok http 9000)  → Copiar la url que genera Ngrok


4. Agregar servidor de SonarQube en Jenkins

  4.1 Ingresar a Administrar Jenkins → Configurar el sistema 
  
  4.2 Click en add sonarQube → Name ponemos el mismo que pusimos en el paso 2.2 → URL del servidor colocamos la URL que nos generó Ngrok
  
  4.3 Damos click en agregar token  → seleccionamos secret text y agregamos el token
  

5. Crear webhook en sonarQube y Github
 
  5.1 Abrir Ngrok y usar el comando ngrok http (agregar la ruta donde esta Jenkins EJ: ngrok http 8080)  → Copiar la url que genera Ngrok
  
  5.2 Abrimos SonarQube  → Administration  → Webhooks
  
  5.3 Create → agregamos nombre y url (la url debe finalizar con /sonarqube-webhook/).
  
  5.4 Abrimos github y luego ingresamos al proyecto.
  
  5.5 Settings → Webhooks  → Addwebhook
  
  5.6 Agregamos la URL (debe terminar con /github-webhook/) → en Content Type colocamos la opción application/json → Agregamos los eventos que queramos que nos dispare el             webhook (push, commit, etc)



6. Creación del pipeline

  6.1 Abrimos jenkins → Nueva tarea → Elegimos la opción de Crear un proyecto Maven → Ok
  
  6.2 Configurar origen código fuente → Seleccionamos git → agregamos la URL del repositorio (no es la url que termina en .git) → agregan la rama del repositorio que se desea         ejecutar 
  
  
  6.3 Disparadores de ejecuciones → GitHub hook trigger for GITScm polling 
  
  6.4 Entorno de Ejecución → Prepare SonarQube Scanner environment → Seleccionamos nuestro token
  
  6.5 Proyecto → Fichero pom raíz: pom.xml → Goles y opciones: clean install sonar:sonar → guardar
  



Con esto ya se podrá ejecutar el pipeline
	





	-
