********Python, instalación to run our project from the PC:
(cd lab2_template)



** 1. Instalar Python:

* Instalación de Python in PC: Al momento de instalar, se debe optar por la opciones recomendada y marcar las 2 opciones al pie de la ventana (admin y Path)
Si al consultar la versión instalado por consola, no se genera respuesta, entonces se tendrá que configurar la ruta a través de entornos del sistema. 
Guía a Youtube: Python / Pip no se reconoce como comando interno o externo...

-Validar versión de Python: python --version ó py --version
-Validar versión de pip (que es instalada al instalar Python): pip --version

* pip (Pip Installs Packages): Es un sistema de gestión de paquetes utilizado para instalar y administrar bibliotecas y 
paquetes de software escritos en Python.
Es una herramienta esencial para administrar dependencias y bibliotecas en proyectos de Python y
es ampliamente utilizada en la comunidad de desarrollo de Python.


1.1 Install Python extension for Visual Studio Code by Microsoft (Windows).

** 2. Configura un entorno virtual (opcional pero recomendado):

Es una buena práctica crear un entorno virtual para tu proyecto de Django. Un entorno virtual te permite aislar las
dependencias de tu proyecto y evitar conflictos con otras aplicaciones de Python en tu sistema. Puedes crear un
entorno virtual con la siguiente línea de comando:

python -m venv myenv     //"myenv" es personalizado y creará una carpeta con este nombre dentro del proyecto.

** 3/3. Activating virtual environment:

* Crear el folder del virtual environment inside the project folder. *:

0. El entorno virtual requiere de la habilitación de scrips locales. Desde Windows/Power Shell ejecutar (para habilitación temporal): 
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass

(Aceptar con la opción "S" (Sí)).


1. Terminal:
python -m venv .venv

(You can also use `py -3 -m venv .venv`)

If we want to create to a specific folder (from a folder more high), then:
(promp/high folder)> python -m .\my_insite_folder\

2. Accessing the path with (...).venv\Scripts>

C:"folder project"\.venv\Scripts> 


3/3. Activate (.\activate)
C:\Users\USER\Desktop\Test\.venv\Scripts>.\activate

Una vez ejecutado, se puede ver al principio del promp "(.venv)" indicando que ya estamos en el entorno virtual.

*****
Extras:

** 4. Run project:

1. Set up the repository path:
cd "folder project"/server (Here is "requirements.txt").

1.1

2. Intall all dependencies from project (If it's not have already yet):
python3 -m pip install -U -r requirements.txt

3. Run server:
python manage.py runserver

(En theia Lab is: python3 manage.py runserver)

4. The server will lounch in:
http://127.0.0.1:8000
NOTE: We need to verify that "127.0.0.1" is iclured in .../djangobackend/settings.py: ALLOWED_HOSTS = ['127.0.0.1', "localhost", ]


4.1 We just need to add '/djangoapp/'
http://127.0.0.1:8000/djangoapp/
NOTE: We need to verify that the path "/djangoapp" is inclured in .../djangobackend/urls.py


NOTA: re-runing local project Django after all prerequisite are done:

1/1. Enter to virtual enviromment:
"folder project"\.venv\Scripts>
"folder project"\.venv\Scripts>.\activate

2/2. Run de server:
 python manage.py runserver


*****(Fin)End of "starting project of Django (Python) from local environment".


Verificar si el puerto 8000 está en uso desde CMD: netstat -ano | findstr "8000"
Si no ves ninguna salida, significa que el puerto 8000 está libre. 


******Lab. Práctica con PostgreSQL / Local con Django:

0.. Instalar PostgreSQL en el PC.

PostgreSQL / Shell:
Username: postgres (por defecto)
Password super usuario: Perseverar  //Ingresado durante la instalación.
Puerto: 5432

* Consultar las bases de datos existentes:
\l

* CREATE DATABASE postgres

* Ingresar a una base de datos called 'postgres': ('postgres' es un nombre aleatorio)
\c postgres

* Validar la información de la base de datos called 'postgres' (después de ingresar):
\d

Debe aparecer vacía, lo que significa que debe ser estructurada (modelo) y cargada con datos.
Para estructurarla desde Django:

1. Se especifica la base de datos a utilizar con los parámetros requeridos en settings.py, en nuestro caso postgresql:

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'Perseverar',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}


2. En models.py modelamos la base de datos creada previamente en Postgres (es decir, creamos los modelos)

from django.db import models (Ejemplo).

class myModelo(models.Model):
    Name = models.CharField(max_length=100)
    Description= models.CharField(blank=True)

3. Instalar dependencia para comunicar Django con Postgree. Ejecutar desde la terminal:

("project root")> pip install psycopg (ó pip install psycopg2)

4. Ejecutar el comando makemigrations para generar un archivo de migración que describa los cambios propuestos (en models.py) 
para aplicar a la base de datos creada llamada 'mydb':	

("project root(lab2_template)")>  python manage.py makemigrations crud (ó python manage.py makemigrations)

5/5. Migrar/cargar los modelos a la base de datos:  
python manage.py migrate

NOTA: Es posible que al reiniciar el computador, sea necesario volver a ejecutar el punto 3, 4 y 5 si al consultar PostgreSQL
no aparecen cargadas las migraciones (modelos). Cada vez se se haga un cambio a los modelos y luego de ejecutar "manage.py makemigrations crud" 
en la terminal aparecerá algo como:

Migrations for 'crud':
  crud/migrations/0001_initial.py


NOTA: Tener encuenta que esto crea "tablas/modelos" así que al consultar en PostgreSQL:
\d
Se están consultando todas las tablas (no la base de datos). Para consultar la base de datos específica creada y ver sus tablas:
1. \c mydb //ingresa a la base de datos.
2. \dt     //muestra las tablas/modelos para la base de datos ingresada.

En el contexto de Django, las "migraciones" se refieren a un mecanismo que permite gestionar y realizar cambios en la estructura de 
la base de datos de una aplicación web de manera controlada y automática. Las migraciones son esenciales para mantener la consistencia
 entre el modelo de datos de tu aplicación (definido en tus modelos de Django) y la base de datos subyacente.

Cuando trabajas con Django, defines la estructura de tus modelos en Python utilizando clases de Django que heredan de models.Model. 
Cada vez que haces cambios en estos modelos, como agregar un nuevo campo, eliminar un campo o realizar otras modificaciones, 
Django te permite crear migraciones para reflejar esos cambios en la base de datos. Las migraciones son archivos de Python que 
contienen instrucciones para modificar la estructura de la base de datos de acuerdo con los cambios realizados en los modelos.

Para trabajar con migraciones en Django, generalmente sigues estos pasos:

Realizas cambios en tus modelos.
1. Ejecutas el comando makemigrations para generar un archivo de migración que describa los cambios propuestos (detallado arriba).
2. Ejecutas el comando migrate para aplicar las migraciones pendientes a la base de datos, lo que actualiza la estructura de la base 
de datos de acuerdo con los cambios definidos en las migraciones (detallado arriba).
Las migraciones son una parte fundamental de la administración de bases de datos en Django, ya que aseguran que la estructura de la base de datos esté en sincronía con la estructura de tus modelos, lo que facilita la evolución de tu aplicación a medida que se desarrolla y se realizan cambios en el esquema de la base de datos.

6.* Consultar las bases de datos existentes:
\l

7. * Ingresar a la base de datos (creada):
\c postgres

8. * Listado de relaciones y tablas(modelos ya cargados desde Django).

postgres=# \c postgres
Ahora está conectado a la base de datos «postgres» con el usuario «postgres».
postgres=# \d

Ejemplo:
                      Listado de relaciones
 Esquema |             Nombre             |   Tipo    |  Due±o
---------+--------------------------------+-----------+----------
 public  | crud_course                    | tabla     | postgres
 public  | crud_course_id_seq             | secuencia | postgres
 public  | crud_course_instructors        | tabla     | postgres
 public  | crud_course_instructors_id_seq | secuencia | postgres
 public  | crud_enrollment                | tabla     | postgres
 public  | crud_enrollment_id_seq         | secuencia | postgres
 public  | crud_instructor                | tabla     | postgres
 public  | crud_learner                   | tabla     | postgres
 public  | crud_lesson                    | tabla     | postgres
 public  | crud_lesson_id_seq             | secuencia | postgres
 public  | crud_user                      | tabla     | postgres
 public  | crud_user_id_seq               | secuencia | postgres
 public  | django_migrations              | tabla     | postgres
 public  | django_migrations_id_seq       | secuencia | postgres
(14 filas)

8.1 Muestra solo las tablas/modelos.
\dt 

postgres=# \dt
                Listado de relaciones
 Esquema |         Nombre          | Tipo  |  Due±o
---------+-------------------------+-------+----------
 public  | crud_course             | tabla | postgres
 public  | crud_course_instructors | tabla | postgres
 public  | crud_enrollment         | tabla | postgres
 public  | crud_instructor         | tabla | postgres
 public  | crud_learner            | tabla | postgres
 public  | crud_lesson             | tabla | postgres
 public  | crud_user               | tabla | postgres
 public  | django_migrations       | tabla | postgres
(8 filas)

*******************************
# ...El próximo paso es realizar operaciones CRUD en estas tablas, las cuales continúan en el archivo crud/write.py del proyecto lab2_tamplate:

# Django specific settings
import inspect
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "settings")
from django.db import connection
# Ensure settings are read
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()

from crud.models import *
from datetime import date


# Your code starts from here:

# Open write.py and append a write_instructors() method to save some instructors objects.
# For the instructor_john, we first create his parent class model user and update instructor_john.user to be user_john.
# For other instructors, Django will automatically assign values of first_name, last_name, dob to their parent user objects.

def write_instructors():
    # Add instructors
    # Create a user
    user_john = User(first_name='John', last_name='Doe', dob=date(1962, 7, 16))
    user_john.save()
    instructor_john = Instructor(full_time=True, total_learners=30050)
    # Update the user reference of instructor_john to be user_john
    instructor_john.user = user_john
    instructor_john.save()
    
    instructor_yan = Instructor(first_name='Yan', last_name='Luo', dob=date(1962, 7, 16), full_time=True, total_learners=30050)
    instructor_yan.save()

    instructor_joy = Instructor(first_name='Joy', last_name='Li', dob=date(1992, 1, 2), full_time=False, total_learners=10040)
    instructor_joy.save()
    instructor_peter = Instructor(first_name='Peter', last_name='Chen', dob=date(1982, 5, 2), full_time=True, total_learners=2002)
    instructor_peter.save()
    print("Instructor objects all saved... ")


# Append a write_courses() method to add some course objects.
def write_courses():
    # Add Courses
    course_cloud_app = Course(name="Cloud Application Development with Database",
                              description="Develop and deploy application on cloud")
    course_cloud_app.save()
    course_python = Course(name="Introduction to Python",
                           description="Learn core concepts of Python and obtain hands-on "
                                       "experience via a capstone project")
    course_python.save()

    print("Course objects all saved... ")



# Append a write_lessons() method to add some lessons
def write_lessons():
    # Add lessons
    lession1 = Lesson(title='Lesson 1', content="Object-relational mapping project")
    lession1.save()
    lession2 = Lesson(title='Lesson 2', content="Django full stack project")
    lession2.save()
    print("Lesson objects all saved... ")



# Following code snippet write_learners() method by saving more learners objects into database:
def write_learners():
    # Add Learners
    learner_james = Learner(first_name='James', last_name='Smith', dob=date(1982, 7, 16),
                            occupation='data_scientist',
                            social_link='https://www.linkedin.com/james/')
    learner_james.save()
    learner_mary = Learner(first_name='Mary', last_name='Smith', dob=date(1991, 6, 12), occupation='dba',
                           social_link='https://www.facebook.com/mary/')
    learner_mary.save()
    learner_robert = Learner(first_name='Robert', last_name='Lee', dob=date(1999, 1, 2), occupation='student',
                             social_link='https://www.facebook.com/robert/')
    learner_robert.save()
    learner_david = Learner(first_name='David', last_name='Smith', dob=date(1983, 7, 16),
                            occupation='developer',
                            social_link='https://www.linkedin.com/david/')
    learner_david.save()
    learner_john = Learner(first_name='John', last_name='Smith', dob=date(1986, 3, 16),
                           occupation='developer',
                           social_link='https://www.linkedin.com/john/')
    learner_john.save()

    learner_BeBono = Learner(first_name='Be', last_name='Bono', dob=date(1980, 10, 24),
                           occupation='developer',
                           social_link='https://www.linkedin.com/bebono/')
    learner_BeBono.save()

    print("Learner objects all saved... ")




# To conveniently clean up your database tables, you can add a clean_data() method like the following code snippet.
# It uses the model manager objects to get all objects first and then delete them from database.
def clean_data():
    # Delete all data to start from fresh
    Enrollment.objects.all().delete()
    User.objects.all().delete()
    Learner.objects.all().delete()
    Instructor.objects.all().delete()
    Course.objects.all().delete()
    Lesson.objects.all().delete()


# Next, let's call those populating methods to actually save the objects
# Append the following methods call to write.py

# Clean any existing data first y llamar todos los métodos de escritura para actualizar la base de datos:
clean_data()
write_courses()
write_instructors()
write_lessons()
write_learners()


# At last, run the write.py in terminal
# python write.py (ó python write.py)



Output:

(djangoenv) PS C:\Users\USER\Desktop\Project\lab2_template> python write.py
Course objects all saved... 
Instructor objects all saved... 
Lesson objects all saved... 
Learner objects all saved...

*******************************

Then, validating the data changed from PostgreeSQL Shell:

* Para ver la estructura de una tabla en particular, incluyendo columnas, tipos de datos y restricciones:
\d nombre_de_tabla

* Ver el contenido de una tabla en PostgreSQL (* signica todas las columnas):
SELECT * FROM crud_course; 

Ejemplo: 

postgres=# SELECT * FROM crud_course;
 id |                    name                     |                                     description                     
----+---------------------------------------------+-------------------------------------------------------------------------------------
  1 | Cloud Application Development with Database | Develop and deploy application on cloud
  2 | Introduction to Python                      | Learn core concepts of Python and obtain hands-on experience via a capstone project
(2 filas)

*********************************************

****Query objects from Django (read_courses.py):

#Find all courses
# * We call the model managers objects to return us all the courses.

courses = Course.objects.all()
print(courses)

# * Run the Python script file to test the result in Terminal: 
python read_courses.py (ó python3 read_courses.py)

Output:

(djangoenv) PS C:\Users\USER\Desktop\Project\lab2_template> python read_courses.py 
<QuerySet [<Course: Name: Cloud Application Development with Database,Description: Develop and deploy application on cloud>, <Course: Name: Introduction to Python,Description: Learn core concepts of Python and obtain hands-on experience via a capstone project>]>


****Query objects from Django - with Filter (read_instructor.py):

# Django specific settings
import inspect
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "settings")
from django.db import connection
# Ensure settings are read
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()

from crud.models import *
from datetime import date


# Your code starts from here:

# Next, let's query instructors with filters to select subsets of instructors meeting following criterions:
# Find a single instructor with first name Yan
# Try to find a non-existing instructor with first name Andy
# Find all part time instructors
# Find all full time instructors with First Name starts with Y and learners count greater than 30000 (with Instructor.objects.exclude)
# Find all full time instructors with First Name starts with Y and learners count greater than 30000
# using multiple parameters (with Instructor.objects.filter)

# Open read_instructor.py, and add the following code snippets to perform queries:

instructor_yan = Instructor.objects.get(first_name="Yan")
print("1. Find a single instructor with first name `Yan`")
print(instructor_yan)

print("\n")
# Note that there is no instructor with first name `Andy`
# So the manager will throw an exception
try:
    instructor_andy = Instructor.objects.get(first_name="Andy")
except Instructor.DoesNotExist:
    print("2. Try to find a non-existing instructor with first name `Andy`")
    print("Instructor Andy doesn't exist")

print("\n")
part_time_instructors = Instructor.objects.filter(full_time=False)
print("3. Find all part time instructors: ")
print(part_time_instructors)

print("\n")
full_time_instructors = Instructor.objects.exclude(full_time=False).filter(total_learners__gt=30000).\
        filter(first_name__startswith='Y')
print("4. Find all full time instructors with First Name starts with `Y` and learners count greater than 30000")
print(full_time_instructors)

print("\n")
full_time_instructors = Instructor.objects.filter(full_time=True, total_learners__gt=30000,
                                                      first_name__startswith='Y')
print("5. Find all full time instructors with First Name starts with `Y` and learners count greater than 30000")
print(full_time_instructors)


# Run read_instructors.py in the terminal
# python read_instructors.py (ó python3 read_instructors.py)

# Terminal OUTPUT:

# (djangoenv) PS C:\Users\USER\Desktop\Project\lab2_template> python read_instructors.py
# 1. Find a single instructor with first name `Yan`
# First name: Yan, Last name: Luo, Is full time: True, Total Learners: 30050


# 2. Try to find a non-existing instructor with first name `Andy`
# Instructor Andy doesn't exist


# 3. Find all part time instructors:
# <QuerySet [<Instructor: First name: Joy, Last name: Li, Is full time: False, Total Learners: 10040>]>


# 4. Find all full time instructors with First Name starts with `Y` and learners count greater than 30000
# <QuerySet [<Instructor: First name: Yan, Last name: Luo, Is full time: True, Total Learners: 30050>]>


# 5. Find all full time instructors with First Name starts with `Y` and learners count greater than 30000
# <QuerySet [<Instructor: First name: Yan, Last name: Luo, Is full time: True, Total Learners: 30050>]>
# (djangoenv) PS C:\Users\USER\Desktop\Project\lab2_template>


**********************Query Learners with - Filters / read_learners.py

 # Find students with last name "Smith"
learners_smith = Learner.objects.filter(last_name="Smith")
print("1. Find learners with last name `Smith`:")
print(learners_smith)
print("\n")
# Order by dob descending, and select the first two objects
learners = Learner.objects.order_by('-dob')[0:2]
print("2. Find top two youngest learners:")
print(learners)


***Run the read_learners.py file: 
python read_learners.py

Terminal OUTPUT:

1. Find learners with last name `Smith`
<QuerySet [<Learner: First name: James, Last name: Smith, Date of Birth: 1982-07-16, Occupation: data_scientist, Social Link: https://www.linkedin.com/james/>, 
<Learner: First name: Mary, Last name: Smith, Date of Birth: 1991-06-12, Occupation: dba, Social Link: https://www.facebook.com/mary/>, 
<Learner: First name: David, Last name: Smith, Date of Birth: 1983-07-16, Occupation: developer, Social Link: https://www.linkedin.com/david/>, 
<Learner: First name: John, Last name: Smith, Date of Birth: 1986-03-16, Occupation: developer, Social Link: https://www.linkedin.com/john/>]>

2. Find top two youngest learners
<QuerySet [<Learner: First name: Robert, Last name: Lee, Date of Birth: 1999-01-02, Occupation: student, Social Link: https://www.facebook.com/robert/>, 
<Learner: First name: Mary, Last name: Smith, Date of Birth: 1991-06-12, Occupation: dba, Social Link: https://www.facebook.com/mary/>]>


NOTE:
In this lab, you have imported a standalone Django ORM project template with PostgreSQL. Based on the template, you
learned and practiced creating Django Models with relationships, saving those Django Models objects into databases, 
performing update and delete operations, and querying them with filters.

******************************************Fin de este Lab.
