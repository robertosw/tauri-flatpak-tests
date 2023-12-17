# tauri-flatpak-tests

# How to run this
Flatpak bundling is quite easy for tauri v2. If you want to run this in your own project, copy the [Dockerfile](/Dockerfile), [docker-compose.yml](/docker-compose.yml) and [flatpak manifest](/flatpak/org.tauri.flatpak.yml) into your project.

- Run the container
  - If there is no tauri project with tauri v2, run `cargo create-tauri-app -t vanilla -m cargo -y --alpha`
  - Configure your [tauri.conf.json](/tauri-app/src-tauri/tauri.conf.json) to build a .deb
  - `cargo tauri build`
  - Modify the flatpak manifest, to point to your .deb file
  - in `flatpak` directory: `flatpak-builder --repo=repo --force-clean build-dir org.tauri.flatpak.yml`
- On host
  - in `flatpak` directory: `flatpak --user remote-add --no-gpg-verify repo-tauri repo`
  - in `flatpak` directory: `flatpak -y --user install repo-tauri org.tauri.flatpak`
  - `flatpak run org.tauri.flatpak`
  - Updates: `flatpak -y --user update org.tauri.flatpak`

# Notes for tauri v1

- cargo project init to use only rust and js: `cargo create-tauri-app -t vanilla -m cargo -y --alpha`
- This assumes that the tauri app is built into a .deb file
- For tauri v1:
  - The main problem is matching the build environment to the environment of org.gnome.Platform//45
  - org.gnome.Platform//44 already contains webkit2gtk-4.1 so for tauri 1.x an older version is required
    - //43 also has 4.1, //42 has 4.0
    - up to date tauri 1.x and gnome//42 runtime dont work together. Tauri required GLIBC 2.34 and libwebkit2gtk requires GLIBS 2.35. Both are not present in the runtime
  - Differences between debian 12 and org.gnome.Platform//42
    - `libwebkit2gtk-4.0.so.37.67.6`  <>  `libwebkit2gtk-4.0.so.37.57.8`
    - TODO compare diffs between `tauri-app-ldd.txt` and `gnome42-libs.txt` and sort according to the severity of the version differences (probably with faster with a rust program)
- `flatpak run --command=bash org.gnome.Platform//42`
- Building webkit from source?
