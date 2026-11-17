# Changelog

## 1.0.1

- Ollama's official image is now pinned by digest as well as by version (still 0.40.16). Your host builds exactly the image that was tested, even if the tag is pushed again.
- Each Ollama update pins the digest of the image it was test-built from.

## 1.0.0

- First release, built from Ollama 0.40.16 ([release notes](https://github.com/ollama/ollama/releases/tag/v0.40.16)).
- Ollama's cloud is off (`OLLAMA_NO_CLOUD=1`): no cloud models and no web search.
- Runs at low CPU priority, keeps at most one model in memory and unloads it after 5 idle minutes.
- Opens no ports on your network, has no folder mappings and no access to Home Assistant's or Supervisor's API.
- Models are left out of backups.
