## Refracció de codi Àlex Peirau

# Bloaters: 
![Image text](https://github.com/apeirau/Refaccio-de-codi/blob/gh-pages/bloaters.png)

Representan algo que ha crecido en proporciones tan grandes que resulta complejo tanto de entender como de manejar. Generalmente, se acumulan con el tiempo y no pueden ser detectados hasta que el programa evolucione.

### Método largo: 
Un método contiene demasiadas líneas de código. En general, cualquier método de más de diez líneas debería hacer que comiences a hacer preguntas. (Forzado)
###### Código con "code smell":
```
public double calculateTotal(List<Item> items, String voucher, String membership, String address) {
      double baseTotal = 0;

      // sumup the prices
      List<Double> prices = new ArrayList<>();
      for (Item item : items) {
          prices.add(item.price());
      }

      for (double price : prices) {
          baseTotal = baseTotal + price;
      }

      // check if voucher is valid
      if (voucher.equals("GIMME_DISCOUNT") || voucher.equals("CHEAPER_PLEASE")) {
          baseTotal = BigDecimal.valueOf(baseTotal * 0.95).setScale(2, RoundingMode.HALF_EVEN).doubleValue();
      } else {
          System.out.println("Voucher invalid");
      }

      // handle delivery fee
      if (memebership.equalsIgnoreCase("GOLD")) {
          // do nothing
      } else {
          if (Pattern.matches(".*US.*", address)) {
              System.out.println("Adding flat delivery fee of 5 USD");
              baseTotal = baseTotal + 5;
          } else {
              System.out.println("Adding flat global delivery fee of 10 USD");
              baseTotal = baseTotal + 10;                
          }
      }
  }
```
######  Refactorización: 
Si se siente la necesidad de comentar el método, es muy posible que se deba tomar ese código y ponerlo en un nuevo método. Incluso una sola línea puede y debe dividirse en un método separado si requiere explicaciones; Si el método tiene un nombre descriptivo, nadie necesitará mirar el código para entender que hace.

### Clase grande
### Obsesión primitiva
### Lista larga de parámetros

# Object-Orientation Abusers:
![Image text](https://github.com/apeirau/Refaccio-de-codi/blob/gh-pages/oo-abusers.png)

Son aplicaciones incompletas o incorrectas de los principios de programación orientada a objetos.

### Cambiar declaraciones:
Tiene un operador de cambio complejo o una secuencia de instrucciones if.
###### Código con "code smell":
```
public class exampleObjectOrientedAbusers {

    enum ethnicity {
        EUROPEAN, AFRICAN, NORWEGIAN_BLUE
    } 
    
    
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        // TODO code application logic here

    }
    
    class Bird {
    // ...
    double getSpeed() {
        ethnicity type = ethnicity.EUROPEAN;
        
      switch (type) {
        case EUROPEAN:
          return getBaseSpeed();
        case AFRICAN:
            int numberOfCoconuts = 2;
          return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
        case NORWEGIAN_BLUE:
          double rand = Math.random();
          return (rand>0.5) ? 0 : getBaseSpeed();
      }
      throw new RuntimeException("Should be unreachable");
    }

        private double getBaseSpeed() {
            throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
        }

        private double getLoadFactor() {
            throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
        }
    }
    
}
```
######  Refactorización: 
Si no hay demasiadas condiciones en el operador y todos llaman al mismo método con diferentes parámetros, el polimorfismo será superfluo. Si este es el caso, puede dividir ese método en varios métodos más pequeños.

### Campos temporales
### Solicitud rechazada
### Clases alternativas con diferentes interfaces

# Change Preventers:
![Image text](https://github.com/apeirau/Refaccio-de-codi/blob/gh-pages/change-preventers.png)


Si necesita cambiar algo en un lugar de su código, también debe realizar muchos cambios en otros lugares. Como resultado, el desarrollo del programa se vuelve mucho más complicado y costoso.

### Cambio divergente
### Cirugía de escopeta:
Hacer cualquier modificación requiere que haga muchos pequeños cambios en muchas clases diferentes.
######  Código con "code smell": 
```
public class exampleChangePreventers {

    public static boolean debug = false;
    
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        // TODO code application logic here
        Person name1 = new Person();
        System.out.println(name1);
    }
    
    static class Person{
        String name;
        int age;
        // Duplicated toString method functionality.
        // Shotgun Surgery
        public String toString(){
        if (debug) {
            return "[*]" + this;
          } else {
            return "[!]" + this;
          }
        }
    }
    
    static class Product{
        int ID;
        String name;
        double price;
        // Duplicated toString method functionality.
        // Shotgun Surgery
        public String toString(){
        if (debug) {
            return "[*]" + this;
          } else {
            return "[!]" + this;
          }
        }
    }
}

// Solución: implementar class Logger para cuando llames al Logger.paint haga la función.
// Solución: implementar class Logger para cuando llames al Logger.paint haga la función.
```
######  Refactorización: 
- Utilice Mover método y Mover campo para mover comportamientos de clase existentes a una sola clase. Si no hay una clase apropiada para esto, cree una nueva.
- Si mover el código a la misma clase deja las clases originales casi vacías, intente deshacerse de estas clases ahora redundantes a través de Inline Class .
### Jerarquías de herencia paralelas

# Dispensables:

![Image text](https://github.com/apeirau/Refaccio-de-codi/blob/gh-pages/dispensables.png)


Un prescindible es algo sin sentido e innecesario cuya ausencia haría que el código fuera más limpio, más eficiente y más fácil de entender. 

### Comentarios:
Un método está lleno de comentarios explicativos. (Aunque está bien comentar que es lo que hacen algunas cosas, a veces superamos esa línea comentando código fácilmente entendible que lo único que hace es aumentar líneas de código y molestar)
######  Código con "code smell": 
```
public class exampleDispensables {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        // TODO code application logic here
        // Se llama a el método println pasándole una cadena "Hola Mundo" al objeto PrintStream de la clase "public final class System".
         System.out.println("Hola Mundo");
    }
    
    // Código que no hace nada. Nombre del "code smell": Speculative Generality.

    public static void noHagoNada(){
        Random random = new Random();
        random.nextInt(1,99);
    }
    
    // Creamos la classe Producto.
    static class Product{
        // Esta clase cuenta con las siguientes propiedades:
        // Una ID que empezará desde el 001 y será una llave autoincremental única.
        int ID;
        // Una cadéna en la que indicaremos el nombre del producto.
        String name;
        // Y finalmente unos números que serán el valor del precio.
        double price;
    }
}

// Solución: Dejar de escribir comentarios absurdos e innecesarios. Nombre del "code smell": Comments.
```
######  Refactorización: 
Si un comentario explica una sección de código, esta sección se puede convertir en un método separado a través de Método de extracción . Lo más probable es que el nombre del nuevo método se pueda tomar del propio texto del comentario.
### Código duplicado
### Clase perezosa
### Clase de datos
### Código muerto
### Generalidad especulativa

# Couplers:
![Image text](https://github.com/apeirau/Refaccio-de-codi/blob/gh-pages/couplers.png)


Todos los “code smells” de este grupo contribuyen al acoplamiento excesivo entre clases o muestran lo que sucede si el acoplamiento se reemplaza por una delegación excesiva.


### Característica Envidia
### Intimidad inapropiada
### Cadenas de mensajes
### Hombre medio:
Si una clase realiza solo una acción, delegando trabajo a otra clase, ¿por qué existe? 
######  Código con "code smell": 
```
public class exampleCouplers {
    
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        Program program = new Program();
        program.run();
        // TODO code application logic here
    }
    
    
    
}

class Program {
        public void run() {
             Person person = new Person();
             person = person.GetManager();
         }
     }


     class Person {
         public Department Department;
         public Person GetManager()
         {
             // Llama al método GetManager del Departamento desde "Person". Nombre del "code smell": Middle Man.
             return Department.GetManager();
         }
     }


     class Department {
         private Person _manager;
         public Department(Person manager)
         {
             _manager = manager;
         }
         public Person GetManager()
         {
             return _manager;
         }
     }
```
######  Refactorización: 
Si la mayoría de las clases de un método delegan a otra clase, Remove Middle Man está en orden; Esta clase no será tan necesaria como pensabamos.




