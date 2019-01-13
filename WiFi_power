
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

uint8_t PowerPin = 13;
bool    PowerOn  = false;

const char* ssid = "";
const char* password = "";

ESP8266WebServer server(80);

void setup() {
  Serial.begin(115200);
  Serial.printf("\n\nFree memory %d\n", ESP.getFreeHeap());
  pinMode(PowerPin , OUTPUT);
  pinMode(15, OUTPUT);
  pinMode(12, OUTPUT);
  WiFi.mode(WIFI_AP_STA);
  WiFi.begin(ssid, password);
  int status = 0;
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    if (status) {
      status = 0;
      digitalWrite(12 , 1);
    } else {
      status++;
      digitalWrite(12 , 0);
    }
    Serial.print(".");
  }
  digitalWrite(12 , 0);
  if (WiFi.waitForConnectResult() == WL_CONNECTED) {
    Serial.println("\n\nHello Vova\n");
    server.on ( "/", HTTP_handleRoot );
    server.on ( "/control", HTTP_handle );
    server.onNotFound ( HTTP_handleRoot );
    server.begin();
    Serial.println("\n\nESP ip address: %l\n", WiFi.localIP());
  } else {
    Serial.printf("WiFi started is fail");
  }
}

void loop() {
  server.handleClient();
  delay(50);
}

void HTTP_handle(void) {
  String message = "";
   for (int i = 0; i < server.args(); i++) {
    message += server.argName(i) + ": ";
    message += server.arg(i) + "\n";
    digitalWrite(atoi(server.argName(i).c_str()), atoi(server.arg(i).c_str()));
   }
  server.send (200, "text/html", message);
}

void HTTP_handleRoot(void) {
  String out;
  out += "\
  <!DOCTYPE html>\
<html>\
    <head>\
        <meta charset=\"utf-8\">\
        <style>\
          .title_b {margin-top:10px}\
        </style>\
        <title>House control...</title>\
        <meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">\
        <link rel=\"stylesheet\" href=\"http://cdn.designinstruct.com/files/480-ios-7-switch-control-tutorial/demo/standalone/switchery.css\" />\
        <link rel=\"stylesheet\" href=\"http://cdn.designinstruct.com/files/480-ios-7-switch-control-tutorial/demo/custom.css\">\
        <link href='http://fonts.googleapis.com/css?family=PT+Sans+Narrow:400,700' rel='stylesheet' type='text/css'>\
        <script src=\"http://cdn.designinstruct.com/files/480-ios-7-switch-control-tutorial/demo/standalone/switchery.js\"></script>\
    </head>\
    <body>\
        <div class=\"container\"> \
            <h1 class=\"title_b\">Дом</h1>\
            <p class=\"sub-title\">Управление приборами:</p>\
            <form class=\"switchery-form\">\
                <p class=\"settings-heading\">Уключатель:</p>\
                <p class=\"setting-switch setting-switch-first\">\
                    <label for=\"setting1\">Освещение комната (большое): </label>\
                    <input type=\"checkbox\" class=\"switchery\" name=\"setting1\" />\
                </p>\
                <p class=\"setting-switch setting-switch-first\">\
                    <label for=\"setting2\">Освещение комната (1): </label>\
                    <input type=\"checkbox\" class=\"switchery\" name=\"setting2\" />\
                </p>\
                <p class=\"setting-switch setting-switch-first\">\
                    <label for=\"setting3\">Освещение комната (2): </label>\
                    <input type=\"checkbox\" class=\"switchery\" name=\"setting3\" />\
                </p>\
                <p class=\"setting-switch setting-switch-first\">\
                    <label for=\"setting4\">Коридор (комод): </label>\
                    <input type=\"checkbox\" class=\"switchery\" name=\"setting4\" />\
                </p>\
                <p class=\"setting-switch setting-switch-first\">\
                    <label for=\"setting5\">Коридор (основное): </label>\
                    <input type=\"checkbox\" class=\"switchery\" name=\"setting5\" />\
                </p>\
            </form>\
        </div>\
        <script type=\"text/javascript\">\
            var Switchery = require('switchery');\
            var elems = Array.prototype.slice.call(document.querySelectorAll('.switchery'));\
            elems.forEach(function(html) {\
                var switchery = new Switchery(html);\
            });\
        </script>\
    </body>\
</html>\
\n";
  server.send ( 200, "text/html", out );
}
