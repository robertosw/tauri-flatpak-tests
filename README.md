# tauri-flatpak-tests

- cargo project init to use only rust and js: `cargo create-tauri-app -t vanilla -m cargo -y`
- This assumes that the tauri app is built into a .deb file
- The main problem is matching the build environment to the environment of org.gnome.Platform//45
  - maybe nix can help with that to install one specific version of a library
- org.gnome.Platform//44 already contains webkit2gtk-4.1 so for tauri 1.x an older version is required
  - //43 also has 4.1, //42 has 4.0   
- Differences between debian 12 and org.gnome.Platform//42
  - `libwebkit2gtk-4.0.so.37.67.6`  <>  `libwebkit2gtk-4.0.so.37.57.8`

---

### Current State:
up to date tauri 1.x and gnome//42 runtime dont work together. Tauri required GLIBC 2.34 and libwebkit2gtk requires GLIBS 2.35. Both are not present in the runtime

---

##### my TODO 
anhand `tauri-app-ldd.txt` und `gnome42-libs.txt` die Versions Differenzen herausfinden + schauen welche libs bei ldd drin stehen welche bei gnome fehlen.
- Das vielleicht gleich am besten als kleines Rust program
- Ideal wäre auch noch die Versions unterschiede zu sortieren. Also nach unterschieden in major.minor.patch sortieren (alle mit major unterschied zsm, alle mit minor unterschied zsm, etc)

`flatpak run --command=bash org.gnome.Platform//42`
