# Creator

Este documento presenta la tercera guía GRASP<sup>1</sup>, la primera fue Expert y la segundo Polymofism.

Recuerden que las guías GRASP fueron planteadas por Craig Larmann en el libro
“Applying UML and Patterns”, de 1998. Tanto la guía presentada en este documento,
como el ejemplo que lo acompaña, están tomados de ese libro, que te recomendamos
consultar.

## Problema

Todos los objetos deben ser creados para poder enviarles mensajes que hagan uso de sus responsabilidades de hacer y de conocer.

¿Quién debería ser responsable de crear una nueva instancia de cierta clase?

## Solución

>Asignar a la clase B la responsabilidad de crear una instancia de la clase A si una de las siguientes condiciones es verdadera:
> - B agrega objetos A
> - B contiene objetos A
> - B guarda instancias de objetos A
> - B usa de forma muy cercana objetos A
> - B tiene los datos que serán provistos al constructor para inicializar instancias de A por lo que B es un experto con respecto a crear A.

Entonces B es un creador de objetos A. Cuando se pueda aplicar más de una alternativa, prefieran la clase B que agrega o contiene instancias de A.

En cualquiera de las opciones anteriores la clase B tiene una variable de instancia de A para referenciar objetos de esa clase, o tiene un contener capaz de almacenar instancias de A.

## Ejemplo

¿Recuerdan el ejemplo de la guía Expert del documento **Expert y SRP**? En ese
ejemplo teníamos algunas clases que podrían ser parte de una aplicación de punto
de venta<sup>2</sup>.

Una venta representada en la clase Sale, tiene su estado que está compuesto por las líneas de venta, representadas por la clase **SalesLineItem**, bien encapsulado: el atributo **lineItem** que contiene las instancias de **SalesLineItem** es privado, y las líneas sólo pueden ser agregadas enviando a una instancia de Sale un mensaje con selector **AddLineItem** y una instancia de **SalesLineItem** como argumento; algo similar ocurre con **RemoveLineItem** para quitar líneas.

La clase **Sale** se dice que agrega instancias de **SalesLineItem**

¿Quién tiene la responsabilidad de crear instancias de SalesLineItem?

Actualmente esto sucede en la clase **Program**; solo mostramos el código relevante, los puntos … representan el resto
del código:

```c#
public class Program
{
    …
    public static void Main(string[] args)
    {
        …
        sale.AddLineItem(new SalesLineItem(1, ProductAt(0)));
        sale.AddLineItem(new SalesLineItem(2, ProductAt(1)));
        sale.AddLineItem(new SalesLineItem(3, ProductAt(2)));
        …
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Creator_And_OCP/blob/master/v1/Program.cs)

De acuerdo a la guía Creator esto no es correcto, pues las instancias de **SalesLineItem** son creadas por la clase Program, cuando en realidad la clase que contiene esas instancias es **Sales**. Modificamos entonces la clase Sale para asignarle la responsabilidad de crear instancias de **SalesLineItem**; nuevamente, solo mostramos el código relevante, los puntos … representan el resto del código:

```c#
public class Sale
{
    private List<SalesLineItem> lineItems = new List<SalesLineItem>();
    …
    public SalesLineItem AddLineItem(double quantity, ProductSpecification product)
    {
        SalesLineItem item = new SalesLineItem(quantity, product);
        this.lineItems.Add(item);
        return item;
    }
    …
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Creator_And_OCP/blob/master/v2/Sale.cs)

El método **AddLineItem** de la clase Sale recibe como argumento todos los datos necesarios para crear instancias de **SalesLineItem**. Noten que además de crear la instancia, el objeto recién creado se agrega a item, con lo cual es menos probable que existan instancias de **SalesLineItem** que no pertenezcan a alguna instancia de **Sale**.

La clase **Program** ahora no crea instancias de **SalesLineItem**, le pide a Sale que lo haga; nuevamente, solo mostramos el código que cambió, los puntos … representan el resto del código.

```c#
public class Program
{
    …
    public static void Main(string[] args)
    {
        …
        sale.AddLineItem(1, ProductAt(0));
        sale.AddLineItem(2, ProductAt(1));
        sale.AddLineItem(3, ProductAt(2));
        …
    }
    …
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Creator_And_OCP/blob/master/v2/Program.cs)

*****

_<sup>1</sup> General Responsibility Assignment Methods._

_<sup>2</sup> La que usan las tiendas y supermercados para hacer las facturas de las ventas._
