# KomenoTV Auto tasks

- [x] Build and verify an independently signed, renamed 2.0.2 universal APK without car/browser behavior changes.
- [x] Additive Pixel 6a install; verify package identity and existing Fermata coexistence.
- [x] Phone video and parked 800x480 touchless DHU smoke: Developer app mode -> All car apps -> KomenoTV Auto; match the prototype CarService, play port 7100, and manually enter app fullscreen.
- [x] Compare installer paths without deleting data. KingInstaller update changes the source to Google Package Installer; ordinary launcher still exposes only the prototype media service after Android Auto restart.
- [ ] Resolve why the independent CarService is absent from the ordinary launcher; diagnostic launch alone does not prove normal listing.
- [ ] User checkpoint: prototype launches in the actual CX-5 via All car apps or its ordinary launcher entry, if present.
- [ ] Then implement fixed KomenoTV startup and automatic app fullscreen; validate launch/return and preserve commander controls.
- [ ] User checkpoint: vehicle video/audio, reconnection and phone restart.
