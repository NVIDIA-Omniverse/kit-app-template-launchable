# Kit App Template Launchable Usage

This project provides a simple way to interact with Kit App Template, either on the cloud or locally.
See the [project repo](https://github.com/NVIDIA-Omniverse/kit-app-template-launchable) for more detailed instructions, but below is a quickstart guide.

## Web Viewer for Kit App UI

### To view in a separate tab
1. Run a command that starts your Kit App (see below)
2. Copy your current URL (ie `https://kat.brevlab-1234`)
3. Open a new tab in your browser and paste the URL, changing the end to `/viewer/` (ie `https://kat.brevlab-1234/viewer/`)
4. The Kit App UI will appear when the app is ready. The page will say "Waiting for stream..." until then.

### To view inside VSCode
1. Run a command that starts your Kit App (see below)
2. Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac)
3. Type "Simple Browser: Show"
4. Enter URL: `/viewer/`
5. The Kit App UI will appear when the app is ready. The page will say "Waiting for stream..." until then.

## Getting Started with Kit App Template

Create a new kit application. Make sure to create a default streaming layer when prompted.

```
cd kit-app-template
./repo.sh template new
./repo.sh build
./repo.sh launch -- --no-window
```

Then follow the Web Viewer instructions above to view the UI.
