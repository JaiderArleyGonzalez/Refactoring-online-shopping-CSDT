# REFACTORING-online-shopping

## Sobre el proyecto...
El proyecto online-shopping es una aplicación de comercio electrónico desarrollada utilizando tecnologías como Spring Boot, Spring Web Flow, Spring Rest Services y Hibernate. También incluye Spring Security con configuración en Java y anotaciones.

## Code smells
### Funcionalidades Principales:
* Gestión de Usuarios: Permite a los usuarios registrarse, iniciar sesión y administrar su perfil.
* Gestión de Productos: Los usuarios pueden navegar por una lista de productos, ver detalles de los productos y agregar productos al carrito de compras.
* Carrito de Compras: Los usuarios pueden agregar productos al carrito, editar la cantidad de productos en el carrito y realizar el proceso de pago.
* Seguridad: Se implementa un sistema de seguridad basado en roles utilizando Spring Security, donde los usuarios pueden tener roles como ADMIN o USER.
### Long Method:
En varias partes del código, especialmente en las implementaciones de los servicios, se observan métodos largos que realizan múltiples tareas. Por ejemplo, en CartLineServiceImpl, CategoryServiceImpl y ProductServiceImpl, algunos métodos podrían dividirse en funciones más pequeñas y específicas para mejorar la legibilidad y facilitar el mantenimiento.

### Duplicate Code:
La falta de centralización de la lógica común puede llevar a la duplicación de código. Se observa que la validación de archivos de imagen se repite en la clase ProductValidator. Sería más efectivo refactorizar este código duplicado en un método compartido para evitar la repetición y facilitar futuras modificaciones.

### Middle Man:
En algunas clases, como en CartServiceImpl, parece que actúan como "middle man" o intermediarios, delegando la mayoría de las operaciones a otros componentes. Esto podría indicar una estructura excesivamente acoplada y una violación del principio de responsabilidad única. Se podría considerar la reestructuración de estas clases para eliminar la necesidad de un intermediario innecesario.

### Primitive Types:
En varias partes del código, se utilizan tipos primitivos para representar conceptos como números, identificadores, etc. Sería beneficioso crear clases pequeñas para encapsular estos conceptos y proporcionar un contexto de dominio más rico y expresivo.

### Uncommunicative Name:
Algunos métodos y variables pueden tener nombres poco comunicativos que no describen claramente su propósito. Por ejemplo, en métodos como saveCategory, findCategoryById, etc., los nombres podrían ser más descriptivos y seguir convenciones de nomenclatura más claras para mejorar la legibilidad del código.

### Falta de pruebas unitarias
El proyecto carece de pruebas unitarias, lo cual compromete la calidad y la estabilidad del código. Es fundamental implementar pruebas unitarias para garantizar el comportamiento esperado de las funciones y mejorar la confiabilidad del sistema.

## Técnicas de refactoring. ¿Qué se hará?

Se revisará la presencia de métodos largos y complejos en las clases de servicio y controladores para aplicar la técnica de refactorización **Long Method**. Se buscarán secciones de código que puedan dividirse en funciones más pequeñas y claras, mejorando así la legibilidad y mantenibilidad del código.

Para eliminar el **Duplicate Code**, se identificarán segmentos de código repetitivo en todo el proyecto y se reorganizarán en métodos o clases separadas para promover la reutilización y evitar la duplicación innecesaria de lógica, garantizando la coherencia y consistencia del sistema.

En cuanto al problema de **Feature Envy**, se examinarán cuidadosamente los métodos en las clases de servicio y los controladores para asegurarse de que cada método opere principalmente en los datos de su propia clase, evitando una dependencia excesiva de otras clases y promoviendo una mejor modularidad y cohesión en el código.

Para abordar el **Primitive Obsession**, se revisarán las clases que manejan tipos primitivos como identificadores enteros o cadenas, considerando envolver estos tipos en clases más descriptivas para mejorar la semántica y la mantenibilidad del código, asegurando así una representación más clara y precisa de los conceptos del dominio.

Finalmente, para resolver el **Lazy Class**, se analizarán las clases menos utilizadas y se evaluará si su funcionalidad podría fusionarse con otras clases relacionadas o si podrían eliminarse por completo sin afectar la funcionalidad del sistema, optimizando así la estructura y la claridad del código en el proyecto.

## Clean Code

### ¿En qué se está fallando?

**Principio "You Aren't Gonna Need It":**
La clase AddressServiceImpl presenta métodos como saveAddress() y findAddressByBilling() que podrían considerarse adicionales en este momento. A menos que haya una necesidad inmediata y clara de estas funcionalidades, se recomienda omitirlas hasta que su necesidad se vuelva evidente. Mantener el código libre de funciones no utilizadas ayuda a reducir la complejidad y a centrarse en las características esenciales del sistema.

**Principio "Keep it Simple, Stupid":**
Dentro del método findAllCategories() en CategoryServiceImpl, la lógica de filtrado de categorías activas podría simplificarse. Actualmente, el servicio recupera todas las categorías, luego itera sobre ellas para filtrar las inactivas. Este proceso podría optimizarse mediante consultas específicas en el repositorio de categorías, evitando así la necesidad de procesamiento adicional en el código.

Asimismo, en el método deleteCategory() de CategoryServiceImpl, en lugar de cargar la categoría y marcarla como inactiva, la eliminación directa podría ser una opción más simple y directa, reduciendo la complejidad del método.

**Principio "Don't Repeat Yourself":**
En la clase ProductServiceImpl, la lógica para filtrar productos inactivos se repite en varios métodos. Sería beneficioso encapsular esta lógica en un método privado dentro del servicio y llamar a este método desde los otros métodos que necesitan filtrar productos activos. Esto evita la repetición de código y mantiene la coherencia en la aplicación.

Del mismo modo, en el método findProductByCategoryId() de ProductServiceImpl, la lógica de filtrado de productos inactivos también se repite. Refactorizar esta lógica en un método privado ayudaría a mantener el código limpio y coherente, siguiendo el principio de no repetir el código.

### Principios de Programación que no se cumplen
**Principio de Responsabilidad Única (SRP - Single Responsibility Principle):**
UserService: La clase UserServiceImpl tiene la responsabilidad de codificar contraseñas y guardar usuarios en la base de datos. Esto podría considerarse una violación del SRP, ya que la codificación de contraseñas y la gestión de usuarios son responsabilidades separadas. Sería preferible tener un componente separado encargado específicamente de la codificación de contraseñas.

**Principio de Abierto/Cerrado (OCP - Open/Closed Principle):**
CategoryService: En la clase CategoryServiceImpl, el método findAllCategories() realiza una búsqueda de todas las categorías y luego filtra las inactivas. Este enfoque no sigue el principio de OCP, ya que la clase debería estar abierta para la extensión pero cerrada para la modificación. Sería más adecuado introducir un mecanismo para permitir la extensión sin necesidad de modificar la clase, como el uso de especificaciones o criterios de filtrado.

**Principio de Sustitución de Liskov (LSP - Liskov Substitution Principle):**
UserModel: Asumiendo que UserModel es una extensión de la clase User, si UserModel está siendo utilizado en lugar de User, debería ser capaz de sustituir a User en todos los lugares sin cambiar el comportamiento del sistema. Si UserModel tiene comportamientos o atributos adicionales que User no tiene, podría violar el principio de LSP.

**Principio de Inversión de Dependencia (DIP - Dependency Inversion Principle):**
Service Layer: En todo el código, las clases de servicio (ServiceImpl) están fuertemente acopladas a las clases de repositorio (Repository). Esto podría dificultar la sustitución de implementaciones de repositorio y violar el principio de DIP. Sería más deseable invertir las dependencias para que las clases de servicio dependan de abstracciones en lugar de implementaciones concretas.

### Prácticas XP para mejorar la calidad de código

**Integración Continua (Continuous Integration):**
Configurar un Sistema de Integración Continua (CI): Un sistema de CI como Jenkins o Travis CI puede ayudar a automatizar las pruebas y la construcción del proyecto cada vez que se realizan cambios en el repositorio de código. Esto ayuda a identificar rápidamente errores y problemas de integración, manteniendo el código en un estado siempre funcional.

**Refactorización Continua:**
Practicar la Refactorización Continua: Fomentar la mejora constante del diseño y la estructura del código sin cambiar su funcionalidad. Esto implica eliminar duplicaciones, simplificar lógica compleja y seguir patrones de diseño sólidos.

**Pruebas Unitarias:**
Escribir Pruebas Unitarias Automatizadas: Las pruebas unitarias garantizan que partes específicas del código funcionen como se espera. Es crucial escribir pruebas unitarias para cada método o función, y ejecutarlas automáticamente como parte del proceso de integración continua.

## Abordando Deuda Técnica
### Documentación de Pruebas Unitarias

Se ha agregado un conjunto de pruebas unitarias para garantizar la calidad del código y su correcto funcionamiento. Anteriormente, el proyecto carecía de pruebas, excepto por una única prueba documentada. Se decidió eliminar esa prueba y crear nuevas pruebas para diferentes aspectos del sistema.

#### Tema Abordado
El tema abordado en este conjunto de pruebas unitarias es la funcionalidad de las clases del paquete `com.isolutions4u.onlineshopping.model`. Estas clases son fundamentales para el funcionamiento del sistema de compras en línea y, por lo tanto, es crucial asegurarse de que estén implementadas correctamente.

#### Pruebas Propuestas
A continuación, se detallan las pruebas unitarias propuestas:

##### 1. AddressTest
Pruebas para la clase `Address`, que incluyen la configuración y obtención de diferentes atributos de direcciones, como la línea de dirección, ciudad, país, etc.

##### 2. CartLineTest
Pruebas para la clase `CartLine`, que se centran en verificar la correcta configuración y obtención de los atributos de las líneas de carrito, como el ID, el total, la cantidad de productos, etc.

##### 3. CartTest
Pruebas para la clase `Cart`, que examinan la funcionalidad relacionada con el carrito de compras, como la configuración del ID del carrito, el gran total, el número de líneas de carrito, etc.

##### 4. CategoryTest
Pruebas para la clase `Category`, que se concentran en verificar la configuración y obtención de atributos como el ID de la categoría, el nombre, la descripción, etc.

##### 5. ProductTest
Pruebas para la clase `Product`, que abordan la funcionalidad relacionada con los productos, incluida la configuración y obtención de atributos como el ID del producto, el nombre, la descripción, etc.

##### 6. RegisterModelTest
Pruebas para la clase `RegisterModel`, que se enfocan en verificar la correcta configuración de la dirección de facturación asociada a un modelo de registro de usuario.

##### 7. UserModelTest
Pruebas para la clase `UserModel`, que examinan la configuración y obtención de atributos como el ID de usuario, el nombre completo, el correo electrónico, el rol, etc.

##### 8. UserTest
Pruebas para la clase `User`, que se centran en verificar la funcionalidad relacionada con la configuración y obtención de atributos de los usuarios, como el primer nombre, el apellido, el correo electrónico, etc.

Todas estas pruebas las podrás encontrar en el directorio: `online-shopping\src\test\java\com\isolutions4u\onlineshopping\test`
### Ejecución de las pruebas
Para poder ejecutar las pruebas, se ingresa el siguiente comando bajo el directorio online-shopping
```
    mvn test
```

Deberá obtener lo siguiente:

![](/img/Test.png)

![](/img/fin.png)

En total se realizaron 50 pruebas unitarias.

### Conclusión
Las pruebas unitarias propuestas cubren una amplia gama de funcionalidades en el proyecto de compras en línea. Al asegurar que estas clases estén correctamente implementadas, se aumenta la confiabilidad y la estabilidad del sistema, lo que resulta en una mejor experiencia para los usuarios finales.

## Análisis con Herramienta Sonarqube

SonarQube es una plataforma de código abierto que evalúa la calidad del código fuente. Realiza análisis estáticos para identificar bugs, vulnerabilidades, código duplicado y malas prácticas. Ofrece métricas, tableros de control y gestión de deuda técnica. Es altamente personalizable y compatible con una amplia gama de lenguajes y herramientas de desarrollo.

### Instalación

Existen dos formas de realizar la instalación, una que es con el [zip](https://www.sonarsource.com/products/sonarqube/downloads/success-download-community-edition/) y la otra que es mediante una imagen de Docker, la cual considero que es más sencilla.

Iniciamos corriendo el Docker Desktop e ingresamos en el cmd el siguiente comando:

```
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest
```

El comando docker run se utiliza para crear y ejecutar un contenedor Docker a partir de una imagen específica. Aquí está el desglose del comando:

- docker run: Inicia la ejecución de un contenedor Docker.
- -d: Ejecuta el contenedor en segundo plano (modo daemon).
- --name sonarqube: Asigna el nombre "sonarqube" al contenedor.
- -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true: Configura la variable de entorno SONAR_ES_BOOTSTRAP_CHECKS_DISABLE como true. Esta variable se utiliza para deshabilitar las comprobaciones de inicio de Elasticsearch en SonarQube.
- -p 9000:9000: Mapea el puerto 9000 del host al puerto 9000 del contenedor. Esto permite acceder a SonarQube a través del puerto 9000 del host.
- sonarqube:latest: Especifica la imagen de SonarQube a utilizar para crear el contenedor. En este caso, se utiliza la etiqueta "latest", lo que significa que se utilizará la última versión disponible de la imagen "sonarqube".

![](/img/comando.png)

Podemos comprobar que el contenedor se creó correctamente aquí:

![](/img/contenedor.png)


Una vez que tu instancia esté en funcionamiento, inicia sesión en [http://localhost:9000](http://localhost:9000) utilizando las credenciales del Administrador del Sistema:

* Usuario: admin
* Contraseña: admin

Ya luego podrás cambiar la contraseña a la que desees.

![](/img/login.png)

En este caso el proyecto que yo voy a crear será local

![](/img/local.png)

seguimos los siguientes pasos:

![](/img/pasos.png)

Creamos el yaml .github/workflows/build.yml

y añadimos lo siguiente:
```
name: Build

on:
  push:
    branches:
      - main


jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    permissions: read-all
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0  # Shallow clones should be disabled for a better relevancy of analysis
      - name: Set up JDK 17
        uses: actions/setup-java@v1
        with:
          java-version: 17
      - name: Cache SonarQube packages
        uses: actions/cache@v1
        with:
          path: ~/.sonar/cache
          key: ${{ runner.os }}-sonar
          restore-keys: ${{ runner.os }}-sonar
      - name: Cache Maven packages
        uses: actions/cache@v1
        with:
          path: ~/.m2
          key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
          restore-keys: ${{ runner.os }}-m2
      - name: Build and analyze
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
        run: mvn -B verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=online-shopping-csdt -Dsonar.projectName='online-shopping-csdt'
```
### ¿Qué podemos ver ahora?



#### 1. Análisis del Código Fuente:
SonarQube realiza un análisis exhaustivo del código fuente de una aplicación en múltiples lenguajes de programación, incluidos Java, JavaScript, Python, C#, entre otros. Examina aspectos como la complejidad del código, las violaciones de estándares de codificación, la duplicación de código y la cobertura de pruebas.

#### 2. Detección de Problemas de Calidad y Seguridad:
Identifica y clasifica una amplia variedad de problemas potenciales en el código, como vulnerabilidades de seguridad, errores de programación, malas prácticas de codificación y posibles cuellos de botella de rendimiento. Estos problemas se presentan en un tablero de control intuitivo que proporciona una visión general de la salud del proyecto.

#### 3. Métricas de Calidad del Código:
SonarQube genera métricas detalladas sobre la calidad del código, como el índice de mantenibilidad, la complejidad ciclomática, la duplicación de código y la cobertura de pruebas. Estas métricas ayudan a los equipos a comprender mejor la estructura y la calidad del código base.

#### 4. Seguimiento de Tendencias y Evolución del Código:
Permite a los equipos realizar un seguimiento de la evolución del código a lo largo del tiempo mediante gráficos y visualizaciones que muestran la mejora o degradación de la calidad del código en diferentes versiones del software. Esto facilita la identificación de áreas problemáticas y la evaluación del impacto de los cambios realizados en el código.

#### 5. Integración con Herramientas de Desarrollo:
SonarQube se integra fácilmente con herramientas de desarrollo y sistemas de integración continua (CI) como Jenkins, GitLab CI, GitHub Actions, entre otros. Esto permite la ejecución automatizada de análisis de código en cada confirmación de código y proporciona retroalimentación instantánea a los desarrolladores.

#### 6. Personalización y Configuración Flexible:
Los usuarios pueden personalizar y configurar las reglas de análisis de código según las necesidades específicas del proyecto y del equipo. Esto permite adaptar SonarQube a los estándares de codificación y políticas de calidad internas de la organización.

#### 7. Informes Detallados y Documentación:
SonarQube genera informes detallados que contienen resultados de análisis de código, métricas de calidad y recomendaciones de mejora. Estos informes pueden ser compartidos con el equipo de desarrollo, los líderes del proyecto y otras partes interesadas para facilitar la toma de decisiones informadas.

#### 8. Mejora Continua del Software:
Al proporcionar una visión integral de la calidad del código y las áreas problemáticas, SonarQube ayuda a los equipos de desarrollo a priorizar y abordar los problemas de manera proactiva. Esto fomenta una cultura de mejora continua del software y contribuye a la entrega de aplicaciones más seguras, confiables y eficientes.

## Implementación con Copilot

Copilot es una herramienta de programación asistida por inteligencia artificial desarrollada por GitHub. Su propósito principal es ayudar a los desarrolladores a escribir código de manera más eficiente y productiva mediante la generación automática de sugerencias de código basadas en el contexto y el estilo de programación. En el proyecto de Refactorización de Compras en Línea, se ha utilizado Copilot para la implementación de pruebas unitarias y la refactorización del código existente.

#### Propósito

El propósito de utilizar Copilot en la implementación de pruebas unitarias y refactorización es agilizar el proceso de desarrollo, reducir la carga cognitiva de los desarrolladores y mejorar la calidad del código generado. Copilot analiza el código existente, comprende su funcionalidad y contexto, y genera automáticamente sugerencias de código que cumplen con los requisitos específicos del proyecto.

#### Logros

Al utilizar Copilot, se han logrado varios beneficios significativos en el proyecto:

- **Aumento de la productividad:** Copilot acelera el proceso de escritura de código al proporcionar sugerencias precisas y relevantes en tiempo real. Esto permite a los desarrolladores escribir pruebas unitarias y realizar refactorizaciones de manera más rápida y eficiente.

- **Mejora de la calidad del código:** Las sugerencias generadas por Copilot están diseñadas para seguir las mejores prácticas de codificación y cumplir con los estándares de calidad del proyecto. Esto contribuye a mejorar la legibilidad, mantenibilidad y robustez del código en general.

- **Reducción de errores:** Al generar automáticamente código basado en el contexto y la semántica del proyecto, Copilot ayuda a reducir la probabilidad de introducir errores humanos durante la implementación de pruebas unitarias y refactorizaciones.

- **Facilitación del aprendizaje:** Copilot proporciona una oportunidad para que los desarrolladores aprendan nuevas técnicas y enfoques de codificación al observar las sugerencias generadas y comprender cómo se aplican en el contexto del proyecto.

La utilización de Copilot en la implementación de pruebas unitarias y refactorización ha sido fundamental para mejorar la eficiencia, calidad y experiencia general de desarrollo en el proyecto de Refactorización de Compras en Línea.

## Desarrollo de Experiencia (DevEx) en el Proyecto

El Desarrollo de Experiencia (DevEx) se refiere al enfoque centrado en el desarrollador para mejorar la productividad, la eficiencia y la satisfacción en el proceso de desarrollo de software. En el contexto del proyecto de refactorización de compras en línea, la experiencia del desarrollador se ha mejorado mediante la implementación de diversas prácticas y herramientas para optimizar el desarrollo y mantenimiento del código.

### Puntos Positivos:

1. **Mejora de la calidad del código:** La introducción de pruebas unitarias, la refactorización del código para abordar code smells y la integración de SonarQube han contribuido a mejorar la calidad general del código, lo que resulta en un software más robusto y menos propenso a errores.

2. **Cumplimiento de estándares de codificación:** La aplicación de prácticas como la refactorización continua y la integración de SonarQube ha ayudado a garantizar que el código siga los estándares de codificación establecidos, promoviendo una mayor coherencia y mantenibilidad a largo plazo.

3. **Reducción de la deuda técnica:** La identificación y abordaje de problemas de calidad del código, junto con la introducción de pruebas unitarias, han contribuido a reducir la deuda técnica acumulada en el proyecto, mejorando así su estabilidad y escalabilidad futura.

4. **Mayor confiabilidad y estabilidad del sistema:** La incorporación de pruebas unitarias automatizadas y la integración de SonarQube han aumentado la confianza en la estabilidad del sistema al identificar y corregir problemas potenciales antes de que afecten a la producción.

5. **Facilitación del mantenimiento del código:** La refactorización del código para abordar code smells y la mejora de la estructura del código han facilitado el mantenimiento continuo del sistema al hacer que el código sea más comprensible y fácil de modificar.

### Puntos Negativos:

1. **Necesidad de un mayor enfoque en los principios SOLID:** Aunque se han realizado mejoras significativas en la calidad del código, aún hay oportunidades para aplicar más rigurosamente los principios SOLID, especialmente en áreas como la responsabilidad única y la inversión de dependencia.

2. **Complejidad adicional en la configuración de herramientas:** La introducción de herramientas como SonarQube y la configuración de integración continua puede agregar una capa adicional de complejidad al proceso de desarrollo, lo que puede requerir más tiempo y recursos para administrar y mantener.

Estos puntos reflejan cómo las acciones tomadas en el proyecto han impactado la experiencia del desarrollador, destacando tanto los aspectos positivos como las áreas que podrían necesitar más atención o mejora.

## SpaceFramework
El modelo SPACE (Satisfaction and well-being, Performance, Activity, Communication and collaboration, Efficiency and Flow) es un marco de evaluación utilizado para medir y mejorar diversos aspectos del entorno laboral y el rendimiento de los equipos de desarrollo de software. Este modelo se centra en cinco áreas clave que impactan en la productividad y el bienestar de los desarrolladores: satisfacción y bienestar, rendimiento, actividad, comunicación y colaboración, y eficiencia y flujo.

Cada una de estas áreas se evalúa utilizando métricas específicas diseñadas para proporcionar una comprensión completa del estado y la salud del equipo de desarrollo. Estas métricas pueden incluir desde encuestas de satisfacción de empleados (eNPS) y métricas de rendimiento (eficacia, burnout) hasta mediciones de actividad (codificación, diseño), comunicación y colaboración (calidad de revisiones, integración de trabajo) y eficiencia (interrupciones, lead time).

Al utilizar el modelo SPACE, las organizaciones pueden identificar áreas de mejora y tomar medidas concretas para optimizar el entorno de trabajo y el proceso de desarrollo de software. Esto puede incluir iniciativas para mejorar la comunicación entre equipos, reducir el tiempo de espera entre etapas del proceso de desarrollo, fomentar la colaboración efectiva y promover un equilibrio saludable entre el trabajo y el bienestar personal de los desarrolladores. En última instancia, el objetivo del modelo SPACE es crear un entorno laboral que fomente la productividad, la satisfacción y el éxito a largo plazo tanto para los desarrolladores como para la organización en su conjunto.

## Análisis del Modelo SPACE
Dentro del proyecto de Refactorización de Compras en Línea, varios puntos se verían influenciados por la aplicación del framework SPACE:

**Satisfacción y Bienestar:** La introducción de mejoras en la calidad del código, la implementación de prácticas como la refactorización continua y la integración de herramientas como SonarQube contribuyen a crear un entorno laboral más satisfactorio y gratificante para los desarrolladores. Al mejorar la calidad del código y reducir la deuda técnica, se disminuye la frustración y el estrés asociado con el mantenimiento de un código defectuoso.

**Rendimiento:** La adopción de pruebas unitarias automatizadas y la integración de herramientas de análisis estático como SonarQube ayudan a mejorar el rendimiento del equipo al identificar y corregir problemas de código de manera más eficiente. Esto conduce a una mayor eficacia en el desarrollo de software y a una entrega más rápida de funcionalidades.

**Comunicación y Colaboración:** Al promover la integración continua y la revisión de código, se fomenta una comunicación más efectiva y una colaboración más estrecha entre los miembros del equipo. Esto facilita la identificación temprana de problemas y la resolución colaborativa de desafíos técnicos.

**Eficiencia y Flujo:** La implementación de prácticas como la refactorización continua y la automatización de pruebas ayuda a mejorar la eficiencia del proceso de desarrollo, reduciendo el tiempo dedicado a actividades manuales y propensas a errores. Esto permite que el equipo se centre en agregar valor al producto y mantener un flujo de trabajo constante y productivo.

Hasta el momento, el proyecto ha aplicado el framework SPACE principalmente a través de la mejora de la calidad del código y la adopción de prácticas de desarrollo centradas en el bienestar del equipo y la eficiencia del proceso. Se han introducido pruebas unitarias, se ha realizado refactorización continua y se ha integrado la herramienta SonarQube para analizar y mejorar la calidad del código. Estas acciones han contribuido a crear un entorno de trabajo más satisfactorio, mejorar el rendimiento del equipo y promover una comunicación y colaboración más efectivas.

