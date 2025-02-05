#include <WiFi.h>
#include <HTTPClient.h>

// Wi-Fi credentials
const char* ssid = "Mathurs_4G";        // Your Wi-Fi network SSID
const char* password = "Vijay@302020"; // Your Wi-Fi network password

// API endpoint URL
const char* apiUrl = "https://httpbin.org/post"; // For testing POST requests

// LED Pins
const int greenLED = 2; // Green LED pin (Wi-Fi connected)
const int redLED = 4;   // Red LED pin (Wi-Fi connection issue)
const int blueLED = 5;  // Blue LED pin (Data sent successfully)

void setup() {
  // Initialize Serial Monitor
  Serial.begin(115200);
  
  // Initialize LED pins
  pinMode(greenLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(blueLED, OUTPUT);

  // Turn off all LEDs initially
  digitalWrite(greenLED, LOW);
  digitalWrite(redLED, LOW);
  digitalWrite(blueLED, LOW);

  // Connect to Wi-Fi
  // WiFi.begin(ssid, password);
  WiFi.begin("Wokwi-GUEST", "", 6);
  Serial.println("Connecting to Wi-Fi...");
  
  while (WiFi.status() != WL_CONNECTED) {
    digitalWrite(redLED, HIGH); // Turn on red LED (Wi-Fi issue)
    delay(1000);
    Serial.println("Still trying to connect...");
  }

  // Wi-Fi connected
  digitalWrite(redLED, LOW);   // Turn off red LED
  digitalWrite(greenLED, HIGH); // Turn on green LED
  Serial.println("Wi-Fi Connected!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());
}

void loop() {
  if (Serial.available() > 0) {
    // Turn off the blue LED before new data transmission
    digitalWrite(blueLED, LOW);

    String barcode = "";
    while (Serial.available()) {
      barcode += (char)Serial.read();  // Read barcode data from Serial Monitor
    }

    // Print the scanned barcode
    Serial.println("Scanned Barcode: " + barcode);

    // Send the barcode to the API
    sendBarcodeToAPI(barcode);
  }

  delay(1000);
}

void sendBarcodeToAPI(String barcode) {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    http.begin(apiUrl); // Start the HTTP request
    http.addHeader("Content-Type", "application/x-www-form-urlencoded");

    // Prepare the payload
    String payload = "barcode=" + barcode;

    // Send POST request
    int httpResponseCode = http.POST(payload);

    // Handle the response
    if (httpResponseCode > 0) {
      Serial.println("HTTP Response Code: " + String(httpResponseCode));
      String response = http.getString();
      Serial.println("Response: " + response);

      // Turn on blue LED for successful data transmission
      digitalWrite(blueLED, HIGH);
    } else {
      Serial.println("Error in HTTP request");
    }

    http.end(); // Close the HTTP connection
  } else {
    Serial.println("Wi-Fi not connected");
    digitalWrite(greenLED, LOW); // Turn off green LED
    digitalWrite(redLED, HIGH); // Turn on red LED (Wi-Fi issue)
  }
}
