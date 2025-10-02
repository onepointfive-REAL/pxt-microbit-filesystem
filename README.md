# File system driver
New instructions - Filesystem has been removed from makecode official extensions

To use this package, go to https://makecode.microbit.org, click ``Extensions`` and search for `https://github.com/onepointfive-REAL/pxt-microbit-filesystem` and press enter, it will automatically popup.

DEPRECATED - This package is no longer maintained or supported.

## Usage

The package allows to read and write files to the micro:bit flash.

The entire file system content is ERASED when a new .hex file is download onto the micro:bit.

### Writing data

* append text and a new line character

```blocks
files.appendLine("data.txt", "Hello");
```

* append text to the file

```blocks
files.appendString("data.txt", "Hello");
```

* append a number (as text) to the file

```blocks
files.appendNumber("data.txt", 42);
```

### Reading data

* send the content of a file to serial
  
<img width="356" height="133" alt="Screenshot 2025-10-02 10 18 26 AM" src="https://github.com/user-attachments/assets/98e75d60-d1e7-4bd7-baf2-9ee15dc190c5" />

```blocks
files.readToSerial("data.txt");
```

### Settings

The package allows to save and load number settings based on the file system

<img width="589" height="183" alt="Screenshot 2025-10-02 10 21 15 AM" src="https://github.com/user-attachments/assets/4f6364a4-d71c-4079-849c-e4ae3475e5d7" />

* save setting value

```blocks
files.settingsSaveNumber("calibrated", 1)
```

* read setting value

```blocks
let calibrated = files.settingsReadNumber("calibrated");
```

### File class

The ``File`` class allows to keep a file instance open, manipulate the pointer position and read from the file.


* open, flush or close the file
  
<img width="383" height="234" alt="Screenshot 2025-10-02 10 23 24 AM" src="https://github.com/user-attachments/assets/4f7df3be-73ec-44b9-a3a6-46d83c7916be" />

```blocks
let f = files.open("data.txt");
f.flush();
f.close();
```

* write strings or buffers
  
<img width="363" height="183" alt="Screenshot 2025-10-02 10 25 05 AM" src="https://github.com/user-attachments/assets/3df7e59f-5bd1-479d-831e-f81cd29deca5" />

```blocks
let f = files.open("data.txt");
f.writeString("yay");
```


* read data
  
  <img width="497" height="230" alt="Screenshot 2025-10-02 10 26 43 AM" src="https://github.com/user-attachments/assets/dd532718-8872-4647-a6b2-cbc1240ef801" />

```blocks
let f = files.open("data.txt");
let buf = f.readBuffer(64);
let c = f.read();
```

* set the cursor position
  
<img width="344" height="219" alt="Screenshot 2025-10-02 10 27 40 AM" src="https://github.com/user-attachments/assets/3c539c3c-8501-42af-9776-1fdb385e6f0a" />

```blocks
let f = files.open("data.txt");
f.setPosition(42);
let pos = f.position();
```

## Example: Writing accelerometer data

The following program allows to collect accelerometer data and save it in a ``data.csv`` file. 
When the user presses button ``A``, the micro:bit pauses for 3 seconds, then starts collecting 720 acceleration samples.
Each sample is written to the file in a format that can be important by spreadsheet programs (CSV).

<img width="966" height="479" alt="Screenshot 2025-10-02 10 29 07 AM" src="https://github.com/user-attachments/assets/91b56b38-6f90-4c17-8cce-187816a7f840" />

```blocks
let file = "data.csv";
input.onButtonPressed(Button.A, () => {    
    basic.pause(3000);
    files.remove(file);
    files.appendLine(file, "Time\tAcceleration");
    for (let i = 0; i < 100; ++i) {
        let t = input.runningTime();
        let ay = input.acceleration(Dimension.Y);
        files.appendLine(file, t + "\t" + ay);
        control.waitMicros(20);
    }
});
input.onButtonPressed(Button.B, () => {
    files.readToSerial(file);
    basic.showString(":)")
})
```

## Supported targets

* for PXT/ microbit
* for PXT/ calliope

## License

MIT

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
