// 회로

![IMG_2884](https://user-images.githubusercontent.com/119734977/205439220-f00385f7-2d63-480b-8bf9-1fd26e81253a.jpeg)

// 아두이노 코드

void setup(){
  Serial.begin(9600);
  pinMode(13, OUTPUT);
}
double th(int v) {
  double
  t = log(((10240000/v) - 10000));
  t = 1 /(0.001129148 + (0.000234125*t) + (0.0000000876741*t*t*t));
  t = t - 273.15;
  return t;
}
void loop(){
  int a=analogRead(A0);
  Serial.println(th(a));
  int dly = (Serial.readString()).toInt();
  digitalWrite(13, HIGH);
  delay(dly);
  digitalWrite(13, LOW);
  delay(dly);
}

// 프로세싱 코드

import processing.serial.*;
import processing.net.*;

Serial p;
Server s;
Client c;
void setup() {
  s = new Server(this, 1234);
  p = new Serial(this, "COM6", 9600);
}
String msg;
void draw() {
  c = s.available();
  if (c!=null) {
    String m = c.readString();
    if (m.indexOf("PUT")==0) {
      int n = m.indexOf("\r\n\r\n")+4;
      m = m.substring(n);
      m += '\n';
      print(m);
      p.write(m);
    }
    else if (m.indexOf("GET")==0) {
      c.write("HTTP/1.1 200 OK\r\n");
      c.write("Content-length: " + msg.length() + " \r\n\r\n");
      c.write(msg);
    }
  }
  if (p.available()>0) {
    String m = p.readStringUntil('\n');
    if (m!=null) {
      msg = m;
      print(m);
    }
  }
}

// 앱인벤터
![image](https://user-images.githubusercontent.com/119734977/205438477-7c4d9adc-51b4-43eb-bf03-d7163edacd68.png)
![image](https://user-images.githubusercontent.com/119734977/205438499-297d6bdb-c5e7-4493-99cc-d578f60f60a5.png)
