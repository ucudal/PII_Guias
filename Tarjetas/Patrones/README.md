![UCU](https://github.com/ucudal/PII_Conceptos_De_POO/raw/master/Assets/logo-ucu.png)

[Principios y Patrones](../../README.md)

# Patrones GRASP

## Tarjetas

| Expert (Experto) |
| ---- |
| **Problema** |
| ¿Cuál es el principio básico para asignar responsabilidades? |
| **Solución** |
| Asignar la responsabilidad al experto en los datos, es decir, a la clase que tiene los datos necesarios para cumplir la responsabilidad |

<br/>

| Creator (Creador) |
| ---- |
| **Problema** |
| ¿Qué clase debe ser responsable de crear instancias de otra clase? |
| **Solución** |
| Asignar a B la responsabilidad de crear instancias de A si:<br/>- B está compuesto de/contiene A<br/>- B usa estrechamente a A<br/>- B es un experto para crear A |

<br/>

| Low Coupling (Bajo Acoplamiento) |
| ---- |
| **Problema** |
| ¿Cómo soportar baja dependencia y maximizar la reutilización? |
| **Solución** |
| Asignar las responsabilidades de forma que el acoplamiento permanezca bajo |

<br/>

| High Cohesion (Alta Cohesión) |
| ---- |
| **Problema** |
| ¿Cómo mantener la complejidad manejable? |
| **Solución** |
| Asignar las responsabilidades de forma que la cohesión permanezca alta |

<br/>

| Polymorphism (Polimorfismo) |
| ---- |
| **Problema** |
| ¿Cómo manejar alternativas con base en los tipos? ¿Cómo construir componentes conectables? |
| **Solución** |
| Usar operaciones polimórficas y evitar preguntar por la clase de un objeto |

<br/>

| Pure Fabrication (Fabricación Pura) |
| ---- |
| **Problema** |
| ¿A quién asignar responsabilidades sin generar baja cohesión y alto acoplamiento? |
| **Solución** |
| Asignar responsabilidades altamente cohesivas a una clase artificial poco acoplada |

<br/>

| Indirection (Indirección) |
| ---- |
| **Problema** |
| ¿A quién asignar responsabilidades para evitar acoplamiento directo? |
| **Solución** |
| Asignar responsabilidades a un objeto intermedio para evitar el acoplamiento directo |

<br/>

| Don’t talk to strangers (Ley de Demeter) |
| ---- |
| **Problema** |
| ¿A quién asignar responsabilidades para evitar conocer la estructura interna de un objeto? |
| **Solución** | 
| Enviar mensajes sólo a sí mismo, a un parámetro, a un atributo, a un objeto creado en el método, o al contenido de una colección poseída |
