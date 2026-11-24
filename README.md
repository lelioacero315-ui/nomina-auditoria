# Revisión de Nómina — Auditoría KACTUS

Herramienta de auditoría de nómina y seguridad social para entidades públicas del
orden territorial en Colombia. Lee la prenómina producida por el sistema KACTUS y
reconstruye el cálculo normativo para contrastarlo servidor por servidor.

**Aplicación:** https://lelioacero315-ui.github.io/nomina-auditoria/

## Qué verifica

- **Seguridad social** — IBC conforme al Prototipo 101 y al artículo 1 del Decreto
  1158 de 1994, con tope de 25 SMLMV, piso prorrateado según los días cotizados y
  Fondo de Solidaridad Pensional discriminado por subcuentas.
- **Riesgos laborales** — tarifa por clase de riesgo del centro de trabajo
  (Decreto 1072 de 2015, artículo 2.2.4.3.5), tomada de la planilla PILA del período
  o de la caracterización de planta. No se aplica una tarifa única a toda la entidad.
- **Retención en la fuente** — depuración del artículo 388 del Estatuto Tributario
  sobre la base del Prototipo 102, con la tabla del artículo 383, el límite de renta
  exenta de 790 UVT anuales y la aproximación del artículo 577. Identifica si el
  sistema aplicó Procedimiento 1 o 2 a partir del campo CANTIDAD del concepto 513.
- **Aportes parafiscales** — base del Prototipo 159, sin el tope de 25 SMLMV, que es
  propio de la seguridad social.
- **Novedades** — IBC del mes inmediatamente anterior en vacaciones, incapacidades y
  licencias de maternidad, paternidad o remuneradas (Decreto 1406 de 1999, artículo 1).
- **Bonificación por servicios prestados** — se causa cada vez que el servidor cumple
  un año continuo de labor (Decreto 1042 de 1978, artículo 45). La aplicación la
  contrasta contra la fecha de ingreso, corrida por las interrupciones del servicio.
- **Liquidación de vacaciones** — conceptos 107, 108, 118 y 155 contra la base de
  factores salariales del Decreto 1045 de 1978.
- **Control de descuentos** — embargo de alimentos (artículo 156 del C.S.T.), embargo
  civil (artículo 155) y libranzas (Ley 1527 de 2012, artículo 3).
- **Conciliación PILA** — cruce registro por registro contra la planilla del operador.

## Cómo se arma el IBC

El IBC de seguridad social no se copia de la planilla: se arma con los conceptos de la
prenómina que pertenecen al **Prototipo 101**, y después se contrasta contra la planilla
cargada. Por eso la hoja DATOS PRENÓMINA trae una columna *Otros Prot.101* que agrupa los
conceptos del prototipo sin columna propia: sin ella el libro sumaría cuatro conceptos en
lugar de los quince del prototipo.

El Prototipo 101 no incluye vacaciones (107) ni incapacidades (139, 141, 148). En un mes
con novedad, entonces, esa suma recoge únicamente los días laborados. Los días de la
novedad entran con el IBC del mes inmediatamente anterior, **prorrateado por esos días**
(Decreto 1406 de 1999, artículo 1), que es como lo liquida el operador: de ahí que el
servidor con novedad aparezca en la planilla en dos renglones, cada uno con su propio
IBC. La pestaña PILA deja esa cadena a la vista, servidor por servidor, y la hoja
NOVEDADES del libro la reproduce con fórmulas.

Cuando la novedad cubre el mes entero, o los días de novedad y los laborados no cuadran
con el mes, se aplica el IBC anterior completo y la aplicación lo dice en la columna de
regla, para que el caso se revise a mano.

Un punto que conviene tener claro: la planilla liquida el **aporte total** de cada
subsistema, no el descuento al servidor. La columna de salud es el 12,5 % y la de pensión
el 16 % más el Fondo de Solidaridad Pensional. El cruce espejo compara contra esas cifras;
el descuento al servidor (conceptos 500, 501 y 502) se verifica aparte, en la revisión de
la prenómina.

## La tarifa de ARL no es una sola

Conviene insistir en esto porque es el error que más plata mueve. La tarifa de riesgos
laborales la fija el **centro de trabajo** según la actividad que allí se desarrolla, no
la entidad como un todo. En la planilla de octubre de 2026 de Empleados Públicos, de 223
afiliados, 190 cotizan al 0,522 % (clase I), 28 al 1,044 %, dos al 4,350 % y solo tres al
6,960 %. Aplicar 6,96 % a toda la planta arrojaba $132.182.594 contra los $12.253.600
efectivamente liquidados: casi once veces el valor correcto.

La aplicación resuelve la tarifa en este orden:

1. La planilla PILA del período, que la trae por afiliado. Es la fuente preferida porque
   es la liquidación real del operador.
2. La columna *Clase riesgo ARL* de la caracterización de planta, que admite el romano
   (I a V), el arábigo (1 a 5) o la tarifa escrita (0,522 · 4,35 · 6,96).
3. El parámetro general, solo como último recurso. Cuando algún servidor cae en este
   caso, la verificación previa lo advierte y el panel de parafiscales cuantifica la
   diferencia entre una y otra forma de calcular.

Un detalle de la planilla que importa: el servidor con novedad en el mes aparece en **dos
renglones** —los días anteriores y los posteriores—, y en los días de novedad el operador
liquida ARL al 0,000 %. Esos renglones se consolidan como fracciones del mismo mes y la
tarifa cero se descarta para efectos de la clase de riesgo.

## Lectura del PDF de KACTUS

El reporte lleva impresa en vertical la marca de agua «Reporte Definición». pdf.js la
entrega como caracteres sueltos, uno por renglón, en una misma columna, y eso rompía la
lectura de dos maneras. La leve: la letra quedaba pegada a una palabra —«cFONSECA»,
«ADMnINISTRATIVO»—. La grave: la letra se dibuja a una altura intermedia entre dos
renglones, y como el agrupador anclaba el renglón en la posición del primer elemento que
encontraba, la letra corría el ancla y partía el renglón en dos, además invertido:

```
"c Salario Básico 3.654.949,00"
"47437504 FONSECA FONSECA FLOR MIRIAM"
```

Partido así, el encabezado no se reconocía, todos los conceptos de ese servidor se
acumulaban sobre el anterior, y el anterior tomaba como propia la línea TOTAL
IDENTIFICACION del siguiente. En la prenómina de junio de 2026 le pasaba a seis
servidores.

Ahora los glifos de la marca de agua se descartan **antes** de agrupar —ninguna celda real
del reporte es una letra sola— y el agrupamiento encadena contra el renglón anterior en
vez de anclarse en el primero. El interlineado del reporte es de unas catorce unidades y
los desfases dentro de un renglón no pasan de 1,1, así que no hay riesgo de fundir dos
renglones. Como red de seguridad queda un reensamblador que vuelve a unir el encabezado si
aun así llegara partido.

Y para que un fallo de lectura no vuelva a pasar inadvertido, hay un control independiente:
en el reporte de KACTUS cada servidor cierra con una línea TOTAL IDENTIFICACION, de modo
que si se reconocen menos encabezados que líneas de total, falta alguno. Ese control
**bloquea la entrega** y dice cuántos faltan.

## Historial de retención

En la pestaña Retención, la aplicación contrasta la retención de cada servidor contra la
de un mes archivado y **atribuye la diferencia a los conceptos que cambiaron**. Esa es la
parte que importa: saber que una retención subió no sirve de nada si no se sabe por qué.

Cada caso sale clasificado:

- **Variación explicada** — la base del Prototipo 102 se movió y los conceptos que
  cambiaron lo justifican. Entró una bonificación por servicios, una prima, una
  indemnización de vacaciones. El desglose dice cuál, cuánto antes, cuánto ahora.
- **Retención sin base** — la retención varió y la base no. Si el servidor cambió de
  procedimiento, la aplicación lo dice; si no, hay que mirar el porcentaje fijo del
  Procedimiento 2, las deducciones del artículo 387 o los aportes obligatorios.
- **Base sin retención** — la base subió y la retención no se movió. Puede ser un
  servidor marcado como no declarante, un cambio de procedimiento, o un concepto que
  quedó fuera del prototipo.
- **Sin conceptos identificados** — la base varió pero ningún concepto del Prototipo 102
  lo explica. Hay que mirar los conceptos que restan y la depuración del artículo 388.

También relaciona quiénes entraron a la nómina y quiénes salieron respecto del mes de
comparación, con lo que retenían.

El historial va al libro de Excel en la hoja **HISTORIAL RETENCIÓN**, y a la relación de
casos para la técnica —pero ahí solo los que no cuadran: una retención que subió porque
entró una prima es lo que debía pasar y no es trabajo para nadie.

Requiere que el mes de comparación esté archivado en la pestaña Histórico. Los meses
archivados con versiones anteriores de la aplicación no guardaban el detalle de retención
por servidor; si selecciona uno de esos, la aplicación lo dice y basta con volver a
archivar ese PDF.

## Por qué este mes sí se retuvo

La retención del Procedimiento 1 se liquida sobre el ingreso **del mes**, no sobre la
asignación básica. La tabla del artículo 383 del Estatuto Tributario empieza a cobrar por
encima de **95 UVT de base gravable** y por debajo de ese punto la tarifa es cero. De ahí
que a un servidor que nunca había tenido retención se le retenga el mes en que le entran
una bonificación, una prima o unas vacaciones: esos pagos suman a la base del mes y la
empujan sobre el umbral.

Para cualquier servidor con retención, la aplicación reconstruye la cadena completa: los
conceptos del mes que suman al Prototipo 102 uno por uno, los aportes obligatorios que
restan, la renta exenta del 25 % con su tope de 790 UVT anuales, la base gravable en pesos
**y en UVT**, el umbral de 95 UVT, el tramo que aplica y la retención aproximada al mil del
artículo 577.

Además calcula qué habría pasado sin los pagos adicionales del mes. Un servidor con
asignación de $4.271.774 queda en 56,28 UVT —sin retención—; con una bonificación por
servicios de $2.600.000 y una prima de $900.000 la base llega a 106,40 UVT y entra al tramo
del 19 %.

De ahí sale una **sustentación técnica en Word**. Se genera desde el selector que está al
comienzo del panel, con la lista de todos los servidores con retención del período: **no
hace falta tener un mes archivado**, porque la depuración se arma con el mes que esté
cargado. Si además hay un mes de comparación, la sustentación trae el contraste —cuánto fue
la base ese mes y por qué no hubo retención— y la aplicación lista aparte a quienes pasaron
de no tener retención a tenerla, cada uno con su propio botón.

El documento va dirigido a la Dirección de Talento Humano. No es una constancia al
servidor: quien revisa la nómina deja la observación, el técnico corrige o aclara en
KACTUS, y responderle al servidor —si llega a consultar— le corresponde a la Dirección. Por
eso habla del servidor en tercera persona y se firma como proyectado, no como certificado.

Trae la cadena paso a paso, el fundamento normativo, el efecto en los períodos siguientes
y la observación concreta. Si la nómina del período no liquidó esa retención, lo dice en un
aviso arriba y la observación pasa a ser que debe liquidarse antes de cerrar.

Los mismos casos entran a la relación para la técnica como numeral propio, con la
depuración resumida y su columna de respuesta, de modo que el soporte quede en el mismo
documento que ella devuelve.

## Verificación previa a la entrega

Antes de generar cualquier documento la aplicación corre una veintena de controles. Los que
detectan cifras incorrectas **bloquean la entrega**; los demás advierten y quedan como
limitaciones del alcance.

Entre los que bloquean: bloques del PDF cuyos conceptos no cuadran contra su propia
línea TOTAL IDENTIFICACION —lo que indica que se atribuyeron conceptos de otro
servidor—, novedades sin IBC del mes anterior, suma de devengados distinta al total y
parámetros de vigencia desactualizados.

Cada control que falla trae un botón **Corregir** que lleva a la pestaña donde está el
problema, activa el filtro que aísla los casos y resalta los campos pendientes.

## Entregables

**Libro de auditoría en Excel** — once hojas fijas y hasta cinco más según lo que se haya
cargado, todas con fórmulas vivas: al cambiar el SMLMV o la UVT en PARÁMETROS, el libro
entero se recalcula. Incluye una guía metodológica paso a paso, el mapa completo de
conceptos clasificados por naturaleza, la trazabilidad mensual, la tabla de clases de
riesgo de ARL y columnas de contraste que muestran cada base calculada por prototipo y por
el criterio anterior, con su diferencia.

Las hojas que dependen de lo cargado son **CRUCE ESPEJO PILA** y **CONCILIACIÓN PILA**
—requieren la planilla del operador—, **ASIGNACIÓN vs PLANTA** —requiere la
caracterización—, **HISTORIAL RETENCIÓN** —requiere un mes archivado— y **NOVEDADES**, que
solo aparece si las hubo en el período.

La hoja **CRUCE ESPEJO PILA** trae una fila por servidor con el IBC y los aportes que
exige la norma frente a los que liquidó el operador, tomados del archivo cargado, y la
diferencia calculada en el propio libro. El dictamen de conformidad de cada fila se forma
con el IBC, la salud y la pensión; la columna de ARL se deja a la vista aparte, porque en
los días de novedad el operador liquida al 0,000 % y esa diferencia es un hecho por
sustentar, no un error de cálculo.

La hoja **HISTORIAL RETENCIÓN** relaciona a cada servidor cuya retención varió frente al
mes archivado, con los conceptos que cambiaron escritos en una columna —para poder
filtrarlos— y la situación en que quedó cada caso.

**Informe de auditoría en Word** — hallazgos, recomendaciones y calificación bajo
metodología MECI. Va dirigido a la Dirección.

**Relación de casos para la técnica, en Word** — el mismo período visto desde el otro lado:
los casos por aclarar agrupados por tema, cada uno con la norma que lo sustenta y qué
revisar en KACTUS. No califica ni dictamina; es un documento de trabajo. Cada tabla lleva
una columna en blanco para que quien liquida anote si corrigió, si el caso no procede o qué
explica la diferencia, y el documento se devuelve diligenciado. Relaciona también lo que
quedó conforme, para no repetir revisión sobre lo que ya está bien.

**Sustentación técnica de retención, en Word** — el soporte de un caso puntual, con la
depuración paso a paso hasta el cruce del umbral de las 95 UVT. Se genera por servidor
desde la pestaña Retención.

**Acta Resumen FO-GD-11** — réplica del formato institucional, con el encabezado del
escudo repetido en todas las páginas.

**Calificación en Excel** — cinco hojas con fórmulas, incluido un plan de mejoras que
calcula estado y avance según las fechas diligenciadas.

## Privacidad

La aplicación funciona por completo en el navegador. **Los datos de nómina nunca se
envían a ningún servidor**: el PDF se procesa localmente y los resultados quedan en el
almacenamiento del propio navegador.

Por eso conviene descargar el respaldo con el botón correspondiente antes de cerrar.

## Uso

1. Abrir el enlace, o descargar `index.html` y abrirlo en el navegador.
2. Elegir el régimen: Empleados Públicos o Trabajadores Oficiales. Cada uno guarda su
   información por separado.
3. En la pestaña Histórico, archivar los meses anteriores. Con eso el IBC del mes
   anterior se aplica solo a los servidores con novedad.
4. Cargar el PDF de prenómina del período.
5. Cargar la planilla PILA. Además del cruce espejo, de ahí sale la tarifa de ARL de
   cada servidor.
6. Cargar el Excel de caracterización de planta de la DTH, para verificar los
   aniversarios de la bonificación por servicios, los retiros del mes y la asignación
   básica; y el acumulado de doce meses si hay servidores por Procedimiento 2.
7. Revisar los hallazgos en Elaborar informe y marcar la casilla de revisión.
8. Atender la verificación previa y generar los documentos.

Funciona sin conexión: todas las librerías están incorporadas en el archivo.

## Caracterización de planta

Se carga el **Excel que produce la Dirección de Talento Humano**, tal como lo entrega. No
hay que convertirlo ni recortarlo: la aplicación localiza las columnas por el texto del
encabezado —CÉDULA, NOMBRE Y APELLIDO, FECHA DE INGRESO, DENOMINACIÓN DEL CARGO, GRADO,
DEPENDENCIA, ESTADO ADMINISTRATIVO, SUELDO, GASTOS DE REPRESENTACIÓN—, de modo que el
archivo sigue sirviendo si se reordenan o se agregan columnas. También lee la hoja de
personal desvinculado con su fecha de retiro.

**Los gastos de representación van en su propia celda.** Son un factor de salario distinto
de la asignación básica (Decreto 1042 de 1978, artículo 42) y KACTUS los liquida en el
concepto 102, aparte del 101. Los dos entran al IBC y a la base prestacional, pero el 102
queda fuera de la base de retención, que es precisamente lo que excluye el Prototipo 102.
Por eso se contrastan por separado. Si en el libro de origen los dos valores vienen
juntos en una celda de texto —«18.249.487 + gastos de representación 881.278»—, la
aplicación los separa: el primer valor como asignación, el segundo como gastos, y lo
reporta al cargar para que se corrija en la fuente.

El libro de la DTH trae sesenta columnas, varias con datos personales sensibles: correo y
teléfono personales, dirección de residencia, RH, EPS, AFP, estrato, grupo étnico,
condición de salud, discapacidad, género y afiliación sindical. **Nada de eso se lee ni se
guarda.** Solo entran las columnas que la auditoría necesita.

Al cargarlo, la aplicación reporta qué leyó y qué le falta: cuántos servidores por
régimen, cuántos registros son vacantes sin cédula, cuántas fechas venían como texto en
lugar de fecha de Excel, y si el libro no trae clase de riesgo de ARL.

De ahí salen cinco controles nuevos en la verificación previa: el régimen de cada servidor
—que **bloquea** si un trabajador oficial aparece en la nómina de públicos o al contrario,
porque la liquidación prestacional no es la misma—, la asignación básica contra el sueldo
registrado, los gastos de representación contra el concepto 102, los servidores de la
prenómina que figuran como desvinculados en el mes, y los que no aparecen ni en la planta
ni entre los retiros.

Como alternativa se admite un CSV con
`Cédula,Nombre,Fecha ingreso,Cargo,Grado,Dependencia,Vinculación,Clase riesgo ARL,Asignación básica,Gastos de representación`,
donde solo las tres primeras columnas son obligatorias y la fecha admite `dd/mm/aaaa` o
`aaaa-mm-dd`.

Las interrupciones del servicio —licencias no remuneradas, suspensiones, comisiones sin
remuneración— se capturan a mano en la misma pestaña, una por línea:

```
4184305 LICENCIA NO REMUNERADA 01/03/2024 15/04/2024
```

Corren el aniversario tantos días como hayan durado, que es lo que define si la
bonificación por servicios se causó o no en el mes auditado.

## Parámetros de la vigencia

Se editan en la pestaña Parámetros y quedan guardados. Para 2026: SMLMV $1.750.905,
auxilio de transporte $249.095 y UVT $52.374 (Resolución DIAN 000238 de 2025). Las
tarifas por clase de riesgo de ARL están en la misma pestaña, a título informativo:
quien manda es la planilla o la planta.

## Lo que falta

- Las hojas VERIF. PRESTACIONES, PRESTACIONES (LIQUIDACIÓN) y COMPARATIVO del libro de
  referencia todavía no se generan.
- La convención colectiva de SINTRADECARE no está parametrizada para Trabajadores
  Oficiales; por ahora la prima de clima y la semestral usan valores aproximados.

## Advertencia

La herramienta apoya el criterio profesional; no lo reemplaza. Antes de fundamentar un
hallazgo o una decisión administrativa, contraste el resultado contra los soportes
documentales.

## Uso del repositorio

Repositorio de uso personal. Los datos contractuales del auditor vienen precargados en
la pestaña Elaborar informe; la cédula y el correo se diligencian una vez y quedan
guardados en el navegador.
