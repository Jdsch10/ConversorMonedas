# Conversor de Monedas en Java

Este proyecto es una aplicación de consola en Java que permite a los usuarios convertir valores entre diferentes monedas utilizando tasas de cambio obtenidas a través de una API de tasas de cambio. A continuación, se detalla el funcionamiento del programa y los pasos necesarios para comprender su estructura y uso.

Requisitos Previos

Antes de ejecutar este programa, asegúrate de cumplir con los siguientes requisitos:

Java Development Kit (JDK): Versión 11 o superior instalada en tu sistema.

Biblioteca Gson: Archivo gson-2.11.0.jar disponible en el directorio libs del proyecto.

Conexión a Internet: Necesaria para realizar solicitudes a la API de tasas de cambio.

Configuración del Entorno: Asegúrate de tener configurado tu entorno de desarrollo (como Visual Studio Code) y estar familiarizado con el uso de la terminal.

Estructura del Proyecto

El proyecto está organizado de la siguiente manera:

ConversorMonedas/
├── libs/
│   ├── gson-2.11.0.jar
├── src/
│   ├── Main.java

libs/: Contiene el archivo JAR de la biblioteca Gson.

src/: Contiene el archivo principal Main.java.

Paso a Paso del Programa

1. Inicialización

El programa inicia creando un cliente HTTP (HttpClient) para realizar solicitudes a la API de tasas de cambio.

HttpClient client = HttpClient.newHttpClient();

El cliente se utiliza para enviar solicitudes y recibir respuestas de la API.

2. Menú Interactivo

Se presenta al usuario un menú con las siguientes opciones:

Convertir Moneda.

Salir del programa.

El menú se ejecuta dentro de un bucle que se repite hasta que el usuario seleccione la opción de salir.

System.out.println("\n--- Menú de Conversor de Monedas ---");
System.out.println("1. Convertir Moneda");
System.out.println("2. Salir");
System.out.print("Seleccione una opción: ");

3. Solicitud a la API

Cuando el usuario selecciona la opción de convertir moneda, el programa realiza una solicitud GET a la API para obtener las tasas de cambio actualizadas.

HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create(apiUrl))
    .header("Authorization", "dbe0603d0749600f9d53ad58")
    .build();

HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

El programa analiza el código de estado de la respuesta HTTP. Si es exitoso (código 200), procede con el procesamiento del JSON recibido.

4. Procesamiento de la Respuesta JSON

El cuerpo de la respuesta en formato JSON es procesado utilizando la biblioteca Gson. Se extraen las tasas de cambio desde el campo rates del JSON.

Gson gson = new Gson();
JsonObject jsonObject = JsonParser.parseString(jsonResponse).getAsJsonObject();
JsonObject rates = jsonObject.getAsJsonObject("rates");

5. Entrada del Usuario

El programa solicita al usuario la cantidad a convertir, la moneda de origen y la moneda de destino.

System.out.print("Ingrese la cantidad a convertir: ");
double amount = scanner.nextDouble();

System.out.print("Ingrese la moneda de origen (ejemplo: USD): ");
String fromCurrency = scanner.next().toUpperCase();

System.out.print("Ingrese la moneda de destino (ejemplo: COP): ");
String toCurrency = scanner.next().toUpperCase();

6. Conversión de Moneda

El programa utiliza las tasas de cambio extraídas del JSON para realizar la conversión. Si alguna de las monedas no está disponible en las tasas de cambio, muestra un mensaje de error.

public static double convertCurrency(double amount, String fromCurrency, String toCurrency, JsonObject rates) {
    double fromRate = rates.has(fromCurrency) ? rates.get(fromCurrency).getAsDouble() : 1;
    double toRate = rates.has(toCurrency) ? rates.get(toCurrency).getAsDouble() : 1;

    if (fromRate == 0 || toRate == 0) {
        System.out.println("Error: No se pudo encontrar la tasa de cambio para una de las monedas.");
        return -1;
    }

    return amount * (toRate / fromRate);
}

El resultado de la conversión se muestra al usuario.

System.out.printf("%.2f %s es equivalente a %.2f %s\n", amount, fromCurrency, convertedAmount, toCurrency);

7. Finalización del Programa

El programa permite al usuario repetir el proceso de conversión tantas veces como desee. Cuando el usuario selecciona la opción de salir, el programa termina.

System.out.println("\u00a1Gracias por usar el conversor de monedas! Hasta la próxima.");
scanner.close();

Ejemplo de Ejecución

Entrada:

--- Menú de Conversor de Monedas ---
1. Convertir Moneda
2. Salir
Seleccione una opción: 1
Ingrese la cantidad a convertir: 100
Ingrese la moneda de origen (ejemplo: USD): USD
Ingrese la moneda de destino (ejemplo: COP): ARS

Salida:

Código de estado: 200
Moneda base: USD
100.00 USD es equivalente a 9700.00 ARS

## Errores Comunes y Soluciones

"The import com.google cannot be resolved":

Verifica que el archivo gson-2.11.0.jar esté en el directorio libs.

Asegúrate de compilar y ejecutar con el comando correcto:

javac -cp libs/gson-2.11.0.jar src/Main.java
java -cp libs/gson-2.11.0.jar;src Main

Error al realizar la solicitud HTTP:

Verifica tu conexión a Internet.

Confirma que la URL de la API y la clave de autorización son correctas.

Moneda no encontrada:

Asegúrate de escribir correctamente los códigos de las monedas (ejemplo: USD, ARS, COP).

¡Disfruta usando tu Conversor de Monedas! Si encuentras algún problema, consulta las secciones de errores comunes y soluciones.