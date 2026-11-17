# Home Assistant apps

[![Build](https://github.com/fratei/ha-apps/actions/workflows/build.yaml/badge.svg)](https://github.com/fratei/ha-apps/actions/workflows/build.yaml)

Apps (formerly called add-ons) for Home Assistant.

| App | Description |
| --- | --- |
| [Ollama](ollama/) | A minimal, locked-down local AI server, built from Ollama's official image, with Ollama's cloud turned off. |

## Installation

[![Open your Home Assistant instance and show the add app repository dialog with this repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Ffratei%2Fha-apps)

Or add the repository by hand:

1. In Home Assistant, go to **Settings** > **Apps** > **App store**.
2. Select ⋮ in the top right > **Repositories**.
3. Add `https://github.com/fratei/ha-apps`, select **Add**, then **Close**.
4. The apps appear in the store. If they don't, select ⋮ > **Check for updates**.

Apps need Home Assistant OS.

## Updates

A weekly GitHub Actions job looks for a new stable Ollama release. It test-builds the release for amd64 and aarch64, then raises the pinned version and the app version, and Home Assistant offers the update.

## License

[MIT](LICENSE)
