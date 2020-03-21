# Using Arduino CLI on windows 10

## Install the cli

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

> \$ arduino-cli sketch new cli-blink
> Sketch created in: C:\projects\cli-blink

A folder with `.ino` file will be create for you.

You can open this file in you favorite IDE or text editor.

## See the connected board

- Connect the board to your PC and run the command to see the list of board attached to it.

`arduino-cli board list`

> \$ arduino-cli board list
> Port Type Board Name FQBN Core
> /dev/ttyACM1 Serial Port (USB) Arduino/Genuino MKR1000 arduino:samd:mkr1000 arduino:samd

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

> \$ arduino-cli compile --fqbn arduino:avr:nano cli-blink
> Sketch uses 924 bytes (3%) of program storage space. Maximum is 30720 bytes.
> Global variables use 9 bytes (0%) of dynamic memory, leaving 2039 bytes for local variables. Maximum is 2048 bytes.

- If you get any error like

  > Error during build: platform not installed

  Then run the command: `arduino-cli core install arduino:avr`, which will install missing platform.

### Upload the sketch

- Use the command:
  `arduino-cli upload -p YourBoardPort --fqbn YourBoardFQBN YourSketchName`

- To compile sketch called `cli-blink` for `Arduino Nano` connected in Port `COM3` run the following command:

  `arduino-cli upload -p COM3 --fqbn arduino:avr:nano cli-blink`
