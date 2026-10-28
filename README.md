# XLibre Debian packages

Builds [XLibre](https://xlibre.net) (the X.Org X server community fork) from
source into installable Debian `.deb` packages.

Produces:

- `xlibre-server` / `xlibre-server-dev` — from the `xserver/` checkout
- `xlibre-server-xorg-video-amdgpu` — from the `xf86-video-amdgpu/` checkout
- `xlibre-server-xorg-input-libinput` — from the `xf86-input-libinput/` checkout
- `xlibre-server-xorg-video-intel` — from the `xf86-video-intel/` checkout

The driver packages are built against the server dev package (they install it
from a shared named volume and compile against its `xorg-server.pc` pkg-config
files).

## Usage

1. Place the three upstream checkouts next to the Dockerfiles at the tags the
   compose file expects:
   - `xserver` @ `xlibre-xserver-25.1.10`
   - `xf86-video-amdgpu` @ `xlibre-xf86-video-amdgpu-25.1.1`
   - `xf86-input-libinput` @ `xlibre-xf86-input-libinput-25.0.1`

   The checkouts are listed in `.gitignore` and must never be committed.
2. If you check out different tags, update the version build args
   (`XLIBRE_VERSION`, `AMDGPU_VERSION`, `LIBINPUT_DRV_VERSION`) in
   `docker-compose.yaml` — and the `xserver-build` service's `environment`
   block, which mirrors `XLIBRE_VERSION`/`DEB_REVISION` for the export check.
3. Run `docker compose up --build` from this directory.
4. Collect the results on the host:
   - `./output/xlibre-server_25.1.10-1_amd64.deb`
   - `./output/xlibre-server-dev_25.1.10-1_amd64.deb`
   - `./output/drivers/xlibre-server-xorg-video-amdgpu_25.1.1-1_amd64.deb`
   - `./output/drivers/xlibre-server-xorg-input-libinput_25.0.1-1_amd64.deb`
   - `./output/drivers/xlibre-server-xorg-video-intel_25.0.2-1_amd64.deb`

   (The two server debs also land in the shared `xlibre-deb-repo` named volume,
   which the driver builds read from. Stale debs are cleared from that volume
   on every run, so re-running with a different version never mixes packages.)
5. Optionally override the server meson options from the host:
   `MESON_OPTIONS="-Dxkb_output_dir=/var/lib/xkb -Ddri3=true -Dglamor=true" docker compose up --build`
   (that is already the default baked into the compose file).
