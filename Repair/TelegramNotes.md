# Telegram Notes

> Hypfer:
the dock wireless module can be found in the front attached to the display. That should iirc contain the pairing data so in that regard you should be fine

> Hypfer:
The pairing has something to do with magic button Presses and wiring up the debug connectors of dock and robot together so that they can talk via UART iirc

> Hypfer:
okay so iirc you should be able to press and hold the left button of the dock, then quickly press the right one 5x and then let go of it
it should beep differently

> Hypfer:
that should write something onto the uart of the dock which needs to be connected to the MCU uart of the w10

> Hypfer:
the pinout of the dock connector was similar iirc so the uart should be in a similar place
I had a hard time back then because I confused myself and didn't use a proper gnd connection but some other pin so make sure to look out for that

> Hypfer:
iirc you don't even have to connect the dock and the robot directly. You can first intercept the uart message of the dock, then connect your PC to the robots MCU uart and then replay that there

> Hypfer
I remember also having a lot of trouble with finding the right thing to do with the buttons

> picakia:
I connected the robot to the UART and tried sending what dock displayed (<SA_><Ccc>), but nothing happened. Does the robot need to be factory reset or put into some special mode?

> Hypfer:
Nope. Just awake iirc

> picakia:
Do you remember what was the reaction after successful pair? The screen on the dock showed something different?

> Hypfer:
yes they started being paired

> Hypfer:
<SA_><Ccc> is definitely wrong

> Hypfer:
you need a different button pattern

> Hypfer:
Oh hey I found something
Though I genuinely have no idea if it is actually correct or makes even sense at all

> Hypfer:
```
const { SerialPort, InterByteTimeoutParser } = require('serialport')

const serialport = new SerialPort({
    path: '/dev/ttyUSB0',
    baudRate: 115200,

    dataBits: 8,
    stopBits: 1,
    parity: 'none',


    xon: false,
    xoff: false,
    rtscts: false
})

const parser = serialport.pipe(new InterByteTimeoutParser({ interval: 300 }))
parser.on('data', data => {
    console.log(`Received message with length ${data.length}`, `Message: "${escape(data)}"`);

    if(data.includes("Software start--")) {
        setTimeout(() => {
            console.log("Sending <SA_>");
            console.log(Buffer.from("<SA_>\r"))
            serialport.write(Buffer.from("<SA_>\r"));

        }, 3000);

    } else if(escape(data) === "%3CZA%01%3E") {
        console.log("Sending next");
        serialport.write(Buffer.from([0x3C, 0x43, 0x54, 0x01, 0x3E, 0x0d]));

        setTimeout(() => {
            console.log("Sending read mac");
            serialport.write(Buffer.from("<Cc_>\r"));
        }, 500)
    }

    //console.log(data);
})
```

> Hypfer:
that probably did something
not sure from what to where though

> Hypfer:
```
Received message with length 5 Message: "%3CZA%01%3E"
Received message with length 5 Message: "%3CRT%01%3E"
Received message with length 22 Message: "%3CRc24%3A18%3AC6%3AE2%3A08%3A74%00%3E"
```

> Hypfer:
```
> unescape("%3CRc24%3A18%3AC6%3AE2%3A08%3A74%00%3E")
'<Rc24:18:C6:E2:08:74\x00>'
>
```

> Hypfer:
okay so by doing the right button presses, the dock first writes some command to the uart
that is then read by the mcu of the robot which in turn responds with other commands and its mac(?)

> Hypfer
hey but that at least seems to actually be the mac that can be found in /mnt/private

> Hypfer
"Software start--" message - that's from the MCU

> Hypfer
I wonder if it's two-way or enough to just write the mac of the robot into the dock

> Hypfer:
maybe the command that makes the MCU respond with the mac makes it enter some kind of pairing mode as well

> Hypfer:
it then responds with its mac and waits for the dock to call out for it with a special message containing said mac?

> Hypfer:
The MAC address saved in the station is part of its flash

> Hypfer:
You can rewrite that. We did that for the p2149

> picakia:
do you have any idea from where this code came from? 
serialport.write(Buffer.from([0x3C, 0x43, 0x54, 0x01, 0x3E, 0x0d]));

> Hypfer:
Unfortunately no. All I have are these chunks of notes and message exchanges back when I was fiddling around with it
