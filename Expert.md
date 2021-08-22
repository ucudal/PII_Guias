![UCU](https://github.com/ucudal/PII_Conceptos_De_POO/raw/master/Assets/logo-ucu.png)

### FIT - Universidad Católica del Uruguay

<br>

# Expert

## Problema

¿Cuál es el principio más básico a seguir para asignar responsabilidades en un diseño orientado a objetos?

## Solución

> Asignar la responsabilidad al experto en información, es decir, a la clase que tiene la información necesaria para poder cumplir con la responsabilidad.

## Ejemplo

Vamos a trabajar en una aplicación de punto de venta<sup>1</sup>, donde tenemos clases para representar un ticket de venta. Para este ejemplo utilizamos un ticket<sup>2</sup> simplificado, que luce más o menos así:

```bash
Fecha: 31/3/2021
2 de 'Agua mineral' a $25
1 de 'Café cortado' a $35
1 de 'Café expreso' a $31
```

En este ejemplo, compramos el 31 de marzo del 2021 dos aguas minerales a $25 cada una, un café cortado a $35, y un café expreso a $31.

> ⚠️ **Importante**
>
> Comenzar asignando las responsabilidades enunciándolas claramente primero.

Para mostrar las clases y sus responsabilidades vamos a utilizar "tarjetas" con tres secciones:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>&nbsp;</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>&nbsp;</p>
        </td>
        <td>
            <p>&nbsp;</p>
        </td>
    </tr>
</table>


En la sección de arriba va el nombre de la clase, en la sección de abajo a la izquierda la lista de responsabilidades de hacer y conocer de esa clase, y en la sección de abajo a la derecha la lista de clases que colabora con ésta para cumplir esas responsabilidades.

Estas "tarjetas" se llaman CRC por "clases", "responsabilidades" y "colaboraciones".

En este ejemplo tenemos como punto de partida las tarjetas CRC para clases que ya existen en la aplicación de punto de venta. La clase `SaleTicket` representa el ticket de venta; esta clase tiene la responsabilidad de conocer la fecha y las líneas de los ítems vendidos; también tiene la responsabilidad de armar el texto para imprimir el ticket. La clase `TicketLineItem` representa las líneas de los ítems vendidos y colabora con la clase `SaleTicket`:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>SaleTicket</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer fecha y hora</p>
            <p>Conocer una o más líneas de ítems vendidos</p>
            <p>Imprimir el ticket</p>
        </td>
        <td>
            <p>TicketLineItem</p>
        </td>
    </tr>
</table>

> La responsabilidad de la clase `SaleTicket` de imprimir el ticket la analizaremos en otro artículo sobre el principio de responsabilidad única o [SRP](https://github.com/ucudal/PII_Principios_Patrones/blob/master/SRP.md) por sus siglas en inglés.

La clase `TicketLineItem` representa la línea del ticket con la cantidad y el producto vendido en esa línea. La clase `ProductSpecification` representa los productos y colabora con la clase `TicketLineItem`:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>TicketLineItem</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer la cantidad del producto</p>
            <p>Conocer el producto</p>
        </td>
        <td>
            <p>ProductSpecification</p>
        </td>
    </tr>
</table>

Por ejemplo, en el ticket de arriba, ```2 de 'Agua mineral' a $25``` es una línea del ticket.

La clase `ProductSpefication` representa los productos con su precio y no necesita colaborar con ninguna clase:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>Product Specification</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer la descripción</p>
            <p>Conocer el precio</p>
        </td>
        <td>
        </td>
    </tr>
</table>

Por ejemplo, en el ticket de arriba, ```Agua mineral``` es un producto que cuesta ```$25```.

Vean estas mismas clases programadas en C#:

```c#
public class SaleTicket
{
    private ArrayList lineItems = new ArrayList();

    public DateTime DateTime { get; set; }

    public void AddLineItem(TicketLineItem item)
    {
        this.lineItems.Add(item);
    }

    public void RemoveLineItem(TicketLineItem item)
    {
        this.lineItems.Remove(item);
    }

    public void PrintTicket()
    {
        Console.WriteLine($"Fecha: {this.DateTime}");
        foreach (TicketLineItem item in this.lineItems)
        {
            Console.WriteLine($"{item.Quantity} de '{item.Product.Description}' a ${item.Product.Price}");
        }
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v1/SaleTicket.cs)

<br/>

```c#
public class TicketLineItem
{
    public TicketLineItem(double quantity, ProductSpecification product)
    {
        this.Quantity = quantity;
        this.Product = product;
    }

    public double Quantity { get; set; }

    public ProductSpecification Product { get; set; }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v1/TicketLineItem.cs)

<br/>

```c#
public class ProductSpecification
{
    public ProductSpecification(string description, double price)
    {
        this.Description = description;
        this.Price = price;
    }

    public string Description { get; set; }

    public double Price { get; set; }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v1/ProductSpecification.cs)

<br/>

Un ejemplo de enviar el mensaje con selector `PrintTicket()` a una instancia de `SaleTicket` con dos aguas minerales a $25.00, un café cortado a $35.00 y un café expreso a $31.00, sería el que ya hemos visto:

```bash
Fecha: 31/3/2021
2 de 'Agua mineral' a $25
1 de 'Café cortado' a $35
1 de 'Café expreso' a $31
```

Ahora bien, en este ejemplo, si quisiéramos agregar al ticket el total de la venta, ¿quién debe tener la responsabilidad de conocer ese total?

Por el patrón Expert, deberíamos mirar qué clases tienen la información necesaria para determinar el total. El total de una venta se calcula sumando el subtotal de las líneas del ticket; y a su vez, el subtotal de cada línea se calcula como el producto de la cantidad vendida en esa línea, multiplicada por el precio del producto vendido en esa línea.

¿Qué se necesita para determinar el total de la venta? Es necesario conocer todas las instancias de `TicketLineItem` de un ticket y la suma de los subtotales de cada línea. Sólo las instancias de `SaleTicket` tienen la responsabilidad de conocer esta información; por lo tanto, por Expert, `SaleTicket` es la clase correcta para asumir la responsabilidad de determinar el total; es el experto de información.

Modificamos la tarjeta CRC de la clase `SaleTicket` para que quede así, los cambios en negrita:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>SaleTicket</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer fecha y hora</p>
            <p>Conocer una o más líneas de ítems vendidos</p>
            <p>Imprimir el ticket</p>
            <p><b>Calcular el total</b></p>
        </td>
        <td>
            <p>SalesLineItem</p>
        </td>
    </tr>
</table>

En C# agregamos una nueva propiedad `Total` a la clase `SaleTicket` y agregamos el total de la venta en el método `GetTicketText()` como aparece a continuación; solo mostramos el código nuevo, los puntos … representan el código que ya apareció antes.

```diff
public class SaleTicket
{
    …

+   public double Total
+   {
+       get
+       {
+           double result = 0;
+           foreach (TicketLineItem item in this.lineItems)
+           {
+               result = result + (item.Quantity * item.Product.Price);
+           }
++           return result;
+       }
+   }

    …

    public void PrintTicket()
    {
        Console.WriteLine($"Fecha: {this.DateTime}");
        foreach (TicketLineItem item in this.lineItems)
        {
            Console.WriteLine($"{item.Quantity} de '{item.Product.Description}' a ${item.Product.Price}");
        }

+       Console.WriteLine($"Total: ${this.Total}");
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v2/SaleTicket.cs)

> Te puede llamar la atención que implementemos la resposabilidad de conocer el total de la venta como una propiedad de sólo lectura `Total` y no como un método con firma `double GetTotal()` o algo así. Tratamos de representar las responsabilidad de conocer como propiedades en C#. En este caso `Total` es una propiead de sólo lectura -sólo tiene implementado su `get` y no su `set`- porque el total de la venta cambia sólo cuando se agregan o quitan líneas, o cambian las cantidades de las líneas.

<br/>

Ahora el resultado de enviar el mensaje con selector `PrintTicket()` a la misma instancia de `SaleTicket` del ejemplo anterior sería:

```bash
Fecha: 31/3/2021
2 de 'Agua mineral' a $25
1 de 'Café cortado' a $35
1 de 'Café expreso' a $31
Total: $116
```

Aún podemos hacer más. ¿Qué información se necesita para calcular el subtotal de una línea? La cantidad vendida y el precio del producto en esa línea.

La clase `TicketLineItem` tiene la responsabilidad de conocer la cantidad vendida en la línea y el producto vendido; el producto es una instancia de `ProductSpecification`, que a su vez tiene la responsabilidad de conocer el precio. Por lo tanto, asignamos la responsabilidad de conocer el subtotal de una línea del ticket a la clase `TicketLineItem`. La tarjeta CRC queda así, la modificación en negrita:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>TicketLineItem</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer la cantidad de cada producto</p>
            <p>Conocer un producto</p>
            <p><b>Conocer el subtotal</b></p>
        </td>
        <td>
            <p>ProductSpecification</p>
        </td>
    </tr>
</table>

El código en C# queda así, las modificaciones marcadas en verde:

```diff
public class TicketLineItem
{
    …

+   public double SubTotal
+   {
+       get
+       {
+           return this.Quantity * this.Product.Price;
+       }
+   }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v3/TicketLineItem.cs)

<br/>

```diff
public class Sale
{
    …

    public double Total
    {
        get
        {
            double result = 0;
            foreach (TicketLineItem item in this.lineItems)
            {

-               result = result + (item.Quantity * item.Product.Price);
+               result = result + item.SubTotal;
            }
            return result;
        }
    }
    …
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v3/SaleTicket.cs)

<br/>

El patrón Expert es usado más que ningún otro patrón en la asignación de responsabilidades; es un principio guía básico usado continuamente en el diseño orientado a objetos. Expert expresa la intuición de sentido común de que los objetos hacen cosas relacionadas con la información que tienen.

Noten que para cumplir una responsabilidad a menudo es necesario información que está desperdigada a través de diferentes clases de objetos. Esto implica que hay “expertos parciales” que colaboran para cumplir con la responsabilidad.

## Beneficios

La encapsulación se mantiene, porque los objetos usan su propia información para cumplir con las responsabilidades. Esto mantiene el acoplamiento<sup>3</sup> bajo, lo que produce programas más robustos y fáciles de mantener. El comportamiento se distribuye a través de clase que tienen la información requerida, promoviendo definiciones de clases más cohesivas<sup>4</sup> que son más fáciles de entender y de mantener.

*****

<sup>1</sup> _La que usan las tiendas y supermercados para hacer las facturas de las ventas._

<sup>2</sup> _El papelito que te entregan cuando comprás algo._

<sup>3</sup> _Veremos acoplamiento más adelante; por ahora vean [este ejemplo](https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Object-oriented_programming)._

<sup>4</sup> _Veremos también cohesión más adelante; por ahora vean [este ejemplo](https://en.wikipedia.org/wiki/Cohesion_(computer_science))_