Acceso a un único elemento de un tipo de retorno estructurado durante la llamada a método/función/propiedad
La siguiente implementación se puede utilizar para tener acceso directamente a un elemento individual del tipo de datos estructurado que devuelve el método/función/propiedad cuando se llama a un método, función o propiedad. Un tipo de datos estructurado es, por ejemplo, una estructura o un bloque de funciones.

El tipo devuelto del método/función/propiedad se define como "REFERENCIA A <tipo estructurado>" (en lugar de simplemente "<tipo estructurado>").
Tenga en cuenta que con este tipo de retorno, si, por ejemplo, se va a devolver una instancia local FB del tipo de datos estructurados, se debe usar el operador de referencia REF= en lugar del operador de asignación "normal" :=.
Las declaraciones y el ejemplo de esta sección se refieren a la llamada de una propiedad. Sin embargo, son igualmente transferibles a otras llamadas que ofrecen valores devueltos (por ejemplo, métodos o funciones).

Muestra

Declaración de la estructura ST_Sample (tipo de datos estructurado):

TYPE ST_Sample :
STRUCT
    bVar  : BOOL;
    nVar  : INT;
END_STRUCT
END_TYPE
Declaración del bloque de funciones FB_Sample:

FUNCTION_BLOCK FB_Sample
VAR
    stLocal     : ST_Sample;
END_VAR
Declaración de la propiedad FB_Sample.MyProp con el tipo de devolución "REFERENCIA A ST_Sample":

PROPERTY MyProp : REFERENCE TO ST_Sample
Implementación del método Get de la propiedad FB_Sample.MyProp:

MyProp REF= stLocal;
Implementación del método Set de la propiedad FB_Sample.MyProp:

stLocal := MyProp;
Llamando a los métodos Get y Set en el programa principal MAIN:

PROGRAM MAIN
VAR
    fbSample    : FB_Sample;
    nSingleGet  : INT;
    stGet       : ST_Sample;    
    bSet        : BOOL;
    stSet       : ST_Sample;
END_VAR
// Get - single member and complete structure possible
nSingleGet := fbSample.MyProp.nVar;
stGet      := fbSample.MyProp;
 
// Set - only complete structure possible 
IF bSet THEN
    fbSample.MyProp REF= stSet;
    bSet            := FALSE;
END_IF 
 

Mediante la declaración del tipo devuelto de la propiedad MyProp como "REFERENCE TO ST_Sample" y mediante el uso del operador de referencia REF= en el método Get de esta propiedad, se puede acceder a un único elemento del tipo de datos estructurados devuelto directamente al llamar a la propiedad.

VAR
    fbSample    : FB_Sample;
    nSingleGet  : INT;
END_VAR
nSingleGet := fbSample.MyProp.nVar;
Si el tipo de retorno solo se declarara como "ST_Sample", la estructura devuelta por la propiedad tendría que asignarse primero a una instancia de estructura local. Los elementos de estructura individuales podrían consultarse sobre la base de la instancia de estructura local.

VAR
    fbSample    : FB_Sample; 
    stGet       : ST_Sample; 
    nSingleGet  : INT;
END_VAR
stGet      := fbSample.MyProp;
nSingleGet := stGet.nVar;