# Offline builds and backups

Goal: be able to keep building Tend's Photo even if upstream repositories or crates.io disappear.

1. **Own forks.** The code lives in `VoltrunGames/photocraft`; its git dependency `craft-libs`
   points at `VoltrunGames/craft-libs` (same pinned commits as upstream). Optional `craft-fonts`
   is forked as `VoltrunGames/craft-fonts`.
2. **Vendored dependencies.** Run the *Vendor dependencies* workflow (Actions tab). It publishes
   `tends-photo-vendor-<date>.tar.zst` as a release. To build offline:
   ```
   tar --zstd -xf tends-photo-vendor-<date>.tar.zst
   mkdir -p .cargo && cat vendor-config.toml >> .cargo/config.toml
   cargo build --release --offline -p photocraft
   ```
   Re-run the workflow whenever `Cargo.lock` changes.
3. **Backup outside GitHub.** On your own machine, for every repo:
   ```
   git clone --mirror https://github.com/VoltrunGames/photocraft
   git clone --mirror https://github.com/VoltrunGames/craft-libs
   ```
   Refresh later with `git remote update` inside each mirror. Keep the mirrors and the vendor
   archive on a drive and, if you like, push the mirrors to a second host (Codeberg, GitLab).
