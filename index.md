## Refracció de codi Àlex Peirau

# Bloaters: 
![Image text](https://github.com/apeirau/Refaccio-de-codi/blob/gh-pages/bloaters.png)

Representan algo que ha crecido en proporciones tan grandes que resulta complejo tanto de entender como de manejar. Generalmente, se acumulan con el tiempo y no pueden ser detectados hasta que el programa evolucione.

### Método largo: 
Un método contiene demasiadas líneas de código. En general, cualquier método de más de diez líneas debería hacer que comiences a hacer preguntas. (Forzado)
###### Codigo con "code smell":
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
### Clase grande: 
### Obsesión primitiva: 
### Lista larga de parámetros: 



