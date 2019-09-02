================================
MealPlanner 0.1
================================

.. contents::

----------------------
Setup Environment
----------------------

Maven
================================
1. First set $JAVA_HOME environment variable on Mac OS X. Follow this `mac environment setting guide <http://www.sajeconsultants.com/how-to-set-java_home-on-mac-os-x/>`_ (with some modifications).

.. code-block:: bash

    $ vim ~/.bash_profile 

    JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home
    export JAVA_HOME;

The path of ``JAVA_HOME`` depends on where you installed Java. One way to find the path is to open IntelliJ, create a new project, and see where the default SDK is. ``.bash_profile`` is read before ``.profile``, so we write the statesment in ``.bash_profile``. To test if it works, restart the terminal and run:

.. code-block:: bash

    $ echo $JAVA_HOME
   /Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home

    $JAVA_HOME/bin/java -version
    java version "11.0.2" 2019-01-15 LTS
    Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS)
    Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.2+9-LTS, mixed mode)

2. Download the binary distribution archive `here <https://maven.apache.org/download.cgi>`_.
Extract the files in any directory: ``tar xzvf apache-maven-3.6.1-bin.tar``.

3. Add the bin directory of the created directory apache-maven-3.6.1 to the PATH environment variable. 

.. code-block:: bash

    $ vim ~/.bash_profile 
    export PATH="/Library/Maven/apache-maven-3.6.1/bin:$PATH"

Confirm with ``mvn -v`` in a new shell. The result should look similar to

.. code-block:: bash

    Maven home: /Library/Maven/apache-maven-3.6.1
    Java version: 11.0.2, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home
    Default locale: en_US, platform encoding: UTF-8
    OS name: "mac os x", version: "10.14.5", arch: "x86_64", family: "mac"


-----------------------------------------------------
Hello World Example: Building a RESTful Web Service
-----------------------------------------------------
We will use Maven to build this example from scratch. Read `Building Java Projects with Maven <https://spring.io/guides/gs/maven/>`_ to learn how to use Maven. 

.. topic:: Note

    Maven projects are defined with an XML file named ``pom.xml``. Among other things, this file gives the project’s name, version, and dependencies that it has on external libraries. A sample ``pom.xml`` file and explaination can be found in the guide.

We can also use IntelliJ IDEA to run the completed codes. Open IntelliJ IDEA and import Maven's ``pom.xml`` file under the **complete** folder, then everything is all set. Reference: `Working a Getting Started guide with IntelliJ IDEA <https://spring.io/guides/gs/intellij-idea/>`_.

After setting up the project and build system, we can start creating a resource representation class ``src/main/java/hello/Greeting.java``. It looks like this:

.. code-block:: java

    package hello;

    public class Greeting {

        private final long id;
        private final String content;

        public Greeting(long id, String content) {
            this.id = id;
            this.content = content;
        }

        public long getId() {
            return id;
        }

        public String getContent() {
            return content;
        }
    }


Then create a resource controller ``src/main/java/hello/GreetingController.java``:

.. code-block:: java

    package hello;

    import java.util.concurrent.atomic.AtomicLong;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class GreetingController {

        private static final String template = "Hello, %s!";
        private final AtomicLong counter = new AtomicLong();

        @RequestMapping("/greeting")
        public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
            return new Greeting(counter.incrementAndGet(),
                                String.format(template, name));
        }
    }

Breakdowns:

- The ``@RequestMapping`` annotation ensures that HTTP requests to ``/greeting`` are mapped to the ``greeting()`` method. The annotation maps all HTTP operations by default. to narrow down the mapping, use ``@RequestMapping(method=GET)``.

- ``@RequestParam`` binds the value of the query string parameter ``name`` into the ``name`` parameter of the ``greeting()`` method. If we have a query string ``http://localhost:8080/greeting?name=User``, ``User`` is parsed to the ``greeting()`` method.


.. topic:: Note

    The traditional MVC controller relies on a view techonology to perform server-side rendering of the greeting data to HTMl. **RESTful web service controller**, on the other hand, populates and returns a ``Greeting`` object. The object data will be written directly to the HTTP response as JSON.

- ``RestController`` is a new annotation in Spring 4. It marks the class as a controller where every method returns a domain object instead of a view.

- Spring's HTTP message converter convertes the ``Greeting`` object to JSON automatically.

The next step is to package everything in a single, executable JAR file. First create the ``main()`` method ``src/main/java/hello/Application.java``:

.. code-block:: java
    
    package hello;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class Application {

        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }
    }

To build an executable JAR, change to ``complete/`` directory then run ``./mvnw clean package``. Then you can run the JAR file:


.. code-block:: bash

    java -jar target/gs-rest-service-0.1.0.jar

Or to run the application directly:

.. code-block:: bash

    ./complete/mvnw spring-boot:run

To test the service, visit http://localhost:8080/greeting. You can also provide a name in the quesry string: http://localhost:8080/greeting?name=User. The results are:

.. code-block:: bash

    {"id":1,"content":"Hello, World!"}

    {"id":2,"content":"Hello, User!"}

 
