# Home Assistant app: Ollama

Run AI models on your Home Assistant host with [Ollama](https://ollama.com), for Home Assistant's built-in Ollama integration: AI tasks and conversation agents, without the cloud.

- Built on your host from Ollama's official image, pinned by version and digest, so your host builds exactly the image that was tested.
- Ollama's cloud models and web search are turned off.
- Reachable only on Home Assistant's internal network: it opens no port on your LAN.
- Sees only its own data folder, with no access to your Home Assistant configuration or API.
- Models are left out of backups.
- Runs at low CPU priority, keeps at most one model in memory and unloads it after 5 idle minutes.

Supports amd64 and aarch64 (64-bit ARM). See the **Documentation** tab to set it up.

This app isn't affiliated with or endorsed by Ollama.
