![Agent logo & text](https://raw.githubusercontent.com/UltimateHackingKeyboard/agent/master/packages/uhk-web/src/assets/images/agent-logo-with-text.png)

Agent is the configuration application of the [Ultimate Hacking Keyboard](https://ultimatehackingkeyboard.com/).

Please do not build Agent from source unless you want to develop it. Using an existing release is far easier for everyone involved:

* If you already own a UHK then [download the latest desktop build of Agent](https://github.com/UltimateHackingKeyboard/agent/releases/latest) from the releases page. If you don't own a UHK then you won't get past the opening screen!
  * On Linux, download the AppImage, make it executable, and run it with the `--no-sandbox` option. On recent versions of Ubuntu, you may need to install the `libfuse2t64` package.
* If you don't own a UHK yet then [try out the web build of Agent](https://ultimatehackingkeyboard.github.io/agent/) in your browser. This is meant to be used for demonstration purposes.

## Building the electron application

### Step 1: Build Dependencies

You'll need Node.js 20.x. Use your OS package manager to install it. [Check the NodeJS site for more info.](https://nodejs.org/en/download/package-manager/ "Installing Node.js via package manager") Mac OS users can simply `brew install node` to get both. Should you need multiple Node.js versions on the same computer, use Node Version Manager for [Mac/Linux](https://github.com/creationix/nvm) or for [Windows](https://github.com/coreybutler/nvm-windows)
I had trouble running UHK Agent on Ubuntu 25.10 -- I first got this:

```
%` ./UHK.Agent-8.0.1-linux-x86_64.AppImage
dlopen(): error loading libfuse.so.2

AppImages require FUSE to run.
You might still be able to extract the contents of this AppImage
if you run it with the --appimage-extract option.
See https://github.com/AppImage/AppImageKit/wiki/FUSE
for more information
```

It looks like it needs `libfuse2`; I have `libfuse3` installed. (See [this issue](https://github.com/upscayl/upscayl/issues/1385)). I installed `libfuse2t64` and tried again, but I got:

```
% ./UHK.Agent-8.0.1-linux-x86_64.AppImage
The setuid sandbox is not running as root. Common causes:
  * An unprivileged process using ptrace on it, like a debugger.
  * A parent process set prctl(PR_SET_NO_NEW_PRIVS, ...)
Failed to move to new namespace: PID namespaces supported, Network namespace supported, but failed: errno = Operation not permitted
[663429:1225/055730.130874:FATAL:content/browser/zygote_host/zygote_host_impl_linux.cc:211] Check failed: . : Invalid argument (22)
zsh: trace trap (core dumped)  ./UHK.Agent-8.0.1-linux-x86_64.AppImage
```

I went down a bit of a rabbit hole from there; I tried running it with sudo. I also tried using the `--appimage-extract` thing suggested initially. That in turn complained about root ownership and 4755 permissions, but even with that it didn't work.

In the end, I got it to run with `--no-sandbox`.

It would be useful if there was some documentation on this -- maybe say something in the README?

You'll also need `libusb`.
On debian-based linux distros, `apt-get install libusb-dev libudev-dev g++` is sufficient.
On Mac OS, use `brew install libusb libusb-compat`.
For everyone else, use the appropriate package manager for your OS.

### Step 2: Build Environment

```
git clone git@github.com:UltimateHackingKeyboard/agent.git
cd agent
npm install
npm run build
npm run electron
```

At this point, Agent should be running on your machine.

## Developing the web application

- The frontend code is located in `packages/uhk-web/`
- Run the project locally with `npm run server:web`
- View the app at `http://localhost:8080`
- The app will automatically reload when you make changes

## Contributing

Wanna contribute? Please let us show you [how](CONTRIBUTING.md).
