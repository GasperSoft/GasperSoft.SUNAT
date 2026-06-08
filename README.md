# GasperSoft.SUNAT
[![NuGet Version](https://img.shields.io/nuget/v/GasperSoft.SUNAT)](https://www.nuget.org/packages/GasperSoft.SUNAT)
[![GitHub License](https://img.shields.io/github/license/LarrySoza/GasperSoft.SUNAT)](/LICENSE.txt)

Conjunto de librerías .NET para generar los XML requeridos por la Facturación Electrónica en Perú. Proyecto en producción y con mantenimiento activo; se publican actualizaciones periódicas.

# Características #
- Generación de XML de los siguientes documentos electrónicos:
  - Facturas
  - Boletas
  - Notas de Crédito
  - Notas de Débito
  - Resumen Diario de Boletas
  - Comunicaciones de Baja
  - Retenciones
  - Guías de Remisión Remitente
  - Guías de Remisión Transportista

# Como Funciona
El código XSD oficial del estándar `UBL/SUNAT` se convierte a clases C# mediante `UblXsdToCS` (https://github.com/LarrySoza/UblXsdToCS). Las clases generadas (por ejemplo `InvoiceType`, `DespatchAdviceType`, `SummaryDocumentsType`, `VoidedDocumentsType`, `RetentionType`) representan la estructura completa del estándar. 

Para simplificar el uso y evitar exponer la gran cantidad de propiedades del modelo UBL, el proyecto ofrece objetos intermedios más sencillos en `GasperSoft.SUNAT.DTO.dll` —`CPEType`, `CREType`, `GREType`, `ResumenDiarioV2Type`, `ComunicacionBajaType`— que contienen solo lo necesario para cumplir los requisitos de SUNAT. Después de poblar estos DTO, se convierten a las clases generadas desde los XSD mediante `GasperSoft.SUNAT.UBL.dll`. Finalmente el resultado se serializa a XML y se firma utilizando los métodos de `GasperSoft.SUNAT.dll`.

Este flujo (XSD → clases generadas → DTOs → conversión → XML firmado) permite extender fácilmente atributos exigidos por SUNAT manteniendo el uso sencillo para desarrolladores.

>[!NOTE]
>Compatibilidad de Target Frameworks
>
>Las librerías son compatibles con: .NET Framework 3.5, 4.0, 4.5.2, 4.6.2, 4.7.2, 4.8.1, .NET Standard 2.0 y .NET 6/7/8/9.
>
>Consideraciones importantes:
>- Envío a SUNAT: los métodos de envío no están implementados para .NET Framework 3.5, 4.0 ni 4.5.2.
>- ZIP en entornos legados: la generación del ZIP en .NET Framework 3.5 y 4.0 utiliza la librería `DotNetZip`, que presenta una vulnerabilidad conocida relacionada con la descompresión. Aunque actualmente no se implementa la lectura del CDR en esos TFM (lo que reduce el alcance del riesgo), se recomienda evitar su uso en entornos de producción.
>- Recomendación: para disponer de todas las funcionalidades y de mejoras de seguridad, migre a un TFM moderno (por ejemplo, .NET 6 o superior).
>
>¿Necesita ayuda?: si requiere una alternativa segura para entornos legados o asistencia en la migración, abra un issue o contacte al mantenedor.


# Como se usa
- En el proyecto **Pruebas** encontrara ejemplos de código de como generar y firmar los XML, se usa certificado digital de prueba generado de manera gratuita en [LLAMA.PE](https://llama.pe/certificado-digital-de-prueba-sunat)(sin valor legal), actualmente se tienen los siguientes ejemplos:

  - [XML BOLETA DE VENTA GRAVADA CON DOS ÍTEMS Y UNA BONIFICACIÓN](/Xml/20606433094-03-B001-1.xml) - [Pagina 60 Manual SUNAT](/ManualesSunat/BoletaDeVentaElectronica2.1.pdf) - [C#](/Pruebas/CPEBoleta1.cs)
  - [XML BOLETA CON ICBPER - COBRANDO BOLSA](/Xml/20606433094-03-B001-2.xml) - [C#](/Pruebas/CPEBoleta2.cs)
  - [XML BOLETA CON ICBPER - REGALANDO BOLSA](/Xml/20606433094-03-B001-3.xml) - [C#](/Pruebas/CPEBoleta3.cs)
  - [XML BOLETA GRATUITA GRABADA - RETIRO POR ENTREGA A TRABAJADORES](/Xml/20606433094-03-B001-4.xml) - [C#](/Pruebas/CPEBoleta4.cs)
  - [XML FACTURA CREDITO (CUOTAS)](/Xml/20606433094-01-F001-1.xml) - [C#](/Pruebas/CPEFactura1.cs)
  - [XML FACTURA GRATUITA](/Xml/20606433094-01-F001-2.xml) - [Pagina 98 Manual SUNAT](/ManualesSunat/FacturaElectronica2.1.pdf) - [C#](/Pruebas/CPEFactura2.cs)
  - [XML FACTURA CONTADO CON DETRACCION](/Xml/20606433094-01-F001-3.xml) - [C#](/Pruebas/CPEFactura3.cs)
  - [XML FACTURA CON 4 ÍTEMS Y UNA BONIFICACIÓN](/Xml/20606433094-01-F001-4.xml) - [Pagina 77 Manual SUNAT](/ManualesSunat/FacturaElectronica2.1.pdf) - [C#](/Pruebas/CPEFactura4.cs)
  - [XML FACTURA CON 2 ÍTEMS E ISC](/Xml/20606433094-01-F001-5.xml) - [Pagina 88 Manual SUNAT](/ManualesSunat/FacturaElectronica2.1.pdf) - [C#](/Pruebas/CPEFactura5.cs)
  - [XML FACTURA CON ANTICIPOS - CON MONTO PENDIENTE DE PAGO](/Xml/20606433094-01-F001-6.xml) - [C#](/Pruebas/CPEFactura6.cs)
  - [XML FACTURA CON ANTICIPOS - MONTO TOTAL EN CERO](/Xml/20606433094-01-F001-7.xml) - [C#](/Pruebas/CPEFactura7.cs)
  - [XML FACTURA CON RETENCION](/Xml/20606433094-01-F001-8.xml) - [C#](/Pruebas/CPEFactura8.cs)
  - [XML FACTURA CON PERCEPCION](/Xml/20606433094-01-F001-9.xml) - [C#](/Pruebas/CPEFactura9.cs)
  - [XML FACTURA AL CONTADO PAGADO CON DEPOSITO EN CUENTA (MEDIO DE PAGO CATALOGO N° 59)](/Xml/20606433094-01-F001-11.xml) - [C#](/Pruebas/CPEFactura11.cs)
  - [XML NOTA CREDITO MOTIVO 13](/Xml/20606433094-07-F001-1.xml) - [C#](/Pruebas/CPENotaCredito1.cs)
  - [XML GUIA REMISION REMITENTE - Transporte Publico](/Xml/20606433094-09-T001-1.xml) - [C#](/Pruebas/GRERemitente1.cs)
  - [XML GUIA REMISION REMITENTE - Transporte Privado (Vehiculo y Conductor)](/Xml/20606433094-09-T001-2.xml) - [C#](/Pruebas/GRERemitente2.cs)
  - [XML GUIA REMISION REMITENTE - Transporte Privado (M1 o L)](/Xml/20606433094-09-T001-3.xml) - [C#](/Pruebas/GRERemitente3.cs)
  - [XML GUIA REMISION REMITENTE - EXPORTACION (PENDIENTE DE VERIFICACION CON SUNAT)](/Xml/20606433094-09-T001-5.xml) - [C#](/Pruebas/GRERemitente5.cs)
  - [XML GUIA REMISION TRANSPORTISTA](/Xml/20606433094-31-V001-1.xml) - [C#](/Pruebas/GRETransportista1.cs)
  - [XML RESUMEN DIARIO DE BOLETAS - INFORMAR](/Xml/20606433094-RC-20260608-1.xml) - [C#](/Pruebas/ResumenDiario1.cs)
  - [XML RESUMEN DIARIO DE BOLETAS - DAR DE BAJA](/Xml/20606433094-RC-20260608-2.xml) - [C#](/Pruebas/ResumenDiario2.cs)
  - [XML COMUNICACION DE BAJA (SOLO FACTURAS)](/Xml/20606433094-RA-20241125-1.xml) - [C#](/Pruebas/ComunicacionBaja1.cs)
  - [XML RETENCION FACTURA SOLES](/Xml/20606433094-20-R001-1.xml) - [C#](/Pruebas/CRE1.cs)
  - [XML RETENCION FACTURA DOLARES - CON TIPO DE CAMBIO](/Xml/20606433094-20-R001-2.xml) - [C#](/Pruebas/CRE2.cs)
  - [XML REVERSION (BAJAS DE RETENCIONES)](/Xml/20606433094-RR-20241127-1.xml) - [C#](/Pruebas/ComunicacionBaja2.cs)

- Estos ejemplos son solo una forma de como incluir datos adicionales en el XML, SUNAT no lo exige y podrían incluirse en otras partes del documento, se tomó de ejemplo una Factura emitida por una entidad Financiera
 
  - [XML GUIA REMITENTE CON INFORMACION ADICIONAL EN 'UBLExtension'](/Xml/20606433094-09-T001-4.xml) - [C#](/Pruebas/GRERemitente4.cs)
  - [XML FACTURA CON INFORMACION ADICIONAL EN 'UBLExtension'](/Xml/20606433094-01-F001-10.xml) - [C#](/Pruebas/CPEFactura10.cs)

>[!NOTE] 
>El ejemplo FACTURA CON 4 ÍTEMS Y UNA BONIFICACIÓN (Página 77 del Manual SUNAT) es el más completo: combina ítems gravados y exonerados, bonificaciones y descuentos por ítem y globales, por lo que conviene revisarlo con detalle. La clase ValidadorCPE.cs del ensamblado GasperSoft.SUNAT está diseñada para detectar y corregir errores de cálculo; si, pese a ello, el XML no supera las validaciones de SUNAT, envíe a it@gaspersoft.com un caso reproducible incluyendo:
>-	El objeto DTO (CPEType, CREType, etc.) exportado a JSON.
>-	El XML generado.
>-	Una breve descripción del problema y los pasos para reproducirlo.
>Con esa información se podrán implementar validaciones adicionales que eviten la generación de XML con errores de cálculo.

<p align="center">
  <img src="https://img.shields.io/badge/UTF-8%20SOPORTADO-brightgreen?style=for-the-badge" alt="UTF-8 Soportado" />
</p>

## ✨ Soporte UTF‑8 — Generación de XML en codificación UTF‑8

La librería puede serializar los XML en UTF‑8. SUNAT exige XML en UTF‑8 sin BOM (Byte Order Mark), por lo que es importante controlar explícitamente la codificación al serializar, firmar y guardar/comprimir los archivos.

Recomendación:
- Pase un `Encoding` explícito con BOM deshabilitado para garantizar compatibilidad con SUNAT: `new UTF8Encoding(false)`.

Ejemplo (recomendado):

```C#
var _encoding = new UTF8Encoding(false);

var _xml = XmlUtil.Serializar(_cpeType, _encoding);

//Firmar el XML y obtener el digestValue
var _xmlFirmado = XmlUtil.FirmarXml(_xml, certificado, out digestValue, signature, _encoding);

//Generar zip
var _bytesZip = XmlUtil.Comprimir(_xmlFirmado, "20606433094-01-F001-1.xml", _encoding);
```


# Validar XML generado
- Se puede validar el XML generado en [NUBEFACT](https://probar-xml.nubefact.com), Sin embargo debe considerar que el solo hecho de copiar y pegar en esta página podría adulterar el contenido del XML y tener un mensaje de error 2335(Como en el ejemplo de "FACTURA CONTADO CON DETRACCION"), de ser ese el caso puede marcar la opcion Firmar.

![ValidarXml](https://github.com/user-attachments/assets/7f9edb32-7c83-4c02-9c8f-f47972ed8a49)

>[!NOTE] 
>A la fecha 24-11-2024 la pagina de nubefact no valida los XML de Guia Transportista

# Envio a SUNAT
- De momento este proyecto se enfoca exclusivamente en la generación de los XML, puede encontrar código de envió a SUNAT en [OpenInvoice](https://github.com/erickorlando/openinvoiceperu)

>[!NOTE] 
>Actualmente existe una Clase para el envió de Guías Electrónicas usando **GasperSoft.SUNAT.dll** [ClientGRE.cs](/GasperSoft.SUNAT/ClientGRE.cs). Un ejemplo de uso en [EnvioGRE1.cs](/Pruebas/EnvioGRE1.cs)


# Asesoría y Soporte

Si encuentras un error en la generación o validación de un XML y no puedes resolverlo con la documentación, exporta el objeto intermedio (`CPEType`, `CREType`, `GREType`, `ResumenDiarioV2Type` o `ComunicacionBajaType`) a JSON y envíalo a `it@gaspersoft.com` junto con:
- Una breve descripción del problema.
- El XML generado (si lo tienes).
- El JSON exportado (permite reproducir exactamente los datos).

El siguiente ejemplo crea `CPE.json`. Usa `System.Text.Json`; si tu TFM no lo soporta, puedes usar `Newtonsoft.Json` (Json.NET).

```C#
var _cpe = new CPEType();
//Codigo para asignar las propiedades

var _json = System.Text.Json.JsonSerializer.Serialize(_cpe);
File.WriteAllText("CPE.json", _json);
```

>[!NOTE] 
>Si domina C#, haga un fork del proyecto, implemente la solución y envíe un pull request con su contribución.