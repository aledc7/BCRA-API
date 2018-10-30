# BCRA-API
### En este documento mostraré como consumir la API del Banco Central de la Republica Argentina con Ajax empleando jQuery.
###### Esto puede ser muy útil para obtener la cotización Oficial del dolar del día, y varias consultas mas que se mencionaran.

- [x] Ale Dc
- [x] jQuery
- [x] Código Probado


1-  Para poder consumir esta API es requisito autenticarse usando un token cada vez que se realice una llamada.  Dicho token nos lo provee el BCRA de manera gratuita, solo hay que registrarse con una cuenta de mail.

link de registro:
http://estadisticasbcra.com/api/registracion


2 - una vez registrados, la página del BCRA nos dará un token válido por un año desde la fecha de registro. 
Pasado el año será necesario registrar otro.

3- Utilizando Ajax co  Jquery, debemos incluir un Header de autorización con nuestro token, esto debe setearse antes de realizar el llamado, por lo que se utilizará el parámetro **beforeSend**   


A continuación se muestra el código completo funcionando en un archivo .html
__NOTA:__
(por razones de seguridad, he modificado mi token, ya que solo se permiten un maximo de 100 consultas al día, por lo que no quiero que me usen mis consultas... pueden generarse su propio token y reemplazarlo en el código.

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <title>AJAX usando la funcion $.ajax() de JQuery - AleDC </title>
</head>

<body>

        <!-- 
            El siguiente código conecta contra la API del BCRA para consultar la cotización del dolar, y luego la imprime en pantalla.
            Se usa AJAX con Jquery.
         -->



    <button id="boton">Leer Api BCRA</button>

    <!-- div principal en donde se dibujara los datos de la API -->
    <div id="lista-api"></div>


    <script>
        // asocia al evento click del boton la funcion leerApi 
        $("#boton").on("click", leerApi);


        function leerApi() {
            alert('Llamando a la API del BCRA, espere un cachito');

            $.ajax({
                url: 'http://api.estadisticasbcra.com/usd_of',
                type: 'GET',
                data:{d: "2004-12-31"},
                // con la propiedad beforeSend le paso el tipo de autorizacion, en este caso será 'Bearer'  y luego el token que registré en el BCRA
                beforeSend: function (xhr) {
                    xhr.setRequestHeader('Authorization', 'Bearer asdfqwerOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1NzIzNzcxNTEsInR5cGUiOiJleHRlcm5hbCIsInVzZXIiOiJhbGVqYasdsgsdfWNhc3Ryb0Bob3RtYWlsLmNvbSJ9.TO2eejZIyHHRD3A_yEu7W0DcMdwmuaCwsNLNAgwHS2CzJ5e74IV3a05j--X9-F-mITbrCzFgY-GtQlFg4KtdfQ');
                },

                // si la conexion es exitosa, ejecutará la función "respuesta", definida allí mismo.
                success: function (respuesta) {

                    // imprimo el valor de respuesta en la consola, para debug.
                    console.log(respuesta);

                    // creo una variable "listaAPI y le asigno el DIV que definí arriba, con el "id:lista-api".
                    var listaAPI = $("#lista-api");

                    $.each(respuesta, function (index, miembro) {
                        listaAPI.append(
                            '<div>' +
                            '<p>' + 'Fecha Cotizacion: ' + miembro.d + '<br>' +
                            'Importe: $ ' + miembro.v + '<br>' +
                            '<br>' + '______________________________________' +
                            '</div>'
                        );
                    });
                }, //fin function respuesta

                error: function () {
                    alert("Hubo un error");
                    console.log("Error al leer la API");
                }

            });
        }
    </script>

</body>

</html>
```
### Consultas disponibles:

#### A continuación se detallan las consultas que la API tiene disponibles:

```
http://api.estadisticasbcra.com/milestones : eventos relevantes (presidencia, ministros de economía, presidentes del BCRA, cepo al dólar)
http://api.estadisticasbcra.com/base : base monetaria
http://api.estadisticasbcra.com/base_usd: base monetaria dividida USD
http://api.estadisticasbcra.com/reservas : reservas internacionales
http://api.estadisticasbcra.com/base_div_res : base monetaria dividida reservas internacionales
http://api.estadisticasbcra.com/usd : cotización del USD
http://api.estadisticasbcra.com/usd_of : cotización del USD Oficial
http://api.estadisticasbcra.com/cuentas_corrientes : cuentas corrientes
http://api.estadisticasbcra.com/cajas_ahorro : cajas de ahorro
http://api.estadisticasbcra.com/plazo_fijo : plazos fijos
http://api.estadisticasbcra.com/otros_depositos : otros depositos
http://api.estadisticasbcra.com/tasa_int_dep : tasa de interés por depósitos
http://api.estadisticasbcra.com/tasa_badlar : tasa BADLAR
http://api.estadisticasbcra.com/tasa_baibar : tasa BAIBAR
http://api.estadisticasbcra.com/cer : CER
http://api.estadisticasbcra.com/uva : UVA
http://api.estadisticasbcra.com/uvi : UVI
http://api.estadisticasbcra.com/m2_privado_variacion_mensual : M2 privado variación mensual
http://api.estadisticasbcra.com/var_usd_vs_usd_of : porcentaje de variación entre la cotización del USD y el USD oficial
http://api.estadisticasbcra.com/lebac : LEBACs
http://api.estadisticasbcra.com/lebac_nominal : LEBACs (Nominal)
http://api.estadisticasbcra.com/circulacion_monetaria : circulación monetaria
http://api.estadisticasbcra.com/billetes_y_monedas : billetes y monedas
http://api.estadisticasbcra.com/efectivo_en_ent_fin : efectivo en entidades financieras
http://api.estadisticasbcra.com/depositos_cuenta_ent_fin : depositos de entidades financieras en cuenta del BCRA
http://api.estadisticasbcra.com/var_usd_anual : variación anual del dólar (porcentaje de variación de la cotización del dólar un año despues a la cotización de la fecha indicada)
http://api.estadisticasbcra.com/var_usd_of_anual : variación anual del dólar oficial (porcentaje de variación de la cotización del dólar oficial un año despues a la cotización de la fecha indicada)
http://api.estadisticasbcra.com/var_merval_anual : variación anual del MERVAL (porcentaje de variación del MERVAL un año despues al la cotización de la fecha indicada)
http://api.estadisticasbcra.com/merval : MERVAL
http://api.estadisticasbcra.com/merval_usd : MERVAL dividido cotización del USD
```


Esto es todo.

Documentación Oficial de la API:

http://estadisticasbcra.com/api/documentacion



-[x] Ale DC



