---
applyTo: 'src/**'
---

# Goals

This is a CLI application to download and sanity test VS Code installations on Windows, Linux and macOS for x64 and ARM. 

As a user I want to run `vscode-sanity --commit <commit hash>` to test a specific published build of VS Code that corresponds to that commit.

When I run `vscode-sanity --help` I want to see help text with a list of available options.

Any operation that is done should be reflected by output to the terminal. Long running operations show progress until finished.

# Folders to use for storing artefacts and data

The OS temp dir joined is the root for all things that are stored on disk during invocation. We refer to it as `$tempdir` going forward.

The commit hash that the user provided from the command line is refered to as `$commit` going forward.

Use `$tempdir/vscode-sanity/.builds/$commit` for storing build artifacts for the provided commit hash. Before downloading a new build, the CLI should check if the build artifacts already exist in this directory.

Use `$tempdir/vscode-sanity/.data/data` for storing user data and `$tempdir/vscode-sanity/.data/extensions` for storing extensions when starting VS Code using the `--user-data-dir` and `--extensions-dir` command line flags.

When I run `vscode-sanity --reset`, the contents of `$tempdir/vscode-sanity/.builds` and `$tempdir/vscode-sanity/.data` are to be deleted.

# Downloading builds

VS Code builds are stored on https://update.code.visualstudio.com. To download a build for a specific commit and OS and architecture, the CLI should use the following endpoint:

```
GET https://update.code.visualstudio.com/api/versions/commit:${commit}/${platform}/stable
```

The `${platform}` should be replaced with the appropriate value based on the user's OS and architecture.

For example, if the user is on macOS with ARM architecture, the URL would look like this:

```
GET https://update.code.visualstudio.com/api/versions/commit:dfaf44141ea9deb3b4096f7cd6d24e00c147a4b1/darwin-arm64/stable
```

The resulting JSON contains a `url` property that should be used to download the build. It is compressed and needs to be extracted before use.

# Running builds

Depending on the user's OS and architecture, the CLI should use the appropriate command to run the downloaded build. For example, on macOS with ARM architecture, the command might look like this:

```
/path/to/downloaded/vscode/Contents/Resources/app/bin/code --user-data-dir=$tempdir/vscode-sanity/.data/data --extensions-dir=$tempdir/vscode-sanity/.data/extensions --disable-updates --disable-telemetry
```

On Windows, the command might look like this:

```
C:\path\to\downloaded\code.exe --user-data-dir=%temp%\vscode-sanity\.data\data --extensions-dir=%temp%\vscode-sanity\.data\extensions
```