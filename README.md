# Revisión de Nómina — Auditoría KACTUS

Herramienta de auditoría de nómina y seguridad social para entidades públicas
del orden territorial en Colombia. Verifica la liquidación producida por el
sistema KACTUS contra el régimen salarial, prestacional y tributario aplicable.

## Qué hace

Lee el PDF de prenómina generado por KACTUS y reconstruye el cálculo normativo
para contrastarlo servidor por servidor:

* **Seguridad social** — IBC conforme al Prototipo 101 y al Decreto 1158 de 1994,
con tope de 25 SMLMV, piso proporcional a los días cotizados y Fondo de
Solidaridad Pensional por subcuentas.
* **Retención en la fuente** — depuración del artículo 388 del Estatuto Tributario
sobre la base del Prototipo 102, con la tabla del artículo 383 y el límite de
renta exenta de 790 UVT anuales.
* **Aportes parafiscales** — base del Prototipo 159, sin el tope de 25 SMLMV que
es propio de la seguridad social.
* **Novedades** — aplicación del IBC del mes anterior en vacaciones, incapacidad
y licencias, conforme al artículo 1 del Decreto 1406 de 1999.
* **Descuentos** — control de embargos de alimentos (artículo 156 CST), embargos
comerciales (artículo 155 CST) y libranzas (Ley 1527 de 2012).
* **Conciliación PILA** — cruce registro por registro contra la planilla del operador.

## Qué produce

* Libro de Excel con once hojas y fórmulas vivas: al cambiar el SMLMV o la UVT
en la hoja PARÁMETROS, el libro entero se recalcula.
* Informe de auditoría en Word con hallazgos, recomendaciones y calificación
bajo metodología MECI.
* Acta Resumen en el formato institucional FO-GD-11.
* Reporte de liquidación de vacaciones.

## Privacidad

La aplicación funciona por completo en el navegador. **Los datos de nómina nunca
se envían a ningún servidor**: el PDF se procesa localmente y los resultados se
guardan en el almacenamiento del propio navegador. Cerrar la pestaña o limpiar
los datos del sitio elimina toda la información.

Por esa razón conviene descargar el respaldo con el botón correspondiente antes
de cerrar la aplicación.

## Uso

1. Abrir el enlace, o descargar `index.html` y abrirlo en el navegador.
2. Seleccionar el régimen: Empleados Públicos o Trabajadores Oficiales.
3. Cargar el PDF de prenómina.
4. Opcionalmente, archivar meses anteriores en la pestaña Histórico para que el
IBC del mes anterior se aplique de forma automática.
5. Revisar los módulos y generar los informes.

Funciona sin conexión: todas las librerías están incorporadas en el archivo.

## Parámetros de la vigencia

Los valores se editan en la pestaña Parámetros y quedan guardados. Para 2026:
SMLMV $1.750.905, auxilio de transporte $249.095 y UVT $52.374 (Resolución DIAN
000238 de 2025).

## Advertencia

La herramienta apoya el criterio profesional; no lo reemplaza. Los resultados
deben verificarse contra los soportes documentales antes de fundamentar un
hallazgo o una decisión administrativa.

## Uso

Repositorio de uso personal. La aplicación contiene los datos contractuales
del auditor precargados en la pestaña Elaborar informe.

