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


A continuación se muestra el código completo funcionando 
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



