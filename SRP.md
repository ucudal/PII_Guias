![UCU](https://github.com/ucudal/PII_Conceptos_De_POO/raw/master/Assets/logo-ucu.png)

### FIT - Universidad Católica del Uruguay

<br>

# Single Responsibility Principle (SRP)

Hasta ahora vimos un criterio para asignar responsabilidades a clases: la clase que es experta en la información necesaria para cumplir con una responsabilidad debe tener esa responsabilidad. A la hora de asignar responsabilidades hay que tener en cuenta que las responsabilidades asignadas a una clase tienen que estar relacionadas con una sola funcionalidad de la aplicación, y de esto se trata el primer principio SOLID: el principio de responsabilidad única, o SRP por sus siglas en inglés.

## Enunciado

El principio de responsabilidad única establece que cada clase debe tener responsabilidad sobre una parte de la funcionalidad proporcionada por el software, y que la responsabilidad debe estar completamente encapsulada por la clase.

Todos los métodos y atributos de la clase deben estar estrechamente alineados con esa responsabilidad. El princpio se expresa como:

> Una clase debe tener solo una razón para cambiar.

## Ejemplo

La clase `Sale` tiene responsabilidades sobre una venta, tales como conocer su fechar y calcular el total, pero también tiene la responsabilidad de imprimir el ticket.

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
            <p>Calcular el total</p>
        </td>
        <td>
            <p>Sales Line</p>
            <p>Item</p>
        </td>
    </tr>
</table>

Aunque imprimir el ticket definitivamente necesita información que está en la clase `Sale`, si en lugar de imprimir en la consola como sucede en la versión actual hubiera que imprimir en una impresora, por ejemplo, la clase `Sale` debería cambiar. Pero también debería cambiar si incluyéramos descuentos, por ejemplo. Entonces existe más de una razón por la cual la clase `Sale` debe cambiar, lo que viola el principio SRP. Podemos separar la responsabilidad de imprimir el ticket a una nueva clase `ConsolePrinter`. Esta clase debe colaborar con la clase `Sale`, que le provee el texto a imprimir, lo cual implica una nueva responsabilidad para la clase `Sale`:

<table id="card">
    <tr>
        <td align="center" colspan="2">
            <h3>ConsolePrinter</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Imprimir el ticket en la consola</p>
        </td>
        <td>
            <p>Sale</p>
        </td>
    </tr>
</table>

La responsabilidad agregada en la clase `Sale` está marcada de negrita:

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
            <p>Calcular el total</p>
            <p><b>Representar la venta como texto a imprimir</b></p>
        </td>
        <td>
            <p>Sales Line</p>
            <p>Item</p>
        </td>
    </tr>
</table>

Con este nuevo diseño podemos tener múltiples formas de imprimir; por ejemplo, para imprimir la venta en una impresora de rollo de papel, podríamos tener una clase `PaperRollPrinter`, que podemos implementar sin tener que modificar ninguna de nuestras clases existentes. 

Las nuevas clases `Sale` y `ConsolePrinter` quedan en C# así -los … representan el código que ya apareció antes, como en los casos anteriores-.

```c#
public class ConsolePrinter
{
    public static void PrintTicket(Sale sale)
    {
        Console.WriteLine(sale.GetTextToPrint());
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v4/ConsolePrinter.cs)

<br/>

```c#
public class Sale
{
    …
    
    public string GetTextToPrint()
    {
        string result = string.Empty;
        foreach (SalesLineItem item in this.lineItems)
        {
            result = result + item.GetTextToPrint();
        }

        result = result + $"Total: ${this.Total}";
        return result;
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v4/Sale.cs)

<br/>

El programa principal ahora usa la nueva clase `ConsolePrinter` para imprimir el ticket:

```c#
public class Program
{
    …

    public static void Main(string[] args)
    {
        PopulateCatalog();

        Sale sale = new Sale();
        sale.DateTime = DateTime.Now;
        sale.AddLineItem(new SalesLineItem(1, ProductAt(0)));
        sale.AddLineItem(new SalesLineItem(2, ProductAt(1)));
        sale.AddLineItem(new SalesLineItem(3, ProductAt(2)));
        ConsolePrinter.PrintTicket(sale);
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Expert_And_SRP/blob/master/v4/Program.cs)

<br/>

## Beneficios

Vimos con el ejemplo que para agregar una clase `RollerPaparPrinter`, podemos comenzar a imprimir la factura en papel sin modificar ninguna de las clases existentes. Este diseño es entonces más fácil de extender, es más robusto a las modificaciones.
