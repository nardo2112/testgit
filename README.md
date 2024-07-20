## Funcionamiento del proyecto
El proyecto consiste en scrapear las carreras que aparecen en la lista de carreras de pregrado de la UNMSM. El termino scrapear significa, en palabras sencillas, extraer informacion de una pagina web, por ejemplo, si quisieras buscar una carrera dentro de las tantas que hay, podrias almacenarlas todas en un txt y luego aplicando ciertos filtros, reducir tu enorme lista a una que contenga solo carreras de ingenieria, o relacionadas al area de salud. Afortunadamente la pagina de San Marcos ya cuenta con la opcion de filtrar, pero en un caso hipotetico donde no lo haya, o la informacion sea demasiada y quieras aplicar filtros mas rigurosos, web scraping seria una muy buena alternativa. A continuacion explico el funcionamiento de mi codigo.

En la parte superior se colocan, similar a c++, las bibliotecas que se usaran y que permitiran el uso de ciertos comandos
```py

from time import sleep
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager #pip install webdriver-manager

```

La __primera linea__, me permite usar el comando sleep(t) donde t es el tiempo en que mi codigo hace una pausa para que la pagina termine de cargarse y los siguientes comandos puedan funcionar correctamente.
La __segunda linea__, importa webdriver, que es la herramienta que permite interactuar con el navegador.
La __tercera linea__, me permite usar el comando By con el uso mas adelante para buscar elementos en especifico, ya sea por su clase, ID, XPATH, entre otros.
La __cuarta y quinta linea__, son las mas importantes porque con ellas voy a designar a Chrome como el navegador que usare para realizar mi proyecto. Para ello uso las siguiente lineas de codigo:

```py

driver = webdriver.Chrome (
  service = Service(ChromeDriverManager().install())
)

```
Con ChromeDriverManager ya no tengo la necesidad de instalar manualmente el webdriver especifico para mi version de Chrome, porque ahora lo hace de forma automatica. Cabe mencionar que use Chrome porque pienso yo que al ser el navegador mas usado debe ser el que tenga mejor compatibilidad y presente menos fallas durante la ejecucion, desde mi punto de vista.

Las lineas de arriba sirvieron para configurar el entorno de trabajo a usar, las siguientes lineas son, por asi decirlo, las que realmente lo pondran en marcha.

Empiezo usando driver.get para especificar la url que se abrira en mi navegador.

```py

driver.get('https://unmsm.edu.pe/formacion-academica/carreras-de-pregrado')

```

Luego uso el comando sleep para darle tiempo a mi pagina a que vaya cargando antes de realizar los proximos comandos

```py

sleep(3)

carreras_list = []

```

Tambien de paso creo una lista en la cual almacenare las carreras que extraiga mas adelante.

usando el metodo de buscque por XPATH ubico a las etiquetas p con la clase 'mb-13px color-rojo-sm'. Dicha clase la obtuve inspeccionando en la pagina de San Marcos y note que ellas contenian el nombre de las carreras de pregrado. Al ser varios elementos con la misma clase uso el comando driver.find_elements, notese que esta en plural. Entonces guardo todas ellas en mi variable 'carreras', y con ayuda de un for almaceno una a una dentro mi lista creada anteriormente.

```py

  carreras = driver.find_elements(By.XPATH, '//p[@class="mb-13px color-rojo-sm"]')
  for carrera in carreras:
      carreras_list.append(carrera.text)
  sleep(2)

```

Uso sleep para esperar un poco antes de iniciar mi siguiente comando.

En vista que es una lista algo extensa, las carreras no fueron colocadas en una pagina, sino que la dividieron en 9 paginas, por lo que si queria seguir obteniendo las demas necesitaba moverme a la siguiente pagina. Entonces nuevamente use el comando driver.find_element para buscar el boton que me lleva a la siguiente pagina (resulto que no fue un boton, sino un hipervinculo), para ello nuevamente una inspeccion de la pagina y ubicar al responsable de dirigirme a la siguiente pagina.

![navigator](/proyect-web-scraping/images/navigator.png)

```py

  next = driver.find_element(By.XPATH, '//a[@class="d-inline-block sig-paginator"]')
  next.click()
  sleep(1)

```

Lo nuevo aqui fue que no solo era necesario ubicar al elemento, sino que tambien debia realizar un click para pasar de pagina, entonces una vez ubicado y guardado en mi variable 'next', uso el comando next.click() para realizar dicha accion.
En esta parte inverti regular tiempo porque no lograba hacer que ocurra, tengo costumbre de no borrar lineas de codigo fallidos porque puedo revisarlos mas adelante y recordar en que me equivoque, por eso en mi codigo original se puede apreciar algunos de los intentos que me tomo y muchos otros que borre que no aparecen en mi version final. 

![errores](/proyect-web-scraping/images/errores.png)

Ya casi por terminar, al notar que la accion iba a repetirse al menos 9 veces por la cantidad de paginas que tiene, decidi colocarlos en un bucle.

```py

for i in range(9):
    
    ...[codigo]...

```

Por ultimo, uso .join() para guardar los elementos de mi lista en strings y separarlos en cada linea los cuales guardo en mi variable 'final' y los exporto en un txt.

```py

sleep(1)
final = '\n'.join(carreras_list)

with open('output.txt', 'w') as file:
   file.write(final)

```











![Usuarios de C++](/assets/images/cpp-users.png)

*Java, Spirit, Windows 11, Dota, Google Chrome.*

Además, es bastante rápido.

![Tiempos de ejecución de programas en distintos lenguajes](/assets/images/cpp-time-comparison.png)

[*Fourment M, Gillings MR. A comparison of common programming languages used in bioinformatics. BMC Bioinformatics. 2008 Feb 5;9:82.*](https://doi.org/10.1186/1471-2105-9-82)

## ¿Qué es C++?

### Assembly
Primero meditemos sobre assembly:
- Instrucciones increíblemente simples (sumar, restar, mover bits).
- Un buen código en assembly es extremadamente veloz.

### C

K era su debilidad:
- No existían **objetos** ni **clases**.
- Difícil de escribir código que funcione **genéricamente**.
- **Tedioso** al escribir programas largos.

*Bjarne Stroustrup: Why I Created C++.*

Él quería un lenguaje que cumpla con lo siguiente:
- Rápido.
- Simple.
- Multiplatafotma.
- **Que tenga características de alto nivel**.

![Evolución de C++](/assets/images/cpp-evolution.png)
*Evolución de C++.*

Su filosofía era la siguiente:
- Solo añadir características si resuelven un problema real.

> Amortajadas las pupilas, traza su aullido pastoral un perro.


### Pero ... ¿qué es C++?
