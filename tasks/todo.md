# KomenoTV Auto tasks

- [x] Build and verify an independently signed, renamed 2.0.2 universal APK without car/browser behavior changes.
- [x] Additive Pixel 6a install; verify package identity and existing Fermata coexistence.
- [x] Additive Pixel 8 install via existing Shizuku/KingInstaller; verify com.android.vending installer metadata, preserve original Fermata, set port 7100, and confirm diagnostic launch/video/manual fullscreen in touchless DHU.
- [x] Phone video and parked 800x480 touchless DHU smoke: Developer app mode -> All car apps -> KomenoTV Auto; match the prototype CarService, play port 7100, and manually enter app fullscreen.
- [x] Compare Pixel 6a installer paths without deleting data. Its KingInstaller update changes the source to Google Package Installer; ordinary launcher still exposes only the prototype media service after Android Auto restart.
- [ ] Resolve why the independent CarService is absent from the ordinary launcher; diagnostic launch alone does not prove normal listing.
- [ ] User checkpoint: Pixel 8 prototype launches and shows port 7100 in the actual CX-5 via All car apps; the Pixel 6a prototype remains installed too.
- [ ] Then implement fixed KomenoTV startup and automatic app fullscreen; validate launch/return and preserve commander controls.
- [ ] User checkpoint: vehicle video/audio, reconnection and phone restart.
