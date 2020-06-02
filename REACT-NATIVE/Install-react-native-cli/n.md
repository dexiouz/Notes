This "tutorial" assumes you already have the **Android SDK** installed.

To install react native on linux you must have **node** and **npm** installed.

Let's start:

## Update your PC with
```
$ sudo apt update
```

## Install open jdk
Do this using the following on the command line 
```
$ sudo apt install openjdk-8-jdk
```
## Install react Native CLI

Assuming you have node installed, the next step is to install the react native CLI. To do this, run the following code on your command line.
```
  $ npm install -g react-native-cli
```
## Add Android home environmental variables
If you're using bash as your shell, go to `$HOME/.bash_profile` or `$HOME/.bashrc` config file and add the following lines
```
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

If you're using `ZSH`, it should be `$HOME/.zshrc`

To open `$HOME/.bashrc`, go to the terminal and type 
```
 $ nano .bashrc
```
Scroll to the bottom and add these lines of code at the last line
```
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

When done, Press `ctrl + s` to save and `ctrl + x` to exit.

Next load the config file into your shell by typing the following on the command line
```
$ source $HOME/.bashrc
```

Comfirm that ANDROID_HOME has been added to your path by typing
```
$ echo $PATH
```
You should get this on your screen 

```
/home/user/.nvm/versions/node/v10.16.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/chidera/Android/Sdk/emulator:/home/chidera/Android/Sdk/tools:/home/chidera/Android/Sdk/tools/bin:/home/chidera/Android/Sdk/platform-tools

```

## Create a React Native App
If you got to this point, congrats. Next let's create a react native app on the desktop or any directory of your choice
```
Desktop$ react-native init reactNativeProject
```

## Run the react native app
First cd into your project
```
Desktop $ cd reactNativeProject
```
the next thing is to

- enable USB debugging on your phone, and
- connect your phone to your PC, then run 

And run the following
```
$ react-native run-android
```

## Start the project

After running 
```
$ react-native run-android
```

```
$ yarn start
```

This will start the project. 