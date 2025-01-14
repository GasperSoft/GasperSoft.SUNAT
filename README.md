# GasperSoft.SUNAT
[![NuGet Version](https://img.shields.io/nuget/v/GasperSoft.SUNAT)](https://www.nuget.org/packages/GasperSoft.SUNAT)
[![GitHub License](https://img.shields.io/github/license/LarrySoza/GasperSoft.SUNAT)](/LICENSE.txt)

Librerías .NET que permite generar los XML de la **Facturación Electrónica** en Perú, estas librerías están actualmente en producción y en constante actualización.

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
Utiliza código generado desde los **XSD oficiales** del estándar UBL/SUNAT **gracias a [UblXsdToCS]( https://github.com/LarrySoza/UblXsdToCS)**, estas clases contienen la estructura completa del estándar UBL/SUNAT, entonces implementar cualquier atributo adicional requerido parte de SUNAT es relativamente sencillo. Sin embargo dado la gran cantidad de propiedades que existen en el estándar UBL, en este proyecto se emplean objetos intermedios más sencillos que cumplen con todos los requerimientos de SUNAT, estas clases están definidas en **GasperSoft.SUNAT.DTO.dll** (CPEType, CREType, GREType, ResumenDiarioV2Type, ComunicacionBajaType), posteriormente después de asignar las propiedades correspondientes son convertidos usando la librería **GasperSoft.SUNAT.UBL.dll** a clases generadas desde los XSD (InvoiceType, DespatchAdviceType, SummaryDocumentsType, VoidedDocumentsType, RetentionType), finalmente son serializados a XML y firmado utilizando métodos definidos en **GasperSoft.SUNAT.dll**.

>[!NOTE] 
>Estas librería son compatible para net35, net40, net452, net462, net472, net481, netstandard2.0, net6.0, net7.0, net8.0 y net9.0, los métodos de envió a SUNAT no serán implementados para net35, net40 ni net452. La generación del zip en net35 y net40 es mediante la librería **DotNetZip** la cual contiene una vulnerabilidad relacionada con la descompresión no hay nada de qué preocuparse porque aún no implemento la lectura del CDR

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
  - [XML RESUMEN DIARIO DE BOLETAS - INFORMAR](/Xml/20606433094-RC-20241125-1.xml) - [C#](/Pruebas/ResumenDiario1.cs)
  - [XML RESUMEN DIARIO DE BOLETAS - DAR DE BAJA](/Xml/20606433094-RC-20241125-2.xml) - [C#](/Pruebas/ResumenDiario2.cs)
  - [XML COMUNICACION DE BAJA (SOLO FACTURAS)](/Xml/20606433094-RA-20241125-1.xml) - [C#](/Pruebas/ComunicacionBaja1.cs)
  - [XML RETENCION FACTURA SOLES](/Xml/20606433094-20-R001-1.xml) - [C#](/Pruebas/CRE1.cs)
  - [XML RETENCION FACTURA DOLARES - CON TIPO DE CAMBIO](/Xml/20606433094-20-R001-2.xml) - [C#](/Pruebas/CRE2.cs)
  - [XML REVERSION (BAJAS DE RETENCIONES)](/Xml/20606433094-RR-20241127-1.xml) - [C#](/Pruebas/ComunicacionBaja2.cs)

- Estos ejemplos son solo una forma de como incluir datos adicionales en el XML, SUNAT no lo exige y podrían incluirse en otras partes del documento, se tomó de ejemplo una Factura emitida por una entidad Financiera
 
  - [XML GUIA REMITENTE CON INFORMACION ADICIONAL EN 'UBLExtension'](/Xml/20606433094-09-T001-4.xml) - [C#](/Pruebas/GRERemitente4.cs)
  - [XML FACTURA CON INFORMACION ADICIONAL EN 'UBLExtension'](/Xml/20606433094-01-F001-10.xml) - [C#](/Pruebas/CPEFactura10.cs)

>[!NOTE] 
>El ejemplo **"FACTURA CON 4 ÍTEMS Y UNA BONIFICACIÓN - Pagina 77 Manual SUNAT"** es el ejemplo mas completo de todos porque combina el uso de productos grabados, exonerados, bonificaciones y descuentos por ítem y global, por lo que recomiendo darle una observación detalla. La clase **ValidadorCPE.cs** del proyecto **GasperSoft.SUNAT** debería poder ayudarte a corregir errores de cálculo, si no es el caso y pudiste generar el XML pero no paso las validaciones de SUNAT te agradecería que me mandes un ejemplo (como los que se implementan en el proyecto Pruebas) con los datos que llenas y el XML generado a [it@gaspersoft.com](mailto:it@gaspersoft.com), me ayudarías a colocar mas validaciones que no permitan generar XMLs con errores de calculo.


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

Si consideras que hay algún error en la validación o generación de un XML y no puede solucionarlo usando las fuentes, puedes serializar a JSON el objeto intermedio "CPEType", "CREType", "GREType", "ResumenDiarioV2Type" o "ComunicacionBajaType" y informarlo a **it@gaspersoft.com** detallando el problema.

El siguiente código genera un archivo "CPE.json" que ayudaría a replicar el XML que estas generando.
```C#
var _cpe = new CPEType();
//Codigo para asignar las propiedades

var _json = System.Text.Json.JsonSerializer.Serialize(_cpe);
File.WriteAllText("CPE.json", _json);
```

>[!NOTE] 
>Recomiendo hacer un Fork del proyecto e intentar darle solución por tus propios medios si dominas algo de C# es muy sencillo.
