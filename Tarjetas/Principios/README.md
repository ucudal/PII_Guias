![UCU](https://github.com/ucudal/PII_Conceptos_De_POO/raw/master/Assets/logo-ucu.png)

[Principios y Patrones](../../README.md)

# Principios

## Tarjetas

| ADP: Dependencias Acíclicas |
| ---- |
| La estructura de dependencias entre paquetes debe ser un grafo dirigido acíclico; o sea, no debe haber ciclos en la estructura de dependencias. |

<br/>

| CCP: Cierre común |
| ---- |
| Las clases en un paquete deben estar cerradas contra el mismo tipo de cambios. |
| Un cambio que afecta un paquete afecta a todas las clases en ese paquete. |

<br/>

| CRP: Reutilización común |
| ---- |
| Las clases en un paquete se reutilizan al mismo tiempo. |
| Si reutilizas una clase de un paquete, las reutilizas todas. |

<br/>

| DIP: Inversión de dependencia |
| ---- |
| Los módulos de alto nivel no deberían depender de módulos de bajo nivel; ambos deberían depender de abstracciones. |
| Las abstracciones no deberían depender de detalles; los detalles deben depender de abstracciones. |

<br/>

| ISP: Segregación de interfaz |
| ---- |
| Los clientes no deberían ser forzados a depender de las interfaces que no usan. |

<br/>

| LSP: Sustitución de Liskov |
| ---- |
| En cualquier lugar de un programa donde se espera un objeto de un tipo puede aparecer un objeto de un subtipo y el comportamiento del programa no debería cambiar. |

<br/>

| OCP: Abierto/Cerrado |
| ---- |
| Los clases y métodos deberían estar abiertos a la extensión y cerrados a la modificación. |

<br/>

| REP: Equivalencia reutilización/liberación |
| ---- |
| La granularidad del reutilización es la granularidad de la liberación. |

<br/>

| SAP: Abstracciones estables |
| ---- |
| Los paquetes que son estables deberían ser abstractos; los inestables, concretos. |
| La abstracción de un paquete debería ser proporcional a su estabilidad. |

<br/>

| SDP: Dependencias estables |
| ---- |
| Las dependencias entre los paquetes en un diseño deben estar en la dirección de la estabilidad de los paquetes. |
| Un paquete debería depender sólo de paquetes que son más estables que él. |

<br/>

| SRP: Responsabilidad única |
| ---- |
| Nunca debería haber más de una razón para que una clase cambie. |

<br/>
