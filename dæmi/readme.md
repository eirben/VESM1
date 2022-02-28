# Kóði til að stjórna 9 -12V dc mótorum
Þessi kóði stjórnar hraða DC mótors frá gildi 100 til 255 þar sem 100 engin hraði bara hátíðni hljóð
upp í fullan hraða 255 en það er hægt að stjórna hraða og hve hratt mótor kemst í þennan hraða.
Þetta er síðan endurtekið þar sem þessi kóði er í loop.
**ATH! ekki hægt að láta mótor snúa í báðar áttir til þess þarf H-Brigde**
### Efni
1. Arduino uno
2. 1 stk 9v rafhlaða
3. 1 stk TIP 120 transistor
4. 5 stk vírar
5. 1 stk brauðbretti
6. 1 stk öflugur DC mótor
``` C
//aðeins notaðir PWM pinnar til að stjórna DC mótorum þ.e pinnar 11,10,9,6,5 0g 3
#define pwm 3

void setup() {
  pinMode(pwm,OUTPUT);
  Serial.begin(9600);
  //núllstilli mótor
  analogWrite(pwm,0);

}
int varyble_power=0;
void loop() {
  analogWrite(pwm, varyble_power);// þetta er pwm digital pinni og ég get því sett analogWrite() til að stjórna hraða.
  delay(1000);// þetta er tími sem mótor er í hverjum hraða, hér full hægt :-)
  varyble_power++; //eyk hraðan um 1 í hvert skipti
  if(varyble_power==255){
    varyble_power=0;//ser hraða á 0 þegar fullum hraða er náð
  }
}
```
## Mynd af tengingu
![Mynd](https://github.com/eirben/VESM1/blob/master/d%C3%A6mi/9V_DC_motor_tip120.png)

# Kóði til að stjórna 9 -12V dc mótorum fram og til baka með H-Brigde
### Efni
1. Arduino uno
2. 1 stk 9v rafhlaða
3. 1 stk L293D H-Brigde
4. 5 stk vírar
5. 1 stk brauðbretti
6. 1 stk öflugur DC mótor

pinni 9 PWM fer í Enable1&2, 8 í Input 1 og 7 í Input 2, pinni 8 og 7 stjórna í hvora áttina mótor snýst ef 8 er HIGH og 7 LOW til hægri
ef 8 LOW og 7 HIGH til vinstri. Plús á mótor fer í Output2 og mínus í Output1 eða öfugt :-). Hægt að tengja tvo DC mótora með einu H-Brigde

``` C
// Motor A
int enA = 9;
int in1 = 8;
int in2 = 7;
 

void setup()
{ 
  pinMode(enA, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
}

void demoOne()
{
  // Turn on motor A
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
 
  // Set speed to 200 out of possible range 0~255 
  analogWrite(enA, 200); 
  delay(2000);
  
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);    
  delay(2000);

  // Now change motor directions
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);  
  analogWrite(enA, 200);
  delay(2000);
 
  // Now turn off motors
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);    
  delay(2000);
}
 
void demoTwo()
{
  // Turn on motors
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);  
 
  // Accelerate from zero to maximum speed
 
  for (int i = 0; i < 256; i++)
  {
    analogWrite(enA, i);
    delay(20);
  } 
 
  // Decelerate from maximum speed to zero
 
  for (int i = 255; i >= 0; --i)
  {
    analogWrite(enA, i);
    delay(20);
  } 
 
  // Now turn off motors
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);  
  delay(2000);
  // Now change direction
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);  
 
  // Accelerate from zero to maximum speed
  for (int i = 0; i < 256; i++)
  {
    analogWrite(enA, i);
    delay(20);
  } 
 
  // Decelerate from maximum speed to zero
 
  for (int i = 255; i >= 0; --i)
 
  {
    analogWrite(enA, i);
    delay(20);
  }
  
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  delay(2000);
}
 
void loop()
{
  demoOne();
  delay(1000);
 
  demoTwo();
  delay(1000);
 
}
```
## Mynd af tengingu
![Mynd](https://github.com/VESM1VS/Kennarasvaedi/blob/master/Mekatronik/Motorar/Tengingar%20og%20k%C3%B3%C3%B0i/9V_DC_motor_Hbrigde-L293D.png)
