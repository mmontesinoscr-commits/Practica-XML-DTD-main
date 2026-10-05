# Práctica XML y DTD

## Objetivo

Modelar información mediante XML y validarla con DTD internas y externas.

## Ejercicio 1: Pedido

### Modelo propuesto

| Campo de búsqueda | Ruta XML | Valor |
|---|---|---|
| Destinatario | `pedido/destinatario/nombre` | Juan Delgado Martínez |
| Artículo | `pedido/articulo/nombre` | Bicicleta Bianchi |
| Dirección | `pedido/entrega/direccion` | Reforma 423, interior 201 |
| Fecha | `pedido/entrega/fecha` | 2021-09-19 |

### Decisiones de diseño

La dirección se separó en calle, número e interior para permitir búsquedas específicas, por ejemplo, por calle o número. La fecha se guardó como `AAAA-MM-DD`, un formato ordenable y fácil de procesar. Se usaron elementos para los datos principales porque tienen contenido propio y pueden ampliarse después; no fue necesario emplear atributos.

## Ejercicio 2: Nota

### DTD externo

`nota.xml` enlaza con `nota.dtd` mediante `<!DOCTYPE nota SYSTEM "nota.dtd">`.

### DTD interno

`nota-interno.xml` contiene las mismas declaraciones dentro de `<!DOCTYPE nota [ ... ]>`.

### Pruebas realizadas

| Modificación | ¿Bien formado? | ¿Válido? | Motivo |
|---|---|---|---|
| Cambiar `para` por `destinatario` | Sí | No | El elemento no está declarado en la DTD. |
| Intercambiar `para` y `de` | Sí | No | La DTD exige el orden `para, de, titulo, contenido`. |
| Agregar `telefono` | Sí | No | `nota` solo admite los cuatro elementos declarados. |

## Ejercicio 3: Matrícula

### Modelo

`matricula` contiene `personal` y `pago`. Dentro de `personal`, el contenedor `domicilios` contiene uno o más elementos `domicilio`.

### Cardinalidad

La expresión `domicilio+` significa «uno o más». Por eso, al eliminar todos los domicilios el XML sigue siendo bien formado, pero deja de ser válido respecto de la DTD.

### Restricción del atributo tipo

La declaración `tipo (familiar | habitual) #REQUIRED` limita los valores permitidos y obliga a indicar el atributo. `familiar` y `habitual` son válidos; `temporal` o la ausencia del atributo no son válidos.

### DTD externo

`matricula.xml` usa `matricula.dtd`.

### DTD interno

`matricula-interno.xml` incluye las mismas reglas dentro del propio documento.

### Pruebas realizadas

| Caso | ¿Válido? | Motivo |
|---|---|---|
| `tipo="familiar"` | Sí | Es un valor permitido. |
| `tipo="habitual"` | Sí | Es un valor permitido. |
| `tipo="temporal"` | No | No pertenece a la enumeración. |
| Sin `tipo` | No | El atributo es obligatorio (`#REQUIRED`). |

## Conclusiones

1. Un XML bien formado cumple las reglas sintácticas de XML; uno válido, además, cumple las reglas de su DTD.
2. Una DTD define qué elementos, atributos, orden y repeticiones admite un documento XML.
3. La DTD interna vive en el propio XML; la externa está en un archivo independiente reutilizable.
4. La cardinalidad se expresa con `?` (cero o uno), `*` (cero o más) y `+` (uno o más).
5. Un atributo se restringe con una enumeración, por ejemplo `(familiar | habitual)`, y con `#REQUIRED` se vuelve obligatorio.
6. Git permite registrar avances y probar cambios sin perder una versión funcional.
7. `git diff` muestra las modificaciones y `git restore` recupera una versión previa del archivo.
8. Una rama permite desarrollar y comprobar una alternativa sin alterar la rama principal hasta que esté lista.
