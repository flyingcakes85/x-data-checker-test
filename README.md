# x-data-checker manifests
Repo for housing manifest for testing x-data-checker on KDE apps

## One time setup

**Prerequisite**: Have Flatpak support on your system

To install Flatpak External Data Checker, run following command
```sh
flatpak install --from https://dl.flathub.org/repo/appstream/org.flathub.flatpak-external-data-checker.flatpakref
```

## Try out on this repo

### Clone this repo

```sh
git clone https://github.com/flyingcakes85/x-data-checker-test
cd x-data-checker-test/kdiff3
```

### Run the checker

```sh
flatpak run org.flathub.flatpak-external-data-checker org.kde.kdiff3.json
```

Expected output :

```
INFO    src.manifest: Loading module from boost.json
INFO    src.manifest: Checking 2 external data items
INFO    src.manifest: Skipped check [1/2] archive boost_1_78_0.tar.bz2 (from boost.json)
INFO    src.manifest: Started check [2/2] archive kdiff3-1.9.2.tar.xz (from org.kde.kdiff3.json)
INFO    src.lib.externaldata: Source kdiff3-1.9.2.tar.xz: got new version 1.9.4
INFO    src.manifest: Finished check [2/2] archive kdiff3-1.9.2.tar.xz (from org.kde.kdiff3.json)
CHANGE SOON: kdiff3-1.9.2.tar.xz
 Has a new version:
  URL:       https://download.kde.org/stable/kdiff3/kdiff3-1.9.4.tar.xz
  MD5:       138dde47d87801f6d9404754818f952b
  SHA1:      adfa312caa4f42ae2cd7eb9b6c87ae2f14cd7bfd
  SHA256:    a130712ceef074df691426909fc4a332b66f4c3f1490a548ab5bfb46316c385a
  SHA512:    76555eb9253c4cb96d32de01126a4547adb3424997034f0d9aed50cc4df87cb15c038a12f8620a491a968b47a3ea5eafcb0f53978aca4300ca319677bb2b9e63
  Size:      1057404
  Version:   1.9.4
  Timestamp: 2021-11-21 23:51:17

INFO    src.main: Check finished with 0 error(s)
```

## To run checker and update manifest in place

I'm giving write permission to current directory. Without the permission, editing will fail, as this tool has only `host:ro` permission.

```sh
flatpak run --filesystem=`pwd` org.flathub.flatpak-external-data-checker org.kde.kdiff3.json --edit-only
```


## Extra behaviour

See help output

```
usage: flatpak-external-data-checker [-h] [-v] [--update] [--commit-only] [--edit-only]
                                     [--check-outdated]
                                     [--filter-type {extra-data,file,archive,git}]
                                     [--always-fork | --never-fork] [--unsafe]
                                     manifest

positional arguments:
  manifest              Flatpak manifest to check

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         Print debug messages
  --update              Update manifest(s) to refer to new versions of external data -
                        also open PRs for changes unless --commit-only is specified
  --commit-only         Do not open PRs for updates, only commit changes to external data
                        (implies --update)
  --edit-only           Do not commit changes, only update files (implies --update)
  --check-outdated      Exit with non-zero status if outdated sources were found and not
                        updated
  --filter-type {extra-data,file,archive,git}
                        Only check external data of the given type
  --unsafe              Enable unsafe features; use only with manifests from trusted
                        sources

control forking behaviour:
  By default, flatpak-external-data-checker pushes directly to the GitHub repo if the
  GitHub token has permission to do so, and creates a fork if not.

  --always-fork         Always push to a fork, even if the user has write access to the
                        upstream repo
  --never-fork          Never push to a fork, even if this means failing to push to the
                        upstream repo
```
