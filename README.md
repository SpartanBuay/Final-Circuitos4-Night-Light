# Final-Circuitos4-Night-Light
Ulatina, proyecto final circuitos 4/ Luz de noche manipulable 

# <p align="center">  💡Luz de Noche Manipulable📱  <p align="center"> Proyectofinal de Circuitos y Sistemas IV

Para el proyecto final de Circuitos y Sistemas IV se decidió crear una luz de noche
(un prototipo, para ser más específicos) que se pueda manipular por wifi y
físicamente con potenciómetro(s), tanto en color como en intensidad y que ayude a
iluminar en la noche sin perturbar mucho la oscuridad.
En esta documentación se presentarán los detalles básicos de sus partes, código y
funcionamiento.

> ### Para ello empecemos con los componentes del prototipo…
------------------------------------------------------------

Inicialmente se pensaba en utilizar fotoresistores para crear una luz de noche que
responda automáticamente con la luz que le rodea pero, debido a circunstancias
fuera de nuestro control se optó por utilizar otros materiales y darle un
funcionamiento diferente que explicara más adelante.

* Para el prototipo se utilizó:📝
  - ESP32
  - 2 LEDs rgb cátodo común
  - 6 resistencias de 330 ohms
  - 4 resistencias de 220 ohms
  - Potenciómetro de 10k ohms
  - cables
  - fuente de alimentación externa
  
### <p align="center"> 📷Imagenes del proyecto 📷
<p align="center">
¡<img src="https://user-images.githubusercontent.com/89326280/130339721-e59b5aa0-0cc8-408f-9917-c2e0ffb471e9.png" width="300">
<img src="https://user-images.githubusercontent.com/89326280/130339940-2f6e72d1-d438-494b-9502-c10f5f81e960.png" width="300">
<img src="https://user-images.githubusercontent.com/89326280/130340057-90923919-48f7-4e1c-84b2-8a096c32d3e7.png" width="275">
  
  ### **[Video Del Funcionamiento](https://drive.google.com/file/d/1mtfiSnPACxtCBxi_FRVO39Qx-CYtJV2Y/view?usp=sharing).** 🔗
  
## **Funcionamiento**🔧 
 En el código se están implementando tres fundamentos del microcontrolador a la vez:
  - Generación, manipulación y uso de una señal PMW.
    En el código utilizamos 3 canales, un canal por color, para controlar la combinaciones de
    colores a dar en el led rgb. Inicialmente se definen variables globales para cada uno de los
    canales definiendo parámetros como la frecuencia del pulso y la resolución de la señal (la
    cual fue dada en 10 bits, en otras palabras el pulso puede ser emitido en 1024 intensidades
    diferentes las cuales son la división en partes iguales del voltaje máximo de salida.
  
 - Conversión ADC input en resolución de 10 bits.
    Para el control del nivel de brillo, el microcontrolador recibe una señal análoga y la convierte
    en digital asignándole un valor a la lectura entre 0 y 1023 (10 bits de resolución, como la
    señal PMW), este valor es asignado en una variable para luego ser comparado en una serie
    de rangos y definir a qué porcentaje del valor obtenido para cada color realmente vamos a
    mandar la señal pmw al led.
    Por ejemplo, si el rango leído del potenciometro cae en 50% y el usuario aplica en la página
    un valor de 1000 para el color rojo y 0 para los otros colores, en lugar de mandar una señal
    pmw al canal del led rojo de 1000 directamente, antes el valor se multiplica por la variable
    “brillo” la cual para este caso tendrá un valor de 0.5 dándole un valor final a la señal de 500.
    Esto permite mantener la combinación de colores entre los tres canales relativamente
    consistente mientras que se da la oportunidad de controlar la luminosidad final sin perder la
    combinación de colores deseada
  
  - Uso de el microcontrolador como web server (gracias a la librería WiFi.h para esp32)
    Para poder poner una página web cómo interfaz y controlar los colores y el encendido y
    apagado de la luz nocturna primero se necesita montar un servidor. La librería de la esp32
    trae algunos ejemplos de la implementación y uso de la librería WiFi.h tanto como servidor,
    como host. Una vez se da el modo de operación creamos dos variables para el nombre de
    la red a conectar y la contraseña de la red (en este caso las variables se llaman ssid y
    password respectivamente.
    Para este caso se ha configurado de manera previa una dirección estática en el router y se
    le han asignado estos valores a el microcontrolador en la parte del setup (ip, mascara
    subred y gateway), en caso de no querer hacer esto simplemente se omite la linea
    “WiFi.begin(ip, máscara de subred, gateway);” en la parte del setup y WiFi.begin con el
    nombre de red y contraseña se conecta a la red, luego el router le dará una dirección por
    dhcp (si tiene la configuracion predeterminada) y esta direccion ip sera impresa en el serial,
    la cual será la dirección a ingresar para dar instrucciones a la esp32 a través de la página
    web en la red local.
    Una vez la conexión es establecida y la negociación http es realizada, podemos mandar
    html a el cliente y obtener cambios realizados en la dirección los cuales se pueden tomar
    como condiciones para instrucciones en el mictrocontrolador, con esto podemos saber
    cuando el cliente en la pagina mueve el valor de una de las barras de “nivel de color” o Hue,
    presiona el boton de encendido o apagado y proceder con estas instrucciones.

## ⚡Esp32 - devkit-v1⚡
  Para el prototipo se escogió utilizar el microcontrolador ESP32-DevKit-V1 de 30 Pines con
  Wifi y Bluetooth.
  
> ### Algunos de los datos más relevantes del microcontrolador sacado de su documentación:
  - El ESP32 DEVKIT V1 es un microcontrolador de bajo costo y consumo de energía,
    cuenta con tecnología Wi-Fi y Bluetooth de modo dual integrada que permite
    controlar todo tipo de sensores, módulos y actuadores; es el sucesor del
    microcontrolador ESP8266.
  - Permite generar proyectos de Internet de las cosas “IoT” de forma eficiente y
    económica, ya que integra internamente una gran cantidad de periféricos incluyendo:
    sensores táctiles capacitivos, sensor de efecto Hall, amplificadores de bajo ruido,
    interfaz para tarjeta SD, Ethernet, SPI de alta velocidad, UART, I2S e I2C. Tiene un
    CPU de dos núcleos de hasta 240Mhz que se pueden controlar independientemente.
  
  - Características específicas:
    - Serie: ESP32 DEVKIT V1
    - Chip USB-Serial: CP2102
    - Voltaje de Alimentación (USB): 5V DC
    - Voltaje de Entradas/Salidas: 3.3V DC
    - Consumo de energía de 5μA en modo de suspensión
    - Pines Digitales GPIO: 24 (Algunos pines solo como entrada)
    - Conversor Analógico Digital:
      - Dos ADC de 12bits tipo SAR, soporta mediciones en hasta 18
        canales, algunos pines soporta un amplificador con ganancia
        programable
    - Antena en PCB
    - Tipo: Módulo Wifi + Bluetooth
    - Wifi: 802.11 b/g/n/e/i (802.11n @ 2.4 GHz hasta 150 Mbit/s)
    - Bluetooth: 4.2 BR/EDR BLE Modo de control dual
    - CPU principal: Tensilica Xtensa 32-bit LX6
    - Memoria: 448 KByte ROM, 520 KByte SRAM, 6 KByte SRAM en RTC y QSPI
      admite múltiples chips flash /SRAM
    - Procesador secundario: Permite hacer operaciones básica en modo de ultra
      bajo consumo
    - Desempeño: Hasta 600 DMIPS
    - Frecuencia de Reloj: hasta 240Mhz
    - Seguridad: IEEE 802.11, incluyendo WFA, WPA/WPA2 y WAPI
    - Criptografía acelerada por hardware: AES, SHA-2, RSA, criptografía de curva
      elíptica (ECC), generador de números aleatorios (RNG)
  
### **ESP32 [Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf).** 🔗
  
## Diagrama📄
  <p align="center">
  <img src="https://user-images.githubusercontent.com/89326280/130342282-255ca9ba-5025-4b6e-bfcf-035ed4f835b8.png" width="350">
  
## Código 👨‍💻
     #include <WiFi.h>

    //-------------------------------------------------Inicio Página Web----------------------------------------------------------------------

    //Parte que siempre será impresa, podemos meterlo todo en una variable para facilitar lectura en el void loop
    String Head = 
    "<!DOCTYPE html>"
    "<html>"
    "<head>"
      "<meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">"
      "<link rel=\"icon\" href=\"data:,\">"
      "<style>"
        "body {"
          "text-align: center;"
          "font-family: \"Trebuchet MS\", Arial;"
          "margin-left:auto;"
          "margin-right:auto;"
          "background-color: rgb(67, 90, 100);"
          "color: whitesmoke;"
        "}"
        ".slider {"
          "width: 70%;"
        "}"

        ".red{"
         " color: orangered;"

        "}"

        ".blue{"
          "color: lightblue;"

        "}"

        ".green{"
         " color: lightgreen;"

        "}"
        ".on{"
          "background-color: green;"
          "color: whitesmoke;"
          "width: 90px;"
          "height: 90px;"
        "}"
        ".off{"
          "background-color: darkslategray;"
          "color: red;"
          "width: 90px;"
          "height: 90px;"
        "}"

      "</style>"
      "<script src=\"https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js\"></script>"
    "</head>"
    "<body>"
      "<h1>Luz Nocturna</h1>";


    //---------------------------Botones en la Web--------------------
    //Boton de Encendido
    String On = "<p><a href=\"/on\"><button class=\"on\">ON</button></a></p>";

    //Boton de Apagado
    String Off = "<p><a href=\"/off\"><button class=\"off\">OFF</button></a></p>";


    //--------------------------------------------------------Parte para web server-----------------------------------------------------------
    const char* ssid = "NOMBRE DE TU RED WIFI";
    const char* password = "CONTRASEÑA DE TU RED WIFI";

    WiFiServer server(80);

    //static values for dhcp, solo si se ha configurado en el router de manera previa
    /*
    IPAddress ip(192,168,0,69);
    IPAddress gateway(192,168,0,1);
    IPAddress subnet(255,255,255,0);
    */

    //Para almacenar el http request
    String header;



    //El valor con el que se controlara cada color
    String valHue = String(5);
    //strings para "decodificar" el valor de los sliders, ya que vienen de un javascript
    int pos1;
    int pos2;




    // Current time
    unsigned long currentTime = millis();
    // Previous time
    unsigned long previousTime = 0; 
    // Define timeout time in milliseconds (example: 2000ms = 2s)
    const long timeoutTime = 2000;


    //----------------------------------------------------------Parte para los leds-----------------------------------------------------------
    //LED pin para 2
    const int ledpin1r = 25;
    const int ledpin1g = 26;
    const int ledpin1b = 27;

    const int ledpin2r = 14;
    const int ledpin2g = 12;
    const int ledpin2b = 13; 

    //-------------------------------Para el Boton (el estado del ciclo anterior a el que se está ejecutando)---------------------------------
    String prevstat = "off";


    //--------------------------------------------------Frecuencias de Pmw y eso---------------------------------------------------------------

    //Channel for red color
    const int freqR = 5000; //5000Hz
    const int ledChannR = 0;
    const int resolutionR = 10;

    //Channel for green color
    const int freqG = 5000; //5000Hz
    const int ledChannG = 1;
    const int resolutionG = 10;

    //Channel for blue color
    const int freqB = 5000; //5000Hz
    const int ledChannB = 2;
    const int resolutionB = 10;



    //---------------------------------------------------------------------- Para el ADC ------------------------------------------------------
    const int potPin = 36;      //seleccionar el pin de entrada para el pot
    int potValue;       
    float brillo;

    //Variables para "memorizar" el ultimo cambio al color, y cambiar la intensidad sin depender de un get del web
    int MemR = 0;
    int MemG = 0;
    int MemB = 0;


    //--------------------------------------------------------------------------Loop Setup---------------------------------------------------------------------------------------------------------
    void setup() {
    //PMW setup

      //LED PMW func
      ledcSetup(ledChannR, freqR, resolutionR);
      ledcSetup(ledChannG, freqG, resolutionG);
      ledcSetup(ledChannB, freqB, resolutionB);
      //GPIO attachment
      ledcAttachPin(ledpin1r, ledChannR);
      ledcAttachPin(ledpin2r, ledChannR);
      ledcAttachPin(ledpin1g, ledChannG);
      ledcAttachPin(ledpin2g, ledChannG);
      ledcAttachPin(ledpin1b, ledChannB);
      ledcAttachPin(ledpin2b, ledChannB);


    //web setup  
      Serial.begin(115200);
      // Connect to Wi-Fi network with SSID and password
      Serial.print("Connecting to ");
      Serial.println(ssid);

    //Usar el Wifi.config solo si se ha seteado dirección IP fija en el router, de caso contrario no utilizar
    //este comando y omitir las variables ip, gateway y subnet; el router dará una ip por dhcp y esta será impresa en serial  
    //  WiFi.config(ip, gateway, subnet);

      WiFi.begin(ssid, password);
      while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.println("Me estoy tratando de conectar :'( ");
      }

      // Print local IP address and start web server
      Serial.println("");
      Serial.println("WiFi connected. :D");
      Serial.println("IP address: ");
      Serial.println(WiFi.localIP());
      server.begin();


    //ADC setup
       analogReadResolution(10);
       analogSetClockDiv(1);
    }

    void loop() {
    //Lo primero que hacemos fuera de la evaluación de la página web y http request 
    //es obtener nuestro valor de brillo para este ciclo (leído desde el potenciometro)
    potValue = analogRead(potPin);

    if (potValue < 256){
     brillo = 0.25;
      }

    else if (potValue < 512 && potValue >= 256){
     brillo = 0.5;
      }

    else if (potValue < 768 && potValue >= 512){
     brillo = 0.75;
      }

    else if (potValue < 1024 && potValue >= 768){
     brillo = 1;
      }

    //-----------------------------parte del http request, que no se altera-------------------------------------------
       WiFiClient client = server.available();   // Listen for incoming clients

      if (client) {                             // If a new client connects,
        currentTime = millis();
        previousTime = currentTime;
        Serial.println("New Client.");          // print a message out in the serial port
        String currentLine = "";                // make a String to hold incoming data from the client
        while (client.connected() && currentTime - previousTime <= timeoutTime) {  // loop while the client's connected
          currentTime = millis();
          if (client.available()) {             // if there's bytes to read from the client,
            char c = client.read();             // read a byte, then
            Serial.write(c);                    // print it out the serial monitor
            header += c;
            if (c == '\n') {                    // if the byte is a newline character
              // if the current line is blank, you got two newline characters in a row.
              // that's the end of the client HTTP request, so send a response:
              if (currentLine.length() == 0) {
                // HTTP headers always start with a response code (e.g. HTTP/1.1 200 OK)
                // and a content-type so the client knows what's coming, then a blank line:
                client.println("HTTP/1.1 200 OK");
                client.println("Content-type:text/html");
                client.println("Connection: close");
                client.println();



    //-------------------------------------------------Hacemos Display de La Página Web---------------------------------------------------------

                //Imprimimos la primera parte de la página (solo mostraremos el botón de Encendido al acceder)
                client.println(Head);

                //Al "encender" haremos 2 ciclos, el primero encenderemos de manera manual para coincidir con lo que se mostrará en la página en el segundo ciclo
                //esta condición se cumple una sola vez al encender la luz ya que "recordaremos" el estado previo de el ciclo anterior
                if(header.indexOf("GET /on") >= 0 && prevstat == "off"){

                  //Grabamos en la memoria la condición máxima de cada valor para encender todos por predeterminado al encender los leds
                  MemR = 1023;
                  MemG = 1023;
                  MemB = 1023;

                  prevstat = "on";          
                  }

                //Desde El segundo Ciclo en adelante esta será la condición que se cumpla hasta apagar el led, el cual regresará a el estado inicial de la página
                if(header.indexOf("GET /on") >= 0 && prevstat == "on"){
                  client.println(Off);

                  //Imprimimos los sliders en el siguiente orden: Rojo, Verde, Azul

                  //Slider Rojo
                  client.println("<p class=\"red\">Hue Rojo: </p><input type=\"range\" min=\"0\" max=\"1023\" value = \"1023\" class=\"slider\" id=\"red\" onchange=\"red(this.value)\"/>"); 
                  client.println("<script>var re = document.getElementById(\"red\"); re.innerHTML = slider.value;");
                  client.println("slider.oninput = function() {slider.value = this.value;red.innerHTML = this.value;}");
                  client.println("$.ajaxSetup({timeout:1000});function red(pos) {");
                  client.println("$.get(\"/on/?reeed=\" + pos + \"&\");{Connection: close};}</script>");

                  //Slider Verde
                  client.println("<p class=\"green\">Hue Verde: </p><input type=\"range\" min=\"0\" max=\"1023\" value = \"1023\" class=\"slider\" id=\"green\" onchange=\"green(this.value)\"/>"); 
                  client.println("<script>var gren = document.getElementById(\"green\"); gren.innerHTML = slider.value;");
                  client.println("slider.oninput = function() {slider.value = this.value;green.innerHTML = this.value;}");
                  client.println("$.ajaxSetup({timeout:1000});function green(pos) {");
                  client.println("$.get(\"/on/?green=\" + pos + \"&\");{Connection: close};}</script>");

                  //Slider Azul
                  client.println("<p class=\"blue\">Hue Azul: </p><input type=\"range\" min=\"0\" max=\"1023\" value = \"1023\" class=\"slider\" id=\"blue\" onchange=\"blue(this.value)\"/>"); 
                  client.println("<script>var blu = document.getElementById(\"blue\");blu.innerHTML = slider.value;");
                  client.println("slider.oninput = function() {slider.value = this.value;blue.innerHTML = this.value;}");
                  client.println("$.ajaxSetup({timeout:1000});function blue(pos) {");
                  client.println("$.get(\"/on/?blues=\" + pos + \"&\");{Connection: close};}</script>");

                  client.println("</body></html>");

                  //Para obtener el valor del último cambio realizado al Slider
                  if(header.indexOf("GET /on/?reeed=") >= 0){
                    Serial.println("Rojo Get");
                    pos1 = header.indexOf('=');
                    pos2 = header.indexOf('&');

                    valHue = header.substring(pos1 + 1, pos2);
                    MemR = valHue.toInt(); 
                  }

                  if(header.indexOf("GET /on/?blues=") >= 0){
                    Serial.println("Azul Get");
                    pos1 = header.indexOf('=');
                    pos2 = header.indexOf('&');

                    valHue = header.substring(pos1 + 1, pos2);
                    MemB = valHue.toInt();
                  }

                  if(header.indexOf("GET /on/?green=") >= 0){
                    Serial.println("Verde Get");
                    pos1 = header.indexOf('=');
                    pos2 = header.indexOf('&');

                    valHue = header.substring(pos1 + 1, pos2);
                    MemG = valHue.toInt();
                  }                
                }


                //Condición sucede si se presiona el boton de apagar o se va a cualquiera otra dirección fuera de 
                //la direccion http://192.168.0.69/on..., procede a apagar
                else{
                  client.println(On);
                  MemR = 0;
                  MemG = 0;
                  MemB = 0;
                  prevstat = "off";
                }

              client.println();
              break;
              }

              else{
                currentLine = "";
              }

            }

            else if(c != '\r'){
              currentLine += c;
              }
            }       
          }
        // Clear the header variable
        header = "";
        // Close the connection
        client.stop();
        Serial.println("Client disconnected.");
        Serial.println("");         
        }   

      //Los valores del ciclo son guardados en las variables de memoria MemR/MemG/MemB (colores R, G y B)
      //podemos encender la luz con la intensidad leída en este ciclo y colores del ciclo anterior
      ledcWrite(ledChannR, MemR * brillo);
      ledcWrite(ledChannG, MemG * brillo);
      ledcWrite(ledChannB, MemB * brillo);

    }
  
