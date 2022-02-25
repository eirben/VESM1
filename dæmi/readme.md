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
