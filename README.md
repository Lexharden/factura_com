# Factura Com Librería

Librería Python para interactuar con la API de Factura.com.
Hay que registrarse en el sitio web de factura.com para poder obtener el API_KEY y SECRET_KEY.

Más detalles en [Factura.com](https://factura.com/)
## Instalación

Puedes instalar la librería utilizando pip:

```bash
pip install factura-com-library
```

## Uso/Ejemplos
### Configuracion Inicial
```python
from factura_com.api import FacturaComClient

api_key = "TU_API_KEY"
secret_key = "TU_SECRET_KEY"
sandbox_url = "https://sandbox.factura.com/api/v4"
live_url = "https://api.factura.com/api/v4"

client = FacturaComClient(api_key, secret_key, url=sandbox_url)
```
Utilizar la url sandbox para hacer pruebas ó live para producción.

### Obtener una lista de facturas
```python
from factura_com.api import FacturaComClient

api_key = "TU_API_KEY"
secret_key = "TU_SECRET_KEY"
sandbox_url = "https://sandbox.factura.com/api/v4"
live_url = "https://api.factura.com/api/v4"

client = FacturaComClient(api_key, secret_key, url=sandbox_url)

# Obtener una lista de facturas para un año y RFC específicos

invoice_list = client.get_invoice_list(year=2024, rfc='XAXX010101000')

print(invoice_list)
```

Obtener todas las facturas pasando como parametro el RFC o Año.
Parametros:
-  month (int, opcional): El número de mes que deseas consultar (por ejemplo, 01 para enero).
- year (int, opcional): El año que deseas consultar (por ejemplo, 2024).
- rfc (str, opcional): El RFC para filtrar las facturas.
- type_document (str, opcional): El tipo de CFDI para listar solo las facturas de ese tipo.
- page (int, opcional): El número de página a consultar. Por defecto, es la página 1.
- per_page (int, opcional): El límite de resultados por página. Por defecto, retorna 100 registros.

### Obtener una factura por UID
```python
# Obtener una factura por UID

invoice_by_uid = client.get_invoice_by_uid(uid='55c0fdc67593d')

print(invoice_by_uid)
```

Obtener una factura en específico pasando como parametro el uid.
- uid (str): El UID de la factura que se desea obtener.

### Crear una nueva factura versión 4.0
```python
invoice_data = {
            "Receptor": {
                "ResidenciaFiscal": "",
                "UID": "55c0fdc67593d"
            },
            "TipoDocumento": "factura",
            "BorradorSiFalla": "1",
            "Draft": "1",
            "Conceptos": [
                {
                    "ClaveProdServ": "43232408",
                    "NoIdentificacion": "0021",
                    "Cantidad": "1.000000",
                    "ClaveUnidad": "E48",
                    "Unidad": "Unidad de servicio",
                    "Descripcion": "Desarrollo web a la medida",
                    "ValorUnitario": "15000.000000",
                    "Importe": "15000.000000",
                    "Descuento": "0",
                    "Impuestos": {
                        "Traslados": [
                            {
                                "Base": "15000.000000",
                                "Impuesto": "002",
                                "TipoFactor": "Tasa",
                                "TasaOCuota": "0.16",
                                "Importe": "2400.000000"
                            }
                        ],
                        "Retenidos": [],
                        "Locales": []
                    }
                }
            ],
            "UsoCFDI": "G01",
            "Serie": 1247,
            "FormaPago": "01",
            "MetodoPago": "PUE",
            "CondicionesDePago": "Pago en 9 meses",
            "CfdiRelacionados": {
                "TipoRelacion": "01",
                "UUID": [
                    "29c98cb2-f72a-4cbe-a297-606da335e187",
                    "a96f6b9a-70aa-4f2d-bc5e-d54fb7371236"
                ]
            },
            "Moneda": "MXN",
            "TipoCambio": "19.85",
            "NumOrder": "85abf36",
            "Fecha": "2020-03-20T12:53:23",
            "Comentarios": "El pedido aún no es entregado",
            "Cuenta": "0025",
            "EnviarCorreo": "true",
            "LugarExpedicion": "12345"
        }

created_invoice = client.create_invoice_4_0(invoice_data)

```

Primeramente se tiene que crear un diccionario.
Parametros:
- invoice_data (dict): Los datos de la factura a crear.
### Obtener el PDF de una factura
```python
# Obtener el PDF de una factura por su UID ó UUID
pdf_data = client.get_invoice_pdf(cfdi_uuid='55c0fdc67593d')
```

### Obtener el XML de una factura
```python
# Obtener el XML de una factura por su UID ó UUID
xml_data = client.get_invoice_xml(cfdi_uuid='55c0fdc67593d')
```

### Cancelar una factura
Para tener mas detalles de los tipos de motivo, hacer clic [aquí](https://factura.com/apidocs/cancelar-cfdi-40.html)
```python
# Cancelar una factura por su UUID
cancel_result = client.cancel_invoice(cfdi_uuid='55c0fdc67593d', motivo='01', folio_sustituto='3336cbb9-ebd4-45e8-b60b-e7bfa6f6b5e0')
```
