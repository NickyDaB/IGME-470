# Fun With Unity

## Table of contents
1. [Goal](#Goal)
2. [Preview](#Preview)
3. [Instructions](#Instructions)
4. [Conclusion](#Conclusion)
5. [Submitting](#Submitting)
6. [Bonus Levels: Where can we go from here?](#bonus-levels-where-can-we-go-from-here)

## Goal

Get some hands-on experience with connecting our Arduino to Unity. We will send data to Unity from the Arduino and vice versa. The demos here are simple, but they are great starting points and proof of functionality.

## Preview

This is a preview of what we will have by the end of the exercise.

[![Thumbnail Image of youtube preview link](video_preview.png)](https://youtu.be/i41ObD9AJeo?list=PLUzUMAB7oUTeWzJGtpiETo4tBbwV8X-_L)

[YouTube Video: Arduino Unity Serial Demo](https://youtu.be/i41ObD9AJeo?list=PLUzUMAB7oUTeWzJGtpiETo4tBbwV8X-_L)

## Instructions

The first thing we need to do is to basically re-create the circuit from HW: 14 - Tweak the Logo.  

![screenshot](1.png)

The only major change that we will add to this is also placing an LED onto the board. You can see my updated circuit below.

![screenshot](2.png)

Next, we need to open up the Arduino IDE and code up the Arduino side of things. Again, this should remind you of HW 14.

Way at the top of our program, we should add in some variables that we will use later. Let's make an `int` called "readData" and a `long` called "mappedData".

You can see my code below:

```
int readData = -1;
long mappedData = 0;
```

After that, we need to put a few lines of code in our `setup()` method. Because we are working with serial connections, we need to start up our serial stuff. Do you remember how to do that? We call `Serial.begin()`. Don't forget to put in the appropriate baud rate. 

We won't need this until the second half when we receive data back from Unity, but I'll also set up my LED now. I plugged mine into digital pin 7. So I set that as an output. Then, I'll start it in the "on" position.

You can see my code below:

```
  // initialize serial communication
  Serial.begin(9600);

  pinMode(7, OUTPUT);

  digitalWrite(7, HIGH);
```

That is all we will need for `setup()`. Now to move on to `loop()`.

Before we send any data over the serial connection, we need to have some data to send! We will be using the potentiometer we connected to pin A0 to control and move a cube in our Unity scene. So let's grab the rawData from the potentiometer.

```
readData = analogRead(A0);
```

Next, for ease of use, I think we should map that data. I'll map it from -100 to 100. I picked these numbers because I think they will help for what I'm doing. 

Some things that went into my thought process were:
- 0 to 1 seems like a good "range" for analog. that is usually my default. but here I want to move a cube left and right. knowing what I'm doing later, I think I'll use this as a modifier of some sort to position, so having it be -1 to 1 might be easier for me for later.
- then I also multiplied it by 100. I wanted that decimal precision later on, and the map function doesn't play nice with floats because of its integer math. so, if I multiply it by 100 now. I can always divide it by 100 later to get that precision on my 0-1 esq scale. 
- again, I have the luxury of writing both ends of the code here, so I can kinda do what I want. But if you were a tools programmer, you might want to make this a little more standardized. 

I store that new mapped data in our variable from earlier.

```
mappedData = map(readData, 0, 1023, -100, 100);
```

Now, you definitely should check out the [Arduino reference for serial communication](https://www.arduino.cc/reference/en/language/functions/communication/serial/). But I kinda know I'll be taking advantage of Unity serial stream and read it in as a string. So I've decided to go with the `print` and `println` functions in the next part. You should also be asking yourself, "How should we structure our data?" Again, I'm taking advantage of knowing how I'll interpret the data on the other end, so I kinda made my own "data buffer" with my own "struct". I make a "CSV" on a single line. 

You can see my code below:

```
  Serial.print("Hi");
  Serial.print(",");
  Serial.print(readData);
  Serial.print(",");
  Serial.print(mappedData);
  Serial.println();
```

I have the first value there as a test. "Hi" won't really help us do anything today. Honestly, the only value we really care about will be `mappedData` but I wanted to showcase sending multiple pieces of data over to Unity. I end my "struct" with a blank `Serial.println()` to append that new line character to make it easier when we interpret the string on the Unity side of things. 

At this point in time, this is what my Arduino code looks like: 

```
int readData = -1;
long mappedData = 0;

void setup() {
  // initialize serial communication
  Serial.begin(9600);

  pinMode(7, OUTPUT);

  digitalWrite(7, HIGH);
}

void loop() {
  // read the value of A0 (the twisty knob)
  readData = analogRead(A0);
  
  //map it
  mappedData = map(readData, 0, 1023, -100, 100);

  //Unity Read as a string?
  //String for Unity

  Serial.print("Hi");
  Serial.print(",");
  Serial.print(readData);
  Serial.print(",");
  Serial.print(mappedData);
  Serial.println();
}
```

Ok. We are good on the Arduino side for now. We will come back to it later, but let's jump into Unity for now.



## Conclusion

## Submitting

## Bonus Levels: Where can we go from here?