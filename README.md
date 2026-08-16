# WAF4-Builder

WAF4-Builder is the official builder utility for the **WAF4** tool. It provides both **CLI** and **GUI** interfaces for generating custom payloads, updater scripts, and other injectable components required by WAF4.

The builder allows you to configure deployment-related values and generate the required build files without manually modifying the source files.

> **Latest version:** Check the repository's [**Releases**](https://github.com/PowerPizza/WAF4-Builder-Public/releases) section to download the latest version of WAF4-Builder.

# How to Use

1. Download the latest WAF4-Builder release.
2. Extract the downloaded ZIP archive.
3. Open the extracted directory.
4. Choose either the CLI or GUI interface described below.

## 1. Using the CLI

Run:

```text
waf4-builder-cli.cmd
```

The CLI will guide you through a series of prompts. Enter the requested values carefully and verify each value before continuing.

## 2. Using the GUI

Run:

```text
waf4-builder-gui.exe
```

Fill in the required fields and click **Execute Builder** to create the build.

> **Note:** The available prompts, options, and behavior of the CLI/GUI may change between WAF4-Builder versions. Always check the **release notes** for the version you are using.

# Configuration Reference

Whether you use the CLI or GUI, WAF4-Builder will ask you to provide several configuration values before the build process begins.

The following sections explain each configuration option and when it should be used.

## `PAYLOAD_HOST_URL`

The generated payload must be hosted at a location that the WAF4 software can access when downloading or updating the payload.

Set this value to the **direct, publicly accessible URL** of the hosted payload file.

## `UPDATOR_HOST_URL`

The updater script must also be hosted at a publicly accessible location so that WAF4 can download it when required.

Set this value to the **direct URL** of the hosted updater file.

## `USE_REDIRECTOR`
This options determine how WAF4 locates the admin panel.

### Using a fixed admin panel URL
If you are certain that the admin panel will always remain at the same URL, or you are using your custom domain linked with your admin panel then set USE_REDIRECTOR to `false` or `no`.  

Then provide the admin panel URL through `DIRECT_SERVER_URL`. Make sure you are providing permanent URL of admin panel.

> **Important:** If `USE_REDIRECTOR` is disabled and the admin panel URL changes later, existing targets configured with the old URL will no longer be able to locate the admin panel.

### Using a redirector
If the admin panel URL may change in the future, it is recommended to enable the redirector by setting USE_REDIRECTOR to `true` or `yes`.

When enabled, `DIRECT_SERVER_URL` is not used as the primary server location. Instead, WAF4 retrieves the current admin panel URL from the redirector configuration described below.

## `REDIRECTOR_FILE_HOST_URL`
This value is required when USE_REDIRECTOR is enabled.

Set it to the **permanent, publicly accessible URL** of a `servers.json` file.

The file must contain the admin panel URL in the following format:

```json
{
  "url": "<URL_TO_ADMIN_PANEL>"
}
```

The main advantage of using a redirector is that the admin panel URL can be changed without loosing all existing targets.

For example, if the admin panel is moved from:

```text
https://old.example.com/admin
```

to:

```text
https://new.example.com/admin
```

you only need to update `servers.json`:

```json
{
  "url": "https://new.example.com/admin"
}
```

As long as the `REDIRECTOR_FILE_HOST_URL` itself remains accessible at the same URL, existing targets can obtain the updated admin panel location.

> **Important:** The redirector file should be hosted at a stable URL. Changing the URL of the `servers.json` file require file editing without change in it's access URL.

# Output
After the build completes successfully, all generated files will be placed in the:

```text
output/
```

directory located within the WAF4-Builder directory.

The exact files generated may vary depending on the selected configuration and the WAF4-Builder version.

See the [Deployment Documentation](https://github.com/PowerPizza/Wireless-ALT-F4-public/tree/master#wireless-alt-f4) for information about deploying and using the generated files.
