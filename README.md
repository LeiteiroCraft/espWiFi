#include <DNSServer.h>
#include <ESP8266WiFi.h>
#include <WiFiUdp.h>
#include <PubSubClient.h>
#include <ESP8266mDNS.h>
#include <ESP8266WebServer.h>
ESP8266WebServer server(80);
  const int led = 2;
  IPAddress ip;
  IPAddress submask;
  IPAddress getway;
  IPAddress broadcast;
  DNSServer dnss;
  IPAddress newIP;

  void setup() 
  {
    pinMode(led, OUTPUT);
    Serial.begin(115200);
    Serial.println();
    WiFi.begin("Antunes_Claro", "mai1595@");
    
    Serial.println("conecting to the WiFi.....");
   
    while(WiFi.status() != WL_CONNECTED)
    {
      Serial.println("Server HTTP iniciado");
    }
      Serial.print(".");
      Serial.println("Connected in the WiFi.");
      Serial.println(". . . . . . .");
      Serial.println("Connected to IP: ");
      ip = WiFi.localIP();
      Serial.println(ip);

      Serial.println(". . . .");

      Serial.println("The mask of this IP is: ");
      submask = WiFi.subnetMask();
      Serial.println(submask);

      Serial.println(". . .");

      Serial.println("The default gateway IP is :");
      getway = WiFi.gatewayIP();
      Serial.println(getway);
      Serial.println(". . . . . .");

      Serial.println("The Total of the broadcast is: ");
      broadcast = WiFi.broadcastIP();
      Serial.println(broadcast);
    
      Serial.println(". . .");
      
      server.on("/", HTTP_GET, handleRoot);
      server.begin();
      Serial.println("Teste a conexao");
    
      if(WiFi.status() == WL_CONNECTED)
      {
        ip = WiFi.localIP();
        Serial.print("IP: ");
        Serial.println(ip);

        server.on("/", HTTP_GET, handleRoot);
        server.begin();
        Serial.println("Server Inicialized");
      }
      
    
    WiFi.mode(WIFI_STA);

    WiFi.softAP("Antunes_Claro", "mai1595@");
    dnss.start(53, "*", ip);

  }
void loop() 
{
  server.handleClient();
  handleRoot();

  digitalWrite(led, HIGH);
  delay(66);
  digitalWrite(led, LOW);
  delay(66);

}
void handleRoot()
{
  server.send(200, "text/html", 
  "<html>" "<head>"
  "<style>"
  "body{"
   "<body><h1>hello to ESP8266! Test</h1><h2>Web Server project test</h2></body></html>"); 
  "  margin: 0;"
"  padding: 0;"
"  height: 200vh;"
"  display: flex;"
"  justify-content: center;"
"  align-items: center;"
"  background: linear-gradient(120deg, #053f1f, #196d4f, #06ff8b, #93ff06, #bace06);"
"  text-align: center;"
"}";
  
}


 


