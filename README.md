A lightweight **public registry** for the nexuslua ecosystem.  
It lists URLs to each plugin’s own `nexuslua_plugin.toml` file, enabling tools and host applications (e.g. the [nexuslua executable](https://github.com/acrion/nexuslua) and [acrionphoto](https://github.com/acrionphoto) to discover, display, and install plugins.

> This repository does **not** host plugin binaries. It is a catalogue of **metadata URLs** only.  
> Plugin authors retain full control over versioning and releases in their own repositories.

---

## How it works

- The registry is a single file: **`plugins.toml`**.
- Each entry points to a plugin’s **`nexuslua_plugin.toml`** (typically a raw URL).
- Host applications fetch `plugins.toml`, follow each URL, and read the plugin metadata from the referenced TOML.

**Example (`plugins.toml`):**
```toml
# Central registry of all official and community nexuslua plugins.

[[plugin]]
# acrion image tools – basic image I/O and processing
url = "https://raw.githubusercontent.com/acrion/image-tools/main/nexuslua_plugin.toml"
````

---

## What makes a valid nexuslua plugin?

A plugin is simply a directory containing **two required files**:

```
<Plugin Root>/
  main.lua                 # message handlers (addmessage(...))
  nexuslua_plugin.toml     # machine-readable metadata
```

The TOML must include at least:

* `displayName` (string), `version` (string), `isFreeware` (bool), `description` (string), and platform download URLs such as `urlDownloadLinux`, `urlDownloadWindows`, and `urlDownloadDarwin`.
  These URLs must point to a **zip archive of your plugin** **without** its `nexuslua_plugin.toml`. During installation, nexuslua places the TOML into the **unzipped** plugin root, next to `main.lua`.
* Optional but recommended: `urlHelp`, `urlLicense`
* Optional for commercial plugins: `urlPurchase`

Hosts use this metadata to render listings, present licensing actions, and - most importantly - download and install the plugin code (which consists of at least a `main.lua`, and optionally shared libraries imported by nexuslua).

---

## Adding your plugin

1. **Fork** this repository.
2. **Append** your entry to `plugins.toml`:

   ```toml
   [[plugin]]
   url = "https://raw.githubusercontent.com/<you>/<repo>/<tag-or-branch>/nexuslua_plugin.toml"
   ```
3. Open a **pull request** with a short description.

### Best practices

* Prefer a **tagged** URL (immutable) over a moving branch for reproducible installs.
* Keep `nexuslua_plugin.toml` accessible over **HTTPS** (e.g. GitHub Raw).
* Update the TOML **in your plugin repository** when you release a new version; no registry change is required unless the URL itself changes.
* Keep `displayName` stable to avoid duplicate listings.

---

## Trust & licensing

This registry is **curated but not audited**. Installing a plugin is at the user’s discretion.
Each plugin’s **licence** is defined by the plugin’s own repository and `urlLicense`.

---

## Related

* **nexuslua (engine & CLI):** [https://github.com/acrion/nexuslua](https://github.com/acrion/nexuslua)
* Example plugins and host applications are linked from their respective repositories.

