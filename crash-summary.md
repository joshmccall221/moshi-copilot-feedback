# Crash summary (identifiers removed; full report available on request)

- Process: `moshi-desktop-tauri` (Intel x86-64 build), `/Applications/Moshi.app`
- OS: macOS 15.7.9 (24G830), iMac19,1, 64 GB RAM
- Exception: `EXC_CRASH (SIGABRT)`, "abort() called"
- Crashed thread: 0 (main, `com.apple.main-thread`)
- Trigger path (top to bottom):
  `abort` > moshi-desktop-tauri frames > `WebKit::UIDelegate::UIClient::runOpenPanel`
  > `WebKit::WebPageProxy::runOpenPanel` > WebKit IPC dispatch > AppKit run loop
- Reading: the app's WKWebView open-panel (file chooser) delegate aborts, likely a
  Rust panic or an unhandled error in the Tauri-side handler. Opening a file
  input (upload/attach) is the trigger.
- Memory at crash: ~2.7 GB MALLOC, ~226 MB WebKit Malloc, 8.8 GB total VM
  (4.8 GB excluding reserved). Uptime 85,000 s (~23.6 h) since boot. Possibly
  related to the slowdown described in the README.
