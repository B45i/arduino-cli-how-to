# arduino-cli - Getting started guide

One software that I use very often but absolutely hate is the Arduino IDE.

The UI is simple and non-intimidating, even a beginner can use it very easily without any trouble.
But I will say Arduino IDE is bit underwhelming as an IDE, At least by today's standard.

- It doesn't have proper intellisense
- It doesn't have goto definition/ goto declaration feature
- No variable renaming
- Even though it supports tabs, You can only open files from the same project.
- No git integration.
- No custom font support.
- Indentation is using 2 spaces only and no way to control it.
- Most of all, NO DARK THEME. (I know we can tweak the theme little bit but it always looked imperfect).

I can go on about the reasons to hate Arduino IDE.

Over time the Arduino team has added some nice features to the IDE but the development experience is same. You can feel that its a software from the 2000s.

There were some attempts like Arduino extension for VS Code and plarformIO
PlatformIO has a different code structure so if you share your project online, Beginners will have a hard time recreating your project.
For the Arduino extension to work, You need to have the Arduino IDE already installed in your system. I felt it like a "hacky" setup.

About a year ago, Arduino launched arduino-cli, their CLI tool written in google's go programing language is powering Arduino Create Web Editor.

Advantage of the arduin-cli is that you can write code in your favourite text editor and compile and upload the code to the specified board using the CLI tool.

CLI is very small in size (about 7MB). but packs all the major feature of the IDE.

## Install the cli

### On windows

1. Download the cli tool from [here](https://arduino.github.io/arduino-cli/installation/)

2. Extract the `arduino-cli.exe` file to a folder which is indexed in `PATH`.

- Either you can move the `arduino-cli.exe` to `C:\Windows\System32`

  **or**

- add the `arduino-cli.exe` path to system variable.

- I crated a folder: `C:\Program Files\arduino-cli`, and added it into my path variable.

- follow [this](https://stackoverflow.com/questions/44272416/how-to-add-a-folder-to-path-environment-variable-in-windows-10-with-screensho) link if need help in adding folder to system variable.
  ![path variable](./img/PATH.PNG)

## Check the installation

- Check if the `arduino-cli` is accessible from terminal by typing:
  `arduino-cli`.

- You should get an output similar to this:
  ![cli-test](./img/cli-test.png)

- If everything is working, update the local cache of available platforms and libraries by running:
  `arduino-cli core update-index`

## Create a new sketch

New sketch can be created in current folder by the command:
`arduino-cli sketch new MyFirstSketch`

eg:

```bash
$ arduino-cli sketch new cli-blink
Sketch created in: C:\projects\cli-blink
```

A folder with `.ino` file will be create for you.

You can open this file in you favorite IDE or text editor.

## See the connected board

- Connect the board to your PC and run the command to see the list of board attached to it.

`arduino-cli board list`

```bash
$ arduino-cli board list
Port Type Board Name FQBN Core
/dev/ttyACM1 Serial Port (USB) Arduino/Genuino MKR1000 arduino:samd:mkr1000 arduino:samd
```

## Compile and upload the sketch

Once you open the code in your favorite Editor, Add you code to the `.ino` file

```cpp
void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
    digitalWrite(LED_BUILTIN, HIGH);
    delay(1000);
    digitalWrite(LED_BUILTIN, LOW);
    delay(1000);
}
```

### Compile the sketch

Run the following command to compile the sketch

`arduino-cli compile --fqbn YourBoardFQBN YourSketchName`

- Here the `FQBN` stands for **Fully Qualified board name**, which is used to denote a particular board.

- for example `arduino:avr:nano` is the `fqbn` for `Arduino Nano`,
  and arduino:avr:uno for `Uno` boards

- To compile sketch called `cli-blink` for `Arduino Nano` run the following command.

`arduino-cli compile --fqbn arduino:avr:nano cli-blink`

and you will get output like

```bash
$ arduino-cli compile --fqbn arduino:avr:nano cli-blink
Sketch uses 924 bytes (3%) of program storage space. Maximum is 30720 bytes.
Global variables use 9 bytes (0%) of dynamic memory, leaving 2039 bytes for local variables. Maximum is 2048 bytes.
```

- If you get any error like

  > Error during build: platform not installed

  Then run the command: `arduino-cli core install arduino:avr`, which will install missing platform.

- To list all the supported boards and its FQBN in you machine use command:

`arduino-cli board listall`

### Upload the sketch

- Use the command:
  `arduino-cli upload -p YourBoardPort --fqbn YourBoardFQBN YourSketchName`

- To compile sketch called `cli-blink` for `Arduino Nano` connected in Port `COM3` run the following command:

`arduino-cli upload -p COM3 --fqbn arduino:avr:nano cli-blink`

## Install 3rd party cores

- You can use `arduino-cli` to compile and upload code to non AVR boards as well such as `ESP8266` and `ESP32`.

- For this, we need to add the additional package indexes in the Arduino CLI configuration file.

- Create a configuration file if it doesn't exist by the command:
  `arduino-cli config init`

```bash
$ arduino-cli config init
Config file written to: C:\Users\B45i\AppData\Local\Arduino15\arduino-cli.
yaml
```

- Open the configuration file in any editor and add new board manger url to `board_manager > additional_urls`.

- To add ESP8266 with URL: `https://arduino.esp8266.com/stable/package_esp8266com_index.json`,
  Edit the `yaml` file like this:

```yaml
board_manager:
  additional_urls:
    [https://arduino.esp8266.com/stable/package_esp8266com_index.json]
```

- now update the core by command:
  `arduino-cli core update-index`

```bash
$ arduino-cli core update-index
Updating index: package_index.json downloaded
Updating index: package_esp8266com_index.json downloaded
```

- If you do `arduino-cli core search esp8266` you can see esp8266 listed.

- Compile code for nodeMCU:

`arduino-cli compile --fqbn esp8266:esp8266:nodemcuv2 cli-blink`

- Upload to nodeMCU connected on `COM4`:

`arduino-cli upload -p COM4 --fqbn esp8266:esp8266:nodemcuv2 cli-blink`

## Install libraries

- You can search for additional libraries by:
  `arduino-cli lib search LibraryName`

```bash
$ arduino-cli lib search wifimanager
Name: "WiFiManager"
  Author: tzapu
  Maintainer: tzapu
  Sentence: ESP8266 WiFi Connection manager with fallback web configuration portal
  Paragraph: Library for configuring ESP8266 modules WiFi credentials at runtime.
  Website: https://github.com/tzapu/WiFiManager.git
  Category: Communication
  Architecture: esp8266
  Types: Contributed
  Versions: [0.5.0, 0.6.0, 0.7.0, 0.8.0, 0.9.0, 0.10.0, 0.11.0, 0.12.0, 0.13.0, 0.14.0, 0.15.0-beta, 0.15.0]
```

- Install the library by:

`arduino-cli lib install WiFiManager`

```bash
$ arduino-cli lib install WiFiManager
WiFiManager depends on WiFiManager@0.15.0
Downloading WiFiManager@0.15.0...
WiFiManager@0.15.0 downloaded
Installing WiFiManager@0.15.0...
Installed WiFiManager@0.15.0
```
