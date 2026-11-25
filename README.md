# volumio-plugins-sources-bookworm (volumio 4.xxx)

The repo for Volumio Bookworm plugins.

Bookworm version of Volumio required some adjustement for plugins due to new node and kernel version.
To learn more how to write a plugin for Volumio, [see](https://developers.volumio.com/plugins/submission-checklist)

## Volumio Bookworm 
This is the current [version](https://volumio.com/get-started/)
---

## Steps to create/modify a plugin
## 1. Fork the Repository
  - Click the Fork button (top-right corner) to create a copy of the repository under your own GitHub account.
## 2. Clone your forked repository
  - From a running Volumio Bookworm system, clone the Bookworm dedicated plugin repo:
```
git clone https://github.com/YOUR-USERNAME/REPOSITORY-NAME.git --depth=1
```
## 3. Create a new branch
```
git checkout -b your-branch-name
```
## 4. Create or copy your plugin folder and cd to it.

In package.json make changes as shown in the example below:

```
                                                                               
{
        "name": "Systeminfo",
        "version": "3.0.7", <------------------------------------PLUGIN VERSION
        "description": "Information about your system",
        "main": "index.js",
        "scripts": {
                "test": "echo \"Error: no test specified\" && exit 1"
        },
        "author": "Balbuze",
        "license": "",
        "repository": "https://github.com/balbuze/volumio-plugins-sources",
        "volumio_info": {
                "prettyName": "System information",
                "plugin_type": "user_interface",
                "icon": "fa-info-circle",
                "architectures": [
                        "amd64", <--------------------------------only "amd64" or ""armhf"
                        "armhf"
                ],
                "os": [
                        "bookworm" <--------------------------------OS VERSION
                ],
                "details": "Gives information about your system",
                "changelog": "bookworm version"
        },
        "engines": {
                "node": ">=20", <-------------------------------NODE VERSION >=20
                "volumio": ">=4" <---------------------VOLUMIO VERSION >=4
        },
        "dependencies": { 
                "fs-extra": "*",
                "kew": "*",                  <---------ADJUST DEPENDENCIES VERSION IF NEEDED
                "systeminformation": "*",
                "v-conf": "*"
        }
}
```

## 5. To install the plugin on your system, use :
```
volumio plugin install
```
Test carefully your plugin!
If ok, uninstall the plugin (important to check it works!)
Remove node_modules
```
rm -Rf node_modules
```
## 6. Check your plugin

Volumio checks every plugin with the [Volumio plugin SDK](https://github.com/VolumioTeam/volumio-plugin-sdk). Run the same checks before you send your pull request, from the root of your clone, on your computer or on a Volumio device:

```
npx --package=@volumio/plugin-sdk volumio-plugin check your_plugin_folder
```

Each error comes with a `fix:` line, and an error blocks the pull request. Warnings and `todo` lines do not block. The most common errors:

- `README is missing`: add a `README.md` that says what the plugin does and how to set it up.
- `changelog must be string`: in `package.json`, `volumio_info.changelog` is one string, not a list.

If you changed the dependencies in `package.json`, run `npm install` in your plugin folder and commit the `package-lock.json` it writes. The build installs with `npm ci`, which fails when the lockfile does not match `package.json`.

The pull request also runs ShellCheck on your scripts and looks for secrets in your code. `check` runs these two only when `shellcheck` and `gitleaks` are installed, which they are not on a Volumio device.

## 7. Send a PR to Github and submit from a BOOKWORM DEVICE!

For Github
```
git add *
git commit -m 'pluginname - change description'
git push origin your-branch-name
```

To submit
```
volumio plugin submit
```

Your plugin is now in beta state, available in the store when "plugin test mode" is enabled.

It will be released as stable once checked by volumio team.

## 8. Update your plugin

If you want to update your plugin (to fix an issue, add a feature or EVEN minor typo correction...) you MUST change the version number in package.json, re-submit the plugin and send a new PULL REQUEST. Without new version number, the new plugin will be NOT available in the store!
