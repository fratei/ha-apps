# Ollama

This app runs [Ollama](https://ollama.com) on your Home Assistant host, so Home Assistant's built-in **Ollama** integration can use local AI models for AI tasks and conversation agents. Ollama's cloud is turned off, and the app opens no port on your network.

## Installation

1. In Home Assistant, go to **Settings** > **Apps** > **App store**.
2. Select ⋮ in the top right > **Repositories**, add `https://github.com/fratei/ha-apps`, select **Add**, then **Close**.
3. Find **Ollama** in the store and select **Install**. Your host builds the app from Ollama's official image, a download of several GB, so this takes a while.
4. Turn on **Start on boot** and **Watchdog**, then select **Start**.
5. On the app's **Info** page, note the **Hostname**, for example `1a2b3c4d-ollama`. You need it in the next section.

The app has no options and no web interface.

## Connect Home Assistant

1. Go to **Settings** > **Devices & services** > **Add integration**, and choose **Ollama**.
2. For **URL**, enter `http://<hostname>:11434`, with the hostname from the app's Info page, for example `http://1a2b3c4d-ollama:11434`. Leave **API key** empty.
3. On the new Ollama entry, select **Add AI task**, or **Add conversation agent** for Assist. Choose a model and save. Home Assistant asks the app to download the model, which can take several minutes.

Suggested settings for an AI task:

| Setting | Value | Why |
| --- | --- | --- |
| Model | `qwen3:4b-instruct` | Home Assistant's default: about 2.5 GB, and it runs well on a CPU. |
| Context window size | 8192 | Enough for most AI tasks. A larger window uses more memory. |
| Keep alive | 300 | Seconds. The integration's default, -1, keeps the model in memory for good and overrides the app's 5 minutes. |
| Think before responding | Off | Faster answers. |

## Configuration

The app has no options. Its settings are built in:

| Setting | Value | Effect |
| --- | --- | --- |
| `OLLAMA_HOST` | `0.0.0.0:11434` | Listens on the app's address on Home Assistant's internal network. No port is published on your host. |
| `OLLAMA_MODELS` | `/data/models` | Stores models in the app's own data folder. |
| `OLLAMA_NO_CLOUD` | `1` | Turns off Ollama's cloud: cloud models, web search and web fetch. |
| `OLLAMA_KEEP_ALIVE` | `5m` | Unloads a model after 5 idle minutes, unless a request asks for another time. |
| `OLLAMA_MAX_LOADED_MODELS` | `1` | Keeps at most one model in memory. |
| `OLLAMA_NUM_PARALLEL` | `1` | Answers one request at a time; others wait their turn. |

The server runs as `nice -n 10 ollama serve`, at low CPU priority, so Home Assistant gets the CPU first when both are busy.

## Security

- **Not on your network.** The app publishes no ports. Only Home Assistant and other apps on its internal network can reach port 11434; other devices on your LAN can't.
- **No authentication.** Ollama has no login, so any app on the internal network can use it. Install only apps you trust.
- **Only its own folder.** The app has no folder mappings. It sees only its own `/data` folder, not your Home Assistant configuration, media or shares.
- **No API access or extra privileges.** It can't call Home Assistant's or Supervisor's API, has no extra privileges and runs under Docker's default AppArmor profile.
- **No cloud.** Ollama refuses cloud models (names ending in `-cloud`), web search and web fetch, with the error "ollama cloud is disabled". Downloads from Ollama's model library still work.
- **Models stay out of backups**, which keeps backups small. After you restore a backup, open the AI task on the Ollama entry, select **Reconfigure** and save it, so Home Assistant downloads the model again.
- **Pinned and tested.** Ollama's version is pinned. A weekly job test-builds each new stable release for both architectures before the app is updated.
- **Logs.** Debug logging and request logging are off, and the app has no option to turn them on. For each request, the log shows its path, timing and token counts, but not the prompt or the reply. Prompts and replies stay in memory: the build check confirms that none reach the log or a file, including before each Ollama update.

## Choosing a model

- Models run on the CPU. Smaller models answer faster: a 4B model such as `qwen3:4b-instruct` can take from a few seconds to a minute or more per answer, depending on your CPU and the length of the prompt.
- A model needs about its download size in memory, plus more for the context window. Keep it well under half of your host's memory, so Home Assistant, backups and other apps still have room. For example, on a host with 32 GB, use models well under 16 GB.
- Only models from Ollama's library work; cloud models are refused.

## Hardware acceleration

The app runs models on the CPU only. Ollama's experimental Vulkan support could use an Intel or AMD integrated GPU, but it needs access to the host's `/dev/dri` devices, which this app doesn't have. That may become an option later, if the CPU turns out to be too slow.

## Troubleshooting

- **Home Assistant can't connect.** Check that the app is running and that the URL starts with `http://` (not `https://`), uses the hostname from the app's Info page and ends with `:11434`.
- **The first answer is slow.** The model loads into memory first. It stays loaded for the keep alive time, so answers that follow soon after are faster.
- **"ollama cloud is disabled".** The model is a cloud model. Choose a local one from Ollama's library.
- **The app runs out of memory or restarts.** Choose a smaller model or a smaller context window.
- **Logs.** The app's **Log** tab shows Ollama's server log.

## License

MIT. This app isn't affiliated with or endorsed by Ollama. Ollama itself is MIT licensed. The icon and logo are original artwork made for this repository.
