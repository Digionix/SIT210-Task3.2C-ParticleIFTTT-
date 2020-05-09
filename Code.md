```
int led1 = A5; 
int led2 = D7; 
int i = 0;
int total_time = 0;


int photoresistor = A0;
int analogValue;


void myHandler(const char *event, const char *data)
{
    i++;
    Serial.print(i);
    Serial.print(event);
    Serial.print(", data: ");
    if (data)
    Serial.println(data);
    else
    Serial.println("NULL");
}


void setup() {


  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  pinMode(photoresistor, INPUT);    
  
  Particle.subscribe("Sun_Status", myHandler);
}


void loop() {

  analogValue = analogRead(photoresistor);      // Reads the data from the sensor
  
  delay(15000);     // takes 15 second to check if the sun is really hitting the terrarium or not
  
  if (analogValue < 50) 
  {
      digitalWrite(led1, LOW);
      digitalWrite(led2, LOW);
      Particle.publish("Sun_Status", "Not Hit");        // Publish data to line that the sun is not hitting the terrarium
  }
  else
  {
      Particle.publish("Sun_Status", "Hit" );       // Publish data to line that the sun is hitting the terrarium
      digitalWrite(led1, HIGH);
      digitalWrite(led2, HIGH);
      
      total_time += 1;      // Adds 1 minute to total time hit by the sun
  }
  
  if (total_time >= 120)
  {
      Particle.publish("Sun_Status", "Terrarium got 2 hour of sunlight");
  }
  
  delay(45000); // Wait for 45 second before publishing data again
  
  // In total every 1 minute there will be new data
}

```
