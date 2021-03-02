# Clipper
Simple app to interact with Android system clipboard service via adb shell one liners.

# Installation
Download the [application apk](https://github.com/majido/clipper/releases/download/v1.2.1/clipper.apk) and manually install application on your android device.

# Usage
Assuming you have already installed the app, connect to your emulator or phone using adb shell.
First start the service. You can do this either by opening the application or using the following commands on ADB shell
	$ adb shell
	# am startservice ca.zgrs.clipper/.ClipboardService


Once service is started you can invoke clipper service by broadcasting intents.
The intent's *Action* can be either "get" or "set". When setting the clipboard value, pass your string as an *Extra* parameter.

* Supported actions
  1. **get**: print the value in clipboard into logs (TODO: print the value on standard output)
  2. **set**: sets the clipboard content to the string passed via extra parameter "text"
* Supported extras
  1. **text**: The text that you want to be copied in the clipboard

Usage example using broadcast intent:

	# am broadcast -a clipper.set -e text "this can be pasted now" ca.zgrs.clipper
	# am broadcast -a clipper.get ca.zgrs.clipper

# Building
Build using gradle
  ./gradlew assemble

# Simplify Usage

1. install [clipper.apk](https://github.com/majido/clipper/releases/download/v1.2.1/clipper.apk)

> adb install clipper.apk

2. add the two methods below to ~/.bash_profile.

```
function get_clip() {
	adb shell am start -W -n ca.zgrs.clipper/ca.zgrs.clipper.Main;
	adb shell am broadcast -a clipper.get ca.zgrs.clipper;
	adb shell input keyevent 4;
}

function set_clip() {
	adb shell am start -W -n ca.zgrs.clipper/ca.zgrs.clipper.Main;
	adb shell "am broadcast -a clipper.set -e text '$1' ca.zgrs.clipper";
	adb shell input keyevent 4;
}
```

3. source environment variable

> source ~/.bash_profile

或者

> source ~/.zsh_rc

4. instructions

> get Clipboard: get_clip

> set Clipboard: set_clip AAA
