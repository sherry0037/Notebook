================================
Spring in Action - Notes
================================
Reading notes of the book Spring in Action - 4th edition by Craig Walls.
<<<<<<< HEAD
=======

----------------------
Basic concepts
----------------------

----------------------
Wiring beans
----------------------


Why do we need bean-wiring?
----------------------------------

An application (at least a non-trivial one) is made up of several objects, each has its own funtionalities. These objects must be aware of one another and communicate with one another to get their jobs done.

If you write the application in the traditional, these associations between objects are created via constuction or lookup, leading to complicated code that's difficult to reuse or unit-test. The ojects do more work (like initializing other objects), and are highly coupled to one another.


This is when **wiring**, or **dependency injection** (DI), comes handy. Remeber, beans can be seen as just Plain Old Java Objects (POJOs) and the ojects are created and wired by *Spring application context* (container). Wiring means that, instead of letting the object to initialize it's own instance of other objects, we will inject the instance into this object either using the *constructor* or a *setter*. In spring, the container will do this for you.

There are several ways to wire beans in Java. The three major ones are: autowiring, JavaConfig and XML. 


Spring's configuration options
----------------------------------

We need to tell Spring which beans to create and how to wire them together. There are three major approaches to write a bean wiring specification:

- Explicit configuration in XML
- Explicit configuration in Java
- Implicit bean discovery and automatic wiring

The author recommends to use automatic wiring as much as possible. But you can use whichever you want, even mix them.

Automatically wiring beans
----------------------------------
Spring attacks automatic wiring from two angles:

- *Component scanning*: Spring automatically discovers beans to be created in the application context.
- *Autowiring*: Spring automatically satisfies bean dependencies.

.. topic:: A Stereo System

    To demonstrate Spring's autowiring mechanism, we will create a stereo system. To start, we will create a ``CompactDisc`` class, and let Spring to discover and create it as a bean. Then we will have a ``CDPlayer`` class, also have Spring to discover it, and inject a CD bean into it.

Creating beans
==================================

First, let's write an interface for ``CompactDisc``. Using interface keeps the coupling between any CD player implementation and the CD itself to a minimum.

.. code-block:: java

    public interface CompactDisc{
        void play();
    }

Now we write a specific implementation, ``SgtPeppers``, of ``CompactDisc``.

.. code-block:: java

    @Component
    public class SgtPeppers implements CompactDisc {

        private String title = "Sgt. Pepper's Lonely Hearts Club Band";
        private String artist = "The Beatles";

        public void play() {
            System.out.println("Playing " + title + " by " + artist);
        }
    }


.. topic:: Note
    
    The ``SgtPeppers`` class is annotated with *@Component*. This identifies this class as a component class and tells Spring that a bean should be created for it. We don't need to explicitly configure a ``SgtPeppers`` bean.

Spring needs to seek out classes annotated with @Component and create beans from them. We write a configuration to tell it to do so.

.. code-block:: java

    @Configuration
    @ComponentScan
    public class CDPlayerConfig {
    }

In this minimal configuration, @ComponentScan default to scanning the same package as the configuration class. We put the ``CompactDisc`` class under the same package as ``CDPlayerConfig``, so that @ComponentScan will find it and create a bean for it.

Now we can write a simple JUnit test that creates a Spring application context and asserts that the ``CompactDisc`` bean is created.

.. code-block:: java

    @RunWith(StringJUnit4ClassRunner.class)
    @ContextConfiguration(classes=CDPlayerCOnfig.class)
    public class CDPlayerTest{

        @Autowired
        private COmpactDisc cd;

        @Test
        public void cdShouldNotBeNull(){
            assertNotNull(cd);
        }
    }

Note:
- ``SpringJUnit4ClassRunner`` automatically creates a Spring application context when the test starts.
- ``@ContextCOnfiguration`` tells it to load configuration from ``CDPlayerConfig`` class.
    - The configuration class includes @ComponentScan, so the application context will include the ``CompactDisc`` bean.
- The test has a property of type ``CompactDisc``, annotated with ``@Autowired``, to inject the bean into the test.
- All classes in or under the ``soundsystem`` package that are annotated with ``@Component`` will be created as beans.

Setting a base package for component scanning
==============================================
When using ``@ComponentScan`` with no attributes, it will default to the configuration class's package as its base package. 
We want to explictitly set the base package, so that we can keep all of the configuration code in a package of its own.
There are several ways to do it:

.. code-block:: java

    @ComponentScan("soundsystem")

.. code-block:: java

    @ComponentScan(basePackages="soundsystem")

To dpecify multiple base packages:

.. code-block:: java

    @ComponentScan(basePackages={"soundsystem", "video"})

Or you can use classes/interfaces instead of strings:

.. code-block:: java

    @ComponentScan(basePackageClasses={CDPlayer.class, DVDPlayer.class})


Autowiring beans
==============================================
Autowiring is a means of letting Spring automatically satisfy a bean's dependencies by finding other beans in the application context that are a match to the bean's needs. ``@Autowired`` annotation is used for this case.

>>>>>>> Add Spring in Action note
