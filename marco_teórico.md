---
geometry: margin=2cm

---

# Bosquejo del sistema
- **Espacio**: Grafo conexo, en general no completo, potencialmente dinámico (que se generen y destruyan sus aristas)
- **Agentes**: Agentes descritos en detalle abajo, existiendo dos tipos: Compradores y vendedores. Estos vivirán en nodos/vértices del grafo, siendo las aristas la conexión que represente que tienen acceso a realizar una compra/venta entre ellos.
- **Dinámica**: En cada paso del tiempo los compradores, quienes ocupan de alguna cantidad $x$ de un bien $B$ para sobrevivir, lo buscarán comprar entre los vendedores con los que estén conectados. Esto significa que necesita existir, para todo comprador y para todo tiempo, al menos una conexión con un vendedor o bien, permitir algún mecanismo por medio del cual un comprador pueda convertirse en vendedor bajo algunas condiciones o que pueda acceder al bien de forma remota ya sea con una penitencia de un precio más elevado sobre el base o con una menor cantidad de mercancilla. Estos escenarios se irán considerando a medida capas de complejidad del modelo se van añadiendo.


# Diseño de los agentes
- Dos tipos de agentes: Compradores y vendedores.
- Existen $N$ agentes en total, siendo de ellos $N_c$ compradores y $N_v$ vendedores con las condiciones que:

    1. $N_v \ll N_c$, es decir, van a haber muchos más vendedores que compradores
    2. $N_c + N_v = N$. El número de vendedores y de compradores debe igualar al número total de agentes.

  Por facilidad de notación posterior podemos también definir dos conjuntos, $\mathcal{C}$ y $\mathcal{V}$ que denoten respectivamente el conjunto de todos los compradores y el de todos los vendedores. Por supuesto, $|\mathcal{C}| = N_c$ y $|\mathcal{V}| = N_v$. 
- Los agentes deben tener un parámetro `supply` que sirva para cuantificar:

    1. En el caso de los vendedores, `supply` indica cuánta mercancilla tienen aún por vender. Un vendedor que venda todo (`supply` = 0) dejará de interactuar con sus compradores vecinos, lo que puede resultar en que sus compradores se vean obligados a comprar de otro vendedor con precios más caros.
    2. En el caso de los compradores, `supply` indica cuánta mercancilla ocupan para sobrevivir y, asumiendo una sola transacción por día, es la cantidad que buscarán comprar de su vendedor de elección (el que esté más cercano y, en caso de empate, se decide en base al precio). En caso de que el vendedor de elección no tenga la cantidad completa de `supply` que necesite, se hará una compra parcial y el resto se comprará de su siguiente mejor opción. Este marco estratégico de compra se formalizará en forma de un algoritmo de selección que puede ser distinto en distintas versiones o capas de complejidad del modelo.
- Denotemos como $S_j^c$ a la `supply` del $j$-ésimo comprador (denotado $c_j \in \mathcal{C}$), donde el órden de los compradores ha sido decidido de forma arbitraria pero fija. Similarmente $S_j^v$ denota la `supply` del $j$-ésimo vendedor (denotado $v_j \in \mathcal{V}$). Entonces tiene que ser el caso (al menos en capas iniciales de complejidad del modelo) que:
    $$
    \sum_{j=1}^{N_c} S_j^c = \sum_{j=1}^{N_v} S_j^v
    $$
    De manera que todos los compradores sean capaces de suplir sus necesidades y todos los compradores puedan vender toda su mercancilla. Pueden implementarse mecanismos de escacez y de reserva de `supply` restante en los vendedores para capas de mayor complejidad del modelo.
