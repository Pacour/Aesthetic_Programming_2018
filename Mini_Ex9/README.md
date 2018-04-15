# Mini_Ex9:

**Objective:**
Acquiring the skill of decomposing a computer program into definable parts, and organize this decomposition using a flowchart. further more to understand a flowchart as a means for communication and planning, and being able to comprehend the concept of algorithms from the perspectives of both computer science and cultural understanding. 

### Obfuscated_FB_Non-human.js as a flowchart
**De-composition of my Mini_Ex4:**

![alt text](https://github.com/L4COUR/Aesthetic_Programming_Mini_Ex-s/blob/master/Mini_Ex9/Mini_ex09%20(3).png "mini_ex9_flowchart")
</br>
[Obfuscated_FB_Non-human.js](https://github.com/L4COUR/Aesthetic_Programming_Mini_Ex-s/tree/master/Mini_Ex4).

This Mini_Ex is very different in comparison to the previous ones I have made, in the sense that it focuses more upon the understanding and de-composition of pre-existing code. I have chosen to decompose my Mini_Ex4 which is probably the most complicated p5.js program I have coded so far, and I thought that this would be a nice oppertunity to make it more clear what the rather complex code does exactly. I have drawed the flowchart above in a way which sort of creates 4 segments which are all interconnected with each other. So the first segment is where the program is initiated and the global varibles are being defined. the webcam is also being initiated here so that the video-feed can then later be tampered with. the second segment consists of the serial data from an arduino circuit with a light-dependent resistor which is channeled into the p5.js program via a ```p5.SerialPort()```, in order to be used as the variable ```inData```. this segment is mostly made up of a lot of console.log messages which is good to have when parsing this kind of data through, because it otherwise would be almost impossible to locate a potential error in this parsing. The third segment is where the initiation of the manipulated webcam feed is coded, along with a p5.dom button which is styled so it looks as close to a facebook "log in" button as possible. The important part here is the part which is contained in the code under the ```start()``` function. So if you press the button it will then trigger this function which will change the varible started from being false to being true. When ```started = true;```then two processes will be initiated, the first when will create a paragraph element containing the sentence "Human to non-human data transfer complete." The next process that is initiated is contained in the fourth and last segment of the flowchart. This part of the code is also placed under the ```draw()```function. This is the segment in which the webcam feed is manipulated into a mesh of pixels made up by rectangular shapes that are being distorted from the serial data. The complex part of this last segment is the nested for-loop which uses the height and width of the captured webcam feed in order to first create the colums of squares on the Y-axis and then from each of those colums add rows of rectangles along the x-axis, thus resulting in a mesh of rectangles covering the whole screen acting as distorted pixels repressenting the webcam overlaying the facebook log in page, as is illustrated in the .gif below.

**Header:**
(Text)

**The Code:**
![alt text](https://github.com/L4COUR/Aesthetic_Programming_Mini_Ex-s/blob/master/Mini_Ex4/La-Cour---Obfuscated_FB_Non-human.gif "mini_ex9_flowchart")

```javascript
/* This mini_ex4 evolves around data capturing in any shape or form
in this example I have chosen to focus on capturing serial data
from a light-dependend-resistor, which gives values to ambient light in a room.

connecting an arduino uno to p5.js through serial is not an easy task,
especially when you dont know anything about serial communication.
*/

//Hardware -> Software communication
var started = false;
var serial //variable to hold an instance of the serialport lib.
var portName = '/dev/cu.usbmodem1421';
var inData;

//button
var button;

//web-cam
var capture;

function setup() {
  var canvas;
  canvas = createCanvas(windowWidth, windowHeight);
  canvas.position(0,0);
//web-cam capture
  capture = createCapture(VIDEO);
  capture.hide();
  rectMode(CENTER);
  noStroke();

  frameRate(10);

//serial data from arduino, interface
serial = new p5.SerialPort(); //make a new instance of the serialport lib.
  serial.on('list', println);  // set a callback function for the serialport list event
  serial.on('connected', serverConnected); // callback for connecting to the server
  serial.on('open', portOpen);        // The port opening
  serial.on('data', serialEvent);     // New data arrives
  serial.on('error', serialError);    // Errors
  serial.on('close', portClose);      // The port closing

serial.open(portName);              // open a serial port
/*draw and serial event function can now be combined
in order to make the code manifest itself as a material
in the "real world".
*/

var button = createButton("Log In");
button.mousePressed(start);
button.style("display","inline-block");
button.style("color","#fff");
button.style("padding","5px 8px");
button.style("text-decoration","none");
button.style("font-size","0.9em");
button.style("font-weight","normal");
button.style("border-radius","3px");
button.style("border","none");
button.style("text-shadow","0 -1px 0 rgba(0,0,0,.2)");
button.style("background","#4c69ba");
button.style("background","-moz-linear-gradient(top, #4c69ba 0%, #3b55a0 100%)");
button.style("background","-webkit-gradient(linear, left top, left bottom, color-stop(0%, #3b55a0))");
button.style("background","-webkit-linear-gradient(top, #4c69ba 0%, #3b55a0 100%)");
button.style("background","-o-linear-gradient(top, #4c69ba 0%, #3b55a0 100%)");
button.style("background","-ms-linear-gradient(top, #4c69ba 0%, #3b55a0 100%)");
button.style("background","linear-gradient(to bottom, #4c69ba 0%, #3b55a0 100%)");
button.style("filter","progid:DXImageTransform.Microsoft.gradient( startColorstr='#4c69ba', endColorstr='#3b55a0', GradientType=0 )");
button.position(width/1.19,35);

noLoop();
}


function draw() {
  if(started){


    clear(0);
      fill(0);
      capture.loadPixels();
      for (var cy = 0; cy < capture.height; cy += 3) {
        for (var cx = 0; cx < capture.width; cx += 3) {
          var offset = ((cy*capture.width)+cx)*4+inData;
          var xpos = (cx / capture.width) * width;
          var ypos = (cy / capture.height) * height;
          rect(xpos, ypos, 10+(inData-100),
            (20) * (capture.pixels[offset+1]/200));
          }
        }
  //image(capture, 0, 0, 320, 240);
  //filter(INVERT);

  }
}

function clearence() {
  clear();
}

var p;

function start(){
   started = true;
   loop();
   p = createElement('p', 'Human to Non-human data transfer complete.');
   p.position(0,0);
   p.style("color","#fff");
}

//serial data - in console.log
  function serialEvent() {
    inData = Number(serial.read());
    console.log(inData);
  }

  function println(portln){ //shows the data from the arduino in the console
    for (var i = 0; i < portList.length; i++){
      console.log(i + " " + portList[i]);
    }
  }

// All the functions below are callback functions, for utilitarian reasons
  function serverConnected() {
    println('connected to server.');
  }

  function portOpen() {
    println('the serial port opened.')
  }

  function serialError(err) {
    println('Something went wrong with the serial port. ' + err);
  }

  function portClose() {
    println('The serial port closed.');
  }

```
**Sources:**
- https://github.com/Mmarksp/Aesthetic_Programming_2018/tree/master/mini_exercises/mini_ex8
- https://developers.giphy.com/explorer/