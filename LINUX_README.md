# Running Glass on Linux

This guide provides instructions for setting up and running **Glass** on Linux distributions (tested on Ubuntu/Debian).

## Prerequisites

1.  **Node.js**: Version 20.x or higher is recommended.
    ```bash
    node -v
    # Should be v20.x.x or higher
    ```
    If not installed, use [nvm](https://github.com/nvm-sh/nvm):
    ```bash
    nvm install 20
    nvm use 20
    ```

2.  **Ollama**: Automatic installation is not supported on Linux. You must install it manually to use local AI features.
    ```bash
    curl -fsSL https://ollama.com/install.sh | sh
    ```
    Start the Ollama service:
    ```bash
    ollama serve
    ```

3.  **Python**: Required for some build scripts.
    ```bash
    python3 --version
    ```

## Installation

1.  Clone the repository (if you haven't already).
2.  Install dependencies and setup the project.
    ```bash
    npm run setup
    ```
    This command will:
    *   Install root dependencies.
    *   Install `pickleglass_web` dependencies.
    *   Build the frontend.
    *   Start the application.

## Running the Application

To start the application after the initial setup:

```bash
npm start
```

### Potential Issues on Linux

#### Window Transparency
Glass relies on transparent windows. If you see a black background instead of transparency, try running with:
```bash
npm start -- --disable-gpu
```
or
```bash
npm start -- --enable-transparent-visuals --disable-gpu
```

#### System Audio Capture
The **"Listen"** feature (for meeting transcription) relies on a macOS-specific binary (`SystemAudioDump`) to capture system audio.
*   **On Linux, this feature is currently disabled.**
*   **Microphone capture** (your voice) should still work if you select the correct input device.

#### Wayland vs X11
Screen recording (`desktopCapturer`) works best on X11.
If you are on Wayland (default on newer Ubuntu/Fedora), you might need to force X11 for Electron compatibility if screen sharing fails:
```bash
npm start -- --ozon-platform=x11
```

## Troubleshooting

*   **Error: `libgconf-2-4` is missing**:
    Install it via: `sudo apt-get install libgconf-2-4`
*   **Error: `better_sqlite3` build failed**:
    This usually happens if you are not using **Node.js 20**.
    Check your version with `node -v` and switch using `nvm use 20`.
    You may also need build tools: `sudo apt-get install build-essential python3`
*   **Error: `Electron failed to install correctly`**:
    This can happen if dependencies were installed with `--ignore-scripts`.
    Fix it by running: `node node_modules/electron/install.js`
*   **Ollama connection refused**:
    Ensure `ollama serve` is running in a separate terminal window.
