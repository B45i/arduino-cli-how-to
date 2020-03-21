# Arduino CLI on windows 10

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

- Check if the `arduino-cli` is accessible from terminal by typing `arduino-cli`.

- You should get an output similar to this:
  ![cli-test](./img/cli-test.png)
