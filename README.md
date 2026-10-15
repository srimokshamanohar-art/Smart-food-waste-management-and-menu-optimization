
#include "HX711.h"

#define DT 4
#define SCK 5

HX711 scale;

// Temporary calibration factor.
// Calibrate this for your actual load cell.
float calibration_factor = 420.0;

void setup() {
  Serial.begin(115200);
  Serial.println("Smart Food Waste Measurement");

  scale.begin(DT, SCK);

  if (!scale.is_ready()) {
    Serial.println("HX711 not detected!");
    while (1) {
      delay(100);
    }
  }

  scale.set_scale(calibration_factor);
  scale.tare();

  Serial.println("HX711 ready");
  Serial.println("Place food waste on the load cell.");
}

void loop() {
  if (scale.is_ready()) {
    float weight = scale.get_units(10);

    if (weight < 0 && weight > -5) {
      weight = 0;
    }

    Serial.print("Food waste weight: ");
    Serial.print(weight, 2);
    Serial.println(" g");
  } else {
    Serial.println("HX711 reading error");
  }

  delay(1000);
}
