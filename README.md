# ¿Qué es efacturapro?

**efacturapro** es un servicio Windows que permite emitir facturas electrónicas sin conectividad o con conectividad intermitente a Internet.

**efacturapro** recibe la información de las facturas mediante servicios web locales y se encarga de generar el formato XML estándar de factura, firmar la factura y administrar el envío a nuestro servicio PAC, cuando la conexión esté disponible.

Para utilizar **efacturapro** debe contar con una cuenta de **efacturapty**, la cual puede crear en [admin.efacturapty.com](admin.efacturapty.com)

# Instalación y configuración

1. Descargue el zip de [efacturapro Release v1.0.0](https://github.com/efacturapty/efacturapro/releases/tag/v1.0.1) y descomprima en una carpeta local.
2. Copie el certificado de firma electrónica en una carpeta local. El certificado de firma es el que posee la letra "F" al inicio del nombre del archivo.
3. Edite el archivo appsetting.json para configurar los siguientes valores:

   - **ElectronicCertificatePath**: Ruta y nombre del archivo de certificado de firma electrónica. Por ejemplo: *C:/Aplicaciones/efacturapro/firma/F-8-12-2332.P12*
   - **ElectronicCertificatePIN**: PIN para acceso al certificado de firma electrónica
   - **InvoicesPath**: Carpeta donde se almacenarán los archivos de las facturas pendientes a enviar, facturas autorizadas o facturas rechazadas. Por defecto se utiliza la carpeta efacturapro que se crea automáticamente al iniciar el servicio.
   - **APIKey**: API Key generado en el sitio admin.efacturapty.com
   - **InvoiceDefaults**: Valores por defecto para las facturas, siguiendo la estructura JSON de la factura según la [documetación del API](https://efacturapty.stoplight.io/docs/efactura-api/branches/main/7fj897tkblpck-emitir-una-factura-o-nota-de-credito-debito)
   - **Kestrel/Endpoints/Http/Url**: Permite establecer el puerto local que utilizará el servicio para recibir las peticiones
   - **URL_API_APPLICATION**: Debe ser idéntico a Kestrel/Endpoints/Http/Url. Es utilizado para procesar las peticiones que recibe la interfaz SOAP.
  
# Creación del servicio

Desde línea de comando, con privilegio de Administrador ejecutar:

`sc.exe create "efacturapro" binpath=ubicacion_efacturapro_exe`

Desde la consola de Servicios de Windows, puede configurar el servicio **efacturapro** para que inicie en forma automática, cada que inice windows.

![image](https://github.com/efacturapty/efacturapro/assets/16403179/c2de74b6-5fa7-4489-938a-755b39e47d9a)

# Servicio local API REST

Los *endpoints* habilitados son los siguientes

    POST	/api/v1/Invoices
    POST	/api/v1/Invoices/Async
    POST	/api/v1/InvoiceEvents/CreateCancellation
    POST	/api/v1/InvoiceEvents/CreateCancellationFromSoapWrpper
    GET	/api/v1/Invoices/GetCafe
    GET	/api/v1/Invoices/GetInvoiceXml
    GET	/api/v1/Invoices/GetCufe
    GET	/api/v1/Subscriptions/GetSummary
    GET	/api/v1/Subscriptions/GetSummaryByBranchOffice/{CodigoSucursal}
    GET	/api/v1/Taxpayers/QueryRucDvPac/{tipoContribuyente}/{ruc}
    GET	/api/v1/Taxpayers/GetDV/{tipoContribuyente}/{ruc}`

La especificación de cada uno de estos *endpoints* es idéntica a la disponible en la [documetación del API](https://efacturapty.stoplight.io/docs/efactura-api/branches/main/7fj897tkblpck-emitir-una-factura-o-nota-de-credito-debito).

La URL completa puede variar según la configuración del puerto definida en appsettings.json, pero si se mantienen los valores por defecto, el URL para la emisión de facturas sería:

`http://localhost:5421/api/v1/Invoices`

# Servicio local SOAP

El servicio SOAP de emisión de facturas se encuentra en el URL:

`http://localhost:5421/Service.asmx`


