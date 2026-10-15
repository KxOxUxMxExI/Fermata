# KomenoTV Auto tasks

- [x] Build and verify an independently signed, renamed 2.0.2 universal APK without car/browser behavior changes.
- [x] Additive Pixel 6a install; verify package identity and existing Fermata coexistence.
- [x] Run phone and parked 800x480 touchless DHU smoke: phone video passed; DHU exposes only the prototype media service, so car Web video is not reached.
- [x] Compare installer paths without deleting data; repeat after Android Auto process restart. KingInstaller update changes the source to Google Package Installer but does not expose the prototype CarService.
- [ ] Resolve independent prototype CarService recognition before actual-car testing.
- [ ] User checkpoint: independent prototype is listed and launches in the actual CX-5.
- [ ] Then implement fixed KomenoTV startup and automatic app fullscreen; validate launch/return and preserve commander controls.
- [ ] User checkpoint: vehicle video/audio, reconnection and phone restart.
