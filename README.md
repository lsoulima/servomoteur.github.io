# servomoteur code arduino C



                                                                                        
  La bibliothèque `<Servo.h>` permet à une carte Arduino de contrôler des servomoteurs. Les servomoteurs intègrent des engrenages et un arbre qui peut être contrôlée avec précision. Les modèles standards permettent l'arbre d’être positionné à divers angles, 
  généralement entre 0 et 180 degrés (cas du SG90). L'utilisation de la bibliothèque désactive la fonctionnalité analogWrite() (PWM) sur les broches 9 et 10 même si aucun servomoteur n’est branché.                                                       
                                                                                         
  > La fonction **attach()**:                                                               
  Cette fonction sert à associer un objet "servomoteur" à une broche de l’Arduino.       
  Syntaxe :   
   `servo.attach(broche); `                                                 
                                                                                         
  > La focntion **write()**:                                                                
  Commande l’arbre du servomoteur à tourner vers l’angle donné en paramètre (0°-180°).   
  Syntaxe :   
  ` servo.write(angle);`                                                       
                                                                                         
  > La focntion **read()**:                                                                 
  La fonction servo.read() retourne la valeur de l’angle de l’arbre du moteur (0°-180°). 
                                                                                         
  > La focntion **detach()**:                                                               
  servo.detach() désassocie un servo d’une broche.                                       
                                                                                        

```c
#include <Servo.h>

//En-tête déclarative
Servo myservo1, myservo2;
int LDR1 = A0, LDR2 = A1, LDR3 = A2, LDR4 = A3;
int rRDL1 = 0, rRDL2 = 0, rRDL3 = 0, rRDL4 = 0;
int max1 = 0, max2 = 0, max3 = 0;
int ser1 = 80, ser2 = 0;

//fonction d'initialisation de la carte (configuration initiale)
void setup
{
    myservo1.attach(9);
    myservo2.attach(8);

    /**--------------------------------------------------------------------------**
    * Initialiser le port série de la carte Arduino. La vitesse de transmission
    * des données est donnée en paramètre à la fonction serial.begin().
    **---------------------------------------------------------------------------*/

    Serial.begin(9600);
    myservo1.write(ser1);
    myservo2.write(100);
}

//fonction principale qui se répète (s’exécute) à l'infini.
void loop()
{
    /**-------------------------------------------------**
    * La fonction analogRead()
    * Lit la valeur de la broche analogique spécifiée.
    **---------------------------------------------------*/

    rRDL1 = analogRead(LDR1) / 100;
    rRDL2 = analogRead(LDR2) / 100;
    rRDL3 = analogRead(LDR3) / 100;
    rRDL4 = analogRead(LDR4) / 100;

    max1 = max(rRDL1, rRDL2);
    max2 = max(rRDL3, rRDL4);
    max3 = max(max1, max2);

    if (rRDL1 < max3 && rRDL2 < max3)
    {
        if (ser1 < 140)
            ser1 += 1;
        myservo1.write(ser1);
    }
    if (rRDL3 < max3 && rRDL4 < max3)
    {
        if (ser1 > 0)
            ser1 -= 1;
        myservo1.write(ser1);
    }
    if (rRDL2 < max3 && rRDL3 < max3)
    {   
        /**---------------------------------------------------------------------------------------------**
        * La fonction println()
        * Transmet les données au port série sous forme de texte lisible ASCII suivie d'un
        * caractère de retour chariot (ASCII 13, ou '\ r') et un caractère de nouvelle ligne (ASCII
        * 10, ou '\ n').
        **----------------------------------------------------------------------------------------------*/

        Serial.println("servo2 +" + String(ser2));
        if (ser2 < 180)
            ser2 += 1;
        myservo2.write(ser2);
    }
    if (rRDL1 < max3 88 rRDL4 < max3)
    {
        Serial.println("servo2 -" + Sting(ser2));
        if (ser2 > 0)
            ser2 -= 1;
        myservo2.write(ser2);
    }
}
```

# Exemples

> Ex 1:

```c
/*
  Parts required:
  - servo motor
  - 10 kilohm potentiometer
  - two 100 uF electrolytic capacitors
*/

// include the Servo library
#include <Servo.h>

Servo myServo;  // create a servo object

int const potPin = A0; // analog pin used to connect the potentiometer
int potVal;  // variable to read the value from the analog pin
int angle;   // variable to hold the angle for the servo motor

void setup() {
  myServo.attach(9); // attaches the servo on pin 9 to the servo object
  Serial.begin(9600); // open a serial connection to your computer
}

void loop() {
  potVal = analogRead(potPin); // read the value of the potentiometer
  // print out the value to the Serial Monitor
  Serial.print("potVal: ");
  Serial.print(potVal);

  // scale the numbers from the pot
  angle = map(potVal, 0, 1023, 0, 179);

  // print out the angle for the servo motor
  Serial.print(", angle: ");
  Serial.println(angle);

  // set the servo position
  myServo.write(angle);

  // wait for the servo to get there
  delay(15);
}

```

> Ex 2:

```c

#include <SoftwareServo.h> 
#include <Servo.h> 
 
SoftwareServo myservo2;  // create servo object to control a servo 
Servo myservo;

int pos = 0;    // variable to store the servo position 
 
void setup() 
{ 
  myservo.attach(9);  // attaches the servo on pin 9 to the servo object
  myservo2.attach(10);  // attaches the servo on pin 10 to the softservo object 
} 
 
 
void loop() 
{ 
  for(pos = 0; pos < 180; pos += 1)  // goes from 0 degrees to 180 degrees 
  {                                  // in steps of 1 degree 
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    myservo2.write(pos); 
    delay(15);                       // waits 15ms for the servo to reach the position 
  } 
  for(pos = 180; pos>=1; pos-=1)     // goes from 180 degrees to 0 degrees 
  {                                
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    myservo2.write(pos); 
    delay(15);                       // waits 15ms for the servo to reach the position 
  } 
} 
```

> Ex 3:

```c

```
