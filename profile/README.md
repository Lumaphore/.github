# Welcome to Lumaphore

If you're looking for a free ready-to-use Lumaphore app, go check out [Lightdrive](https://lightdrive.app)

# Table of Contents
1. [Summary](#example)
2. [Very Cool Characteristics](#Very-cool-characteristics)
2. [Lumaphore data operations](#Lumaphore-data-operations)
3. [Lumaphore packet structure](#Lumaphore-packet-structure)
4. [FAQ](#FAQ)
4. [Prior Art / References](#Prior-art--References)
4. [Contribute](#contribute)


### what is Lumaphore?
Lumaphore is a protocol that deals with Visible Light Representations of data (VLRs). With it, you can send files from a screen, or off of a piece of paper, and into a camera (like a webcam or smartphone)

Yes this really works. Scan it! ( the first code you scan will forward you to an app that implements the Lumaphore protocol. )

![A Lumaphore GIF stream](https://i.imgur.com/y76bV6a.gif)


To help you get a quick rough mental idea of the technology - imagine if you cut a file into lots of tiny pieces, turn each tiny piece into a QR code, and then scanned all those QR codes. In very simple terms this is what Lumaphore is about.

You could arrange such a process by "streaming" or cycling through those QR codes very quickly on a monitor. 

You could also use printed paper as a VLR.
Lumaphore enables you to lock down physical paper documents with passwords and encryption. Imagine paper covered in QR codes, each a tiny piece of a file that was compressed and encrypted beforehand (This feature is available in Lightdrive already, after selecting a file to send, go to options > save to Paperdrive).

![the first paperdrive document created](https://miro.medium.com/max/1400/1*TfW4zXw7UiZaxgrP439rKQ.jpeg)

---

### Very cool characteristics
---
## closing the biggest encryption gap in the world
Mail fraud costs the global economy hundreds of billions of dollars 

---

## protocol hijacking
Many smartphones' default camera apps are capable of detecting QR codes these days. 

Many people recognise and know how to scan QR codes. 

Lumaphore uses QR codes to encode each packet of data.

Mix that all together and we have an easier-than-usual onboarding process for Lumaphore.

The user sees a QR code, the phone reads the QR code, the URL prepended to the data in the QR code triggers a redirect to your app which implements Lumaphor. Easy! People can start using Lumaphor without knowing it exists.

---

## magic

---
## Lumaphore data operations
The sending and scanning of files with Lumaphore involves a sequence of data-processing operations:

### pre-send data operations

0. (user selects file)
1. compress
2. encrypt
3. fountain
4. packet formatting


> (Visible Light Representation is created and scanned)

### post-scan data operations

5. defountain
6. decrypt
7. decompress   
8. (user downloads reconstituted file)

----

## Lumaphore packet structure
Packets are the contents of visual encodings (1 packet = 1 QR code). 
Most packets are "data" packets containing bits of the file.
Frequent "metadata" packets are interspersed to give recipients and the scanning application information about the file. 

Packet subsections are separated by semicolon.

The `PAYLOAD` will contain either metadata or data.

```
PREFIX;FRAME_TYPE;FRAME_NUMBER;PAYLOAD

- protocol / onboarding prefix  STRING
- frame type (0,1)              NUMBER
    - 0 = metadata frame
    - 1 = data frame
- frame number                  NUMBER
- payload                       ...


```

## metadata frame payload structure
```
- filename          STRING
- filetype          STRING
- encrypted         BOOL
- size              NUMBER
- total frames      NUMBER
- public metadata   BOOL
- sender            STRING
- recipient         STRING
- auth group        STRING
- fps               NUMBER
```
metadata frame example:
```
https://lightdrive.app/#/scan?a=;1;7;11;5;130,66,247,70,101,233

PREFIX;FRAME_TYPE;FRAME_NUMBER;FILENAME;FILETYPE;ENCRYPTED;SIZE;TOTAL_FRAMES;PUBLIC_METADATA;SENDER;RECIPIENT;AUTH_GROUP;FPS
```
(the prefix here is a working example from Lightdrive "https://lightdrive.app/#/scan?a=")

-----

## data frame payload structure
```
- degree NUMBER
- K NUMBER
- data INT[]
```

data frame example:

```
https://lightdrive.app/#/scan?a=;1;7;11;5;130,66,247,70,101,233

PREFIX;FRAME_TYPE;FRAME_NUMBER;DEGREE;K;DATA
```
(the prefix here is a working example from Lightdrive "https://lightdrive.app/#/scan?a=")

--- 

## Glossary

- Visible Light Representation (VLR) 
    - whichever visible medium you use for representing 
- Onboarding Prefix
    - these are strings appended to the beginning of datapackets for the purpose of onboarding users that aren't familiar with Lumaphore applications by forwarding them to applications that implement Lumaphore.
- Fountain Codes
    - Lumaphore makes use of [Fountain Codes](https://en.wikipedia.org/wiki/Fountain_code). These allow a percentage of the total data packets to be missed or destroyed without affecting the successful transmission of the file. This is perfect for environments where you can't guarantee that every packet will be received.
---

## FAQ
> Isn't this just QR codes?

We use [QR codes](https://en.wikipedia.org/wiki/QR_code) for the visual encoding of data packets. We may eventually support other visual encoding standards such as [JABcode](https://github.com/jabcode/jabcode), for higher bandwidth. For now QR codes have amazing benefits, such as wide support in camera apps by smartphone manufacturers. This means that while people might not yet have applications on their devices that implement Lumaphore, we can use QR codes to fairly seemlessly redirect users to apps that *do* implement Lumaphore (for example [Lightdrive](https://lightdrive.app)). I guess you could say that Lumaphore isn't "just QR codes", in the same way that WiFi and Bluetooth are not just noise on radio frequencies.


---
## Prior art / References 

### scanning large codes:
I saw this anime, [Ghost In The Shell: Stand Alone Complex](https://en.wikipedia.org/wiki/Ghost_in_the_Shell:_Stand_Alone_Complex) back in 2012 when I was creating Oriscript. It solidified my interest in making some sort of "streaming visible light data thing".

![A scene from GITS:SAC where a character scans large amounts of data with their eyes](https://miro.medium.com/max/1200/1*kLQzFwhZwWb51DinpxUvfQ.gif)

### Fountain Codes:
While googling stuff like "losing data packets during transmission", "raptor codes" etc., I came across some amazing articles by Ivan Daniluk, whose code we ended up going through and learning from for our first implementation of fountain encoding. Check them out! (thanks Ivan!) https://divan.dev/posts/fountaincodes/

