# Revisión de Nómina — Auditoría KACTUS

Herramienta de auditoría de nómina y seguridad social para entidades públicas del
orden territorial en Colombia. Lee la prenómina producida por el sistema KACTUS y
reconstruye el cálculo normativo para contrastarlo servidor por servidor.

**Aplicación:** https://lelioacero315-ui.github.io/nomina-auditoria/

## Qué verifica

- **Seguridad social** — IBC conforme al Prototipo 101 y al artículo 1 del Decreto
  1158 de 1994, con tope de 25 SMLMV, piso prorrateado según los días cotizados y
  Fondo de Solidaridad Pensional discriminado por subcuentas.
- **Retención en la fuente** — depuración del artículo 388 del Estatuto Tributario
  sobre la base del Prototipo 102, con la tabla del artículo 383, el límite de renta
  exenta de 790 UVT anuales y la aproximación del artículo 577. Identifica si el
  sistema aplicó Procedimiento 1 o 2 a partir del campo CANTIDAD del concepto 513.
- **Aportes parafiscales** — base del Prototipo 159, sin el tope de 25 SMLMV, que es
  propio de la seguridad social.
- **Novedades** — IBC del mes inmediatamente anterior en vacaciones, incapacidades y
  licencias de maternidad, paternidad o remuneradas (Decreto 1406 de 1999, artículo 1).
- **Liquidación de vacaciones** — conceptos 107, 108, 118 y 155 contra la base de
  factores salariales del Decreto 1045 de 1978.
- **Control de descuentos** — embargo de alimentos (artículo 156 del C.S.T.), embargo
  civil (artículo 155) y libranzas (Ley 1527 de 2012, artículo 3).
- **Conciliación PILA** — cruce registro por registro contra la planilla del operador.

## Verificación previa a la entrega

Antes de generar cualquier documento la aplicación corre diez controles. Los que
detectan cifras incorrectas **bloquean la entrega**; los demás advierten y quedan como
limitaciones del alcance.

Entre los que bloquean: bloques del PDF cuyos conceptos no cuadran contra su propia
línea TOTAL IDENTIFICACION —lo que indica que se atribuyeron conceptos de otro
servidor—, novedades sin IBC del mes anterior, suma de devengados distinta al total y
parámetros de vigencia desactualizados.

Cada control que falla trae un botón **Corregir** que lleva a la pestaña donde está el
problema, activa el filtro que aísla los casos y resalta los campos pendientes.

## Entregables

**Libro de auditoría en Excel** — doce hojas con fórmulas vivas: al cambiar el SMLMV o
la UVT en PARÁMETROS, el libro entero se recalcula. Incluye una guía metodológica paso
a paso, el mapa completo de conceptos clasificados por naturaleza, la trazabilidad
mensual y columnas de contraste que muestran cada base calculada por prototipo y por el
criterio anterior, con su diferencia.

**Informe de auditoría en Word** — hallazgos, recomendaciones y calificación bajo
metodología MECI.

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
5. Cargar la planilla PILA y, si hubo ingresos o retiros, las novedades de planta.
6. Revisar los hallazgos en Elaborar informe y marcar la casilla de revisión.
7. Atender la verificación previa y generar los documentos.

Funciona sin conexión: todas las librerías están incorporadas en el archivo.

## Parámetros de la vigencia

Se editan en la pestaña Parámetros y quedan guardados. Para 2026: SMLMV $1.750.905,
auxilio de transporte $249.095 y UVT $52.374 (Resolución DIAN 000238 de 2025).

## Advertencia

La herramienta apoya el criterio profesional; no lo reemplaza. Antes de fundamentar un
hallazgo o una decisión administrativa, contraste el resultado contra los soportes
documentales.

## Uso del repositorio

Repositorio de uso personal. Los datos contractuales del auditor vienen precargados en
la pestaña Elaborar informe; la cédula y el correo se diligencian una vez y quedan
guardados en el navegador.
