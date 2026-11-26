# Yuby SDK

<p align="center">
  <img src="docs/banner.svg" alt="Yuby — lowest-latency audio engine" width="960" />
</p>

<p align="center">
  <a href="https://taudj.com/demo/websrc/djplayer.html?platform=web"><img src="https://img.shields.io/badge/%E2%96%B6_Hear_it_live-Tau_web_demo-33ff66?style=for-the-badge&labelColor=020903" alt="Hear it live — Tau web demo" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/macOS-Intel64%20%2B%20ARM64-33ff66?style=flat-square&labelColor=111" alt="macOS" />
  <img src="https://img.shields.io/badge/iOS%20%2F%20iPadOS%20%2F%20tvOS%20%2F%20visionOS-ARM64-33ff66?style=flat-square&labelColor=111" alt="Apple" />
  <img src="https://img.shields.io/badge/Windows-Intel64%20%2B%20ARM64-33ff66?style=flat-square&labelColor=111" alt="Windows" />
  <img src="https://img.shields.io/badge/Android-x86__64%20%2B%20ARM64-33ff66?style=flat-square&labelColor=111" alt="Android" />
  <img src="https://img.shields.io/badge/Linux-Intel64%20%2B%20ARM64-33ff66?style=flat-square&labelColor=111" alt="Linux" />
  <img src="https://img.shields.io/badge/Web-JS%20%2F%20WASM-33ff66?style=flat-square&labelColor=111" alt="Web" />
  <a href="https://yuby.com"><img src="https://img.shields.io/badge/license-Yuby%20SDK-2dbe52?style=flat-square&labelColor=111" alt="License" /></a>
  <a href="https://yuby.com"><img src="https://img.shields.io/badge/site-yuby.com-2dbe52?style=flat-square&labelColor=111" alt="yuby.com" /></a>
</p>

The fastest, lowest-latency audio engine. Same features, quality, and performance on macOS, iOS, Windows, Android, Linux, and the web. Intel 64-bit and ARM64.

Superpowered is shutting down **31 July 2027**. Include [`yubycompat.h`](windows/yubycompat.h) and keep going — same object lifecycle, Yuby syntax. Details below.

## Quickstart

```cpp
#include "yuby.h"

yuby::YubyInit(0, 0, 0); // zeros = time-limited eval; get a key at yuby.com

void *reverb = yuby::Create(yuby::ObjectType::Reverb);
yuby::Set(reverb, yuby::OnOff, 1.0);
yuby::Set(reverb, yuby::WetPercent, 40.0);

// in your existing audio callback:
yuby::Process(reverb, input, nullptr, output, samplerateHz, numFrames);

yuby::Delete(reverb);
```

JS / WASM is the same idea: `Yuby.load()`, `CreateObject(Player | Reverb | …)`, `Process` on the audio thread. See [`examples/webdemo/`](examples/webdemo/).

## Transitioning from Superpowered

One include replaces Superpowered classes for most audio features:

```cpp
#include "yubycompat.h"
```

Objects still create / process / destroy the way they did. Class names live in `SuperpoweredCompat`. Native Yuby types are `yuby::ObjectType`.

| Superpowered | `yubycompat.h` | Native Yuby |
| --- | --- | --- |
| `SuperpoweredAdvancedAudioPlayer` | `AdvancedAudioPlayer` | `ObjectType::Player` |
| `SuperpoweredReverb` | `Reverb` | `ObjectType::Reverb` |
| `SuperpoweredGate` | `Gate` | `ObjectType::Gate` |
| `SuperpoweredRoll` | `Roll` | `ObjectType::Roll` |
| `SuperpoweredBitcrusher` | `Bitcrusher` | `ObjectType::BitCrusher` |
| `SuperpoweredFilter` | `Filter` | `ObjectType::Filter` |
| `Superpowered3BandEQ` | `ThreeBandEQ` | `ObjectType::EQ` |
| `SuperpoweredWhoosh` | `Whoosh` | `ObjectType::Whoosh` |
| `SuperpoweredFlanger` | `Flanger` | `ObjectType::Flanger` |
| `SuperpoweredEcho` | `Echo` | `ObjectType::Echo` |
| `SuperpoweredDelay` | `Delay` | `ObjectType::Delay` |
| `SuperpoweredClipper` | `Clipper` | `ObjectType::Clipper` |
| `SuperpoweredLimiter` | `Limiter` | `ObjectType::Limiter` |
| `SuperpoweredCompressor` / `Compressor2` | `Compressor` / `Compressor2` | `ObjectType::Compressor` |
| `SuperpoweredAutomaticVocalPitchCorrection` | `AutomaticVocalPitchCorrection` | `ObjectType::AutoTune` |
| AEC / echo cancellation | `AEC` | `ObjectType::AEC` |
| `SuperpoweredDecoder` | `Decoder` | `ObjectType::Decoder` |
| `SuperpoweredRecorder` | `Recorder` | `ObjectType::Recorder` |
| `SuperpoweredAnalyzer` | `Analyzer` | `ObjectType::Analyzer` |
| `SuperpoweredBandpassFilterbank` | `BandpassFilterbank` | `ObjectType::BandpassFilterbank` |
| `SuperpoweredResampler` | `Resampler` | `ObjectType::Resampler` |

Time stretching and frequency-domain work are first-class on the player / `ObjectType::TimeStretcher` / `ObjectType::FrequencyDomain` — not a separate Superpowered-style wrapper in `yubycompat.h`.

License: [yuby.com](https://yuby.com) · `licensing@yuby.com`. Evaluation keys exist; public release needs a White Label license.

## Folders

- [`android/`](android/) — Android library and headers.
- [`linux/`](linux/) — Linux static library and headers.
- [`wasm/`](wasm/) — JavaScript/WebAssembly library.
- [`windows/`](windows/) — Windows static library and headers.
- [`yuby.xcframework/`](yuby.xcframework/) — Framework for Apple platforms: macOS, iOS, iPadOS, tvOS, and visionOS.
- [`io/`](io/) — Open-source, cross-platform audio I/O library for Android, Windows, macOS, and iOS. Supports audio device listing and selection, channel mapping (routing), low-latency audio, and multiple sample formats. Supports both WASAPI and ASIO on Windows.

### Examples

- [`examples/androiddemo/`](examples/androiddemo/) — Android Studio project.
- [`examples/appledemo/`](examples/appledemo/) — Xcode project.
- [`examples/webdemo/`](examples/webdemo/) — Web browser demo written in JavaScript.
- [`examples/windemo/`](examples/windemo/) — Visual Studio project.
- [`examples/separatorexample/`](examples/separatorexample/) — AudioShake and Demucs separator integration examples, including a HTDemucs model for all Apple platforms and Windows (unzip the models first).

## API discovery

`yubyinfo.json` enables programmatic API discovery. In C++, you can use `yubyjson.h` to parse it.

## Running the web demo

Zero-install sound is the Tau demo at the top of this page. To run **this repo’s** web example locally:

1. Serve the entire Yuby repository on `localhost`.
2. Configure the server to send CORS and COEP headers (SharedArrayBuffer).
3. Open your browser's developer console.
4. Open `http://localhost:PORT/examples/webdemo/index.html`, replacing `PORT` with your server's port number.
