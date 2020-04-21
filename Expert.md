![UCU](https://github.com/ucudal/PII_Conceptos_De_POO/raw/master/Assets/logo-ucu.png)

### FIT - Universidad Católica del Uruguay

<br>

# Expert

## Problema

¿Cuál es el principio más básico a seguir para asignar responsabilidades en un diseño orientado a objetos?

## Solución

> Asignar la responsabilidad al experto en información, es decir, a la clase que tiene la información necesaria para poder cumplir con la responsabilidad.

## Ejemplo

En una aplicación de punto de venta<sup>1</sup>, alguna clase tiene que tener la responsabilidad de conocer el total de la venta.

> ⚠️ **Importante**
>
> Comenzar asignando las responsabilidades enunciándolas claramente primero.

Siguiendo este consejo, la responsabilidad sería: ¿Quién debe tener la responsabilidad de conocer el total de una venta?

Por el patrón Expert, deberíamos mirar qué clases tienen la información necesaria para determinar el total. El total de una venta se calcula sumando el subtotal de las líneas de la venta; y a su vez el subtotal de cada línea se calcula como el producto de la cantidad vendida en esa línea multiplicada por el precio del producto vendido en esa línea.

Para mostrar las clases y sus responsabilidades vamos a utilizar tarjetas con tres secciones:

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

En la sección de arriba va el nombre de la clase, en la sección de abajo a la izquierda la lista de responsabilidades de hacer y conocer, y en la sección de abajo a la derecha la lista de clases que colabora con ésta para cumplir esas responsabilidades. 

Distribuyamos todas las responsabilidades que conocemos, excepto la de calcular el total de la venta que es la que estamos analizando con el patrón Expert usando las tarjetas CRC. 

Inicialmente, la clase `Sale` representa la venta con la fecha y con sus líneas de venta, que son responsabilidades de conocer; también tiene la responsabilidad de imprimir el ticket de la venta. La clase `Sales Line Item` representa las líneas de venta y colabora con la clase `Sale`:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>Sale</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer fecha y hora</p>
            <p>Conocer una o más líneas de ítems vendidos</p>
            <p>Imprimir el ticket</p>
        </td>
        <td>
            <p>Sales Line</p>
            <p>Item</p>
        </td>
    </tr>
</table>

La clase `SalesLineItem` representa la línea de la venta con la cantidad y el producto vendido en esa línea. La clase `Product Specification` representa los productos y colabora con la clase `SalesLineItem`:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>Sales Line Item</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer la cantidad de cada producto</p>
            <p>Conocer un producto</p>
        </td>
        <td>
            <p>Product Specification</p>
        </td>
    </tr>
</table>

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

Vean estas mismas clases programadas en C#:

```c#
public class Sale
{
    private ArrayList lineItems = new ArrayList();
    
    public DateTime DateTime { get; set; }
    
    public void AddLineItem(SalesLineItem item)
    {
        this.lineItems.Add(item);
    }
    
    public void RemoveLineItem(SalesLineItem item)
    {
        this.lineItems.Remove(item);
    }
    
    internal void PrintTicket()
    {
        foreach (SalesLineItem item in this.lineItems)
        {
            Console.WriteLine(
            $"{item.Quantity} de '{item.Product.Description}' a ${item.Product.Price}");
        }
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v1/Sale.cs)

<br/>

```c#
public class SalesLineItem
{
    public SalesLineItem(double quantity, ProductSpecification product)
    {
        this.Quantity = quantity;
        this.Product = product;
    }
    
    public double Quantity { get; set; }

    public ProductSpecification Product { get; set; }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v1/SalesLineItem.cs)

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

Un ejemplo del resultado de enviar el mensaje con selector `PrintTicket()` a una instancia de Sale con dos aguas minerales a $25.00, un café cortado a $35.00 y un café expreso a $31.00 sería:

```bash
2 de 'Agua mineral' a $25
1 de 'Café cortado' a $35
1 de 'Café expreso' a $31 
```

¿Qué se necesita para determinar el total de la venta? Es necesario conocer todas las instancias de `SalesLineItem` de una venta y la suma de los subtotales de cada línea. Sólo las instancias de `Sale` tienen la responsabilidad de conocer esta información; por lo tanto, por Expert, `Sale` es la clase correcta para asumir la responsabilidad de determinar el total; es el experto de información. 

Modificamos la tarjeta CRC de la clase `Sale` para que quede así:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>Sale</h3>
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
            <p>Sales Line</p>
            <p>Item</p>
        </td>
    </tr>
</table>

En C# agregamos un nuevo método `GetTotal` a la clase `Sale` y agregamos el total de la venta en el método `PrintTicket()` como aparece a continuación; solo mostramos el código nuevo, los puntos … representan el código que ya apareció antes.

```diff
public class Sale
{
    …
    
+   public double GetTotal()
+   {
+       double result = 0;
+       foreach (SalesLineItem item in this.lineItems)
+       {
+           result = result + (item.Quantity * item.Product.Price);
+       }
+       return result;
+   }

    …

    public void PrintTicket()
    {
        foreach (SalesLineItem item in this.lineItems)
        {
            Console.WriteLine(
            $"{item.Quantity} de '{item.Product.Description}' a ${item.Product.Price}");
        }
+       Console.WriteLine($"Total: ${this.Total}");
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v2/Sale.cs)

<br/>

Ahora el ejemplo del resultado de enviar el mensaje con selector `PrintTicket()` a la misma instancia de `Sale` del ejemplo anterior sería:

```bash
2 de 'Agua mineral' a $25
1 de 'Café cortado' a $35
1 de 'Café expreso' a $31
Total: $116 
```

Aún podemos hacer más. ¿Qué información se necesita para calcular el subtotal de una línea? La cantidad vendida y el precio del producto en esa línea. 

La clase `SalesLineItem` tiene la responsabilidad de conocer la cantidad vendida en la línea y el producto vendido; el producto es una instancia de `ProductSpecification`, que a su vez tiene la responsabilidad de conocer el precio. Por lo tanto, asignamos la responsabilidad de conocer el subtotal a la clase `SalesLineItem`. La tarjeta CRC queda así, la modificación en negrita:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>Sales Line Item</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Conocer la cantidad de cada producto</p>
            <p>Conocer un producto</p>
            <p><b>Conocer el subtotal</b></p>
        </td>
        <td>
            <p>Product Specification</p>
        </td>
    </tr>
</table>

El código en C# queda así, las modificaciones marcadas en verde:

```diff
public class SalesLineItem
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

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v3/SalesLineItem.cs)

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
            foreach (SalesLineItem item in this.lineItems)
            {
+               result = result + item.SubTotal;
            }
            return result;
        }
    }
    …
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v3/Sale.cs)

<br/>

El patrón Expert es usado más que ningún otro patrón en la asignación de responsabilidades; es un principio guía básico usado continuamente en el diseño orientado a objetos. Expert expresa la intuición de sentido común que los objetos hacen cosas relacionadas con la información que tienen. 

Noten que para cumplir una responsabilidad a menudo es necesario información que está desperdigada a través de diferentes clases de objetos. Esto implica que hay “expertos parciales” que colaboran para cumplir con la responsabilidad.

## Beneficios

La encapsulación se mantiene, porque los objetos usan su propia información para cumplir con las responsabilidades. Esto mantiene el acoplamiento<sup>2</sup> bajo, lo que produce programas más robustos y fáciles de mantener. El comportamiento se distribuye a través de clase que tienen la información requerida, promoviendo definiciones de clases más cohesivas<sup>3</sup> que son más fáciles de entender y de mantener.

*****

<sup>1</sup> _La que usan las tiendas y supermercados para hacer las facturas de las ventas._

<sup>2</sup> _Veremos acoplamiento más adelante; por ahora vean [este ejemplo](https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Object-oriented_programming)._

<sup>3</sup> _Veremos también cohesión más adelante; por ahora vean [este ejemplo](https://en.wikipedia.org/wiki/Cohesion_(computer_science))_