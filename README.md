# Forge AI Studio

Forge is a self-hosted AI workspace prototype for organizing open-weight models, fine-tuning runs, datasets, and deployments. The current project is a browser-only product prototype; it does not download model weights, launch training, or host inference endpoints.

## Open the prototype

Serve the `web/` folder with any static file server, then open its local URL in a browser. For example, run `python -m http.server 8000 --directory web` from this folder and visit `http://localhost:8000`.

The workspace screens are responsive. Model, dataset, deployment, chat-preview, and demo API-key records persist in the current browser's local storage. Use **Settings → Reset demo** to restore sample records.

## What is and is not connected

- **Implemented:** responsive dashboard and navigation; browser-local create/remove flows for model records, training-job records, endpoint records, datasets, and demo keys; a simulated chat flow; usage and setup views.
- **Sample data:** usage figures, model catalog entries, training history, and deployment records. They are illustrative and are not observed from a machine or service.
- **Not implemented:** authentication, a database, model downloads, dataset file upload, training orchestration, GPU scheduling, real inference, real API-key authentication, public networking, or always-on hosting.

Do not use the demo API keys for real applications. Browser local storage is not a secure secret store. The UI calls out the runtime, power, network, and hardware needed for 24/7 self-hosting. Software may be free or open source, but no software can make hardware and electricity free for life.

## Suggested self-hosted MVP path

1. Add a small local API service and persistent database for users, models, datasets, jobs, and endpoints.
2. Integrate a local inference runtime such as Ollama for a low-friction first run; add a vLLM or llama.cpp adapter for other hardware and serving needs.
3. Connect the playground to the runtime, display its actual installed models and health, and move credentials out of browser storage.
4. Add dataset upload and validation, then queue LoRA/QLoRA training through a user-configured GPU worker. Training can be implemented with tools such as Axolotl or Unsloth where their model, dataset, and hardware support fits.
5. Launch inference deployments as isolated services, add access controls, logs, health checks, and resource limits, and document secure remote access before exposing an endpoint to the internet.

Each runtime and model has its own hardware requirements and license. The user supplies the machine, storage, network connection, and any required model access. Keep a cost and power estimate visible before launching long-running jobs.

The appearance toggle in the top bar switches between light and dark themes. Your selection is saved in this browser; first visit follows the operating system theme.

## Windows and Android app builds

The same responsive UI is wrapped in a Tauri 2 native shell. Install Node.js LTS with npm, Rust (MSVC toolchain), and the Microsoft C++ Build Tools to build on Windows. WebView2 is also required (usually already present on supported Windows versions).

```powershell
npm install
npm run dev
npm run build:windows
```

The Windows installer bundles are written under `src-tauri/target/release/bundle/`.

For Android, install Android Studio and its SDK Platform, Platform-Tools, NDK, Build-Tools, and command-line tools; configure `JAVA_HOME`, `ANDROID_HOME`, and `NDK_HOME`; and install the Android Rust targets. Then run:

```powershell
npm run android:init
npm run android:dev
npm run android:apk
```

Use `npm run android:aab` for a Play Store bundle. Android release distribution requires signing. This repository contains the Tauri project configuration; Android Studio's generated Gradle project is created by `android:init` on a machine with the Android toolchain installed.
