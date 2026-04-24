# Thalion Systems

Hardened open source firmware for FPV drones deployed in contested RF environments.

The entire open source FPV stack was built by hobbyists with zero adversarial threat modeling. Thalion maintains security-hardened forks of each critical layer, from control link to flight controller to video transport. Every fork stays compatible with upstream hardware and tooling. Operators flash via the same tools they already use.

## Projects

| Repo | What it does | Status |
|:-----|:-------------|:-------|
| **[MurmurLRS](https://github.com/thalion-systems/murmur-lrs)** | Encrypted control link. ASCON-128 AEAD packet encryption + CSPRNG frequency hopping. Fork of [ExpressLRS](https://github.com/ExpressLRS/ExpressLRS). | Active |
| **HushWFB** | Hardened video transport. Per-drone keys, channel hopping, forward secrecy. Fork of [wfb-ng](https://github.com/svpcom/wfb-ng). | Planned |
| **WardFlight** | Hardened flight controller. MSP authentication, blackbox encryption, WiFi beacon suppression. Fork of [Betaflight](https://github.com/betaflight/betaflight). | Planned |
| **GhostNav** | Hardened navigation. Encrypted missions, telemetry encryption, anti-GPS-spoofing. Fork of [iNav](https://github.com/iNavFlight/inav). | Planned |
| **SealTX** | Hardened transmitter firmware. Encrypted SD storage, device lock, panic wipe. Fork of [EdgeTX](https://github.com/EdgeTX/edgetx). | Planned |
| **Anvil** | Key management CLI. Generate squad keys, provision devices, air-gapped operation. | Planned |

## Why this exists

Stock open source FPV firmware has no encryption, no authentication, and predictable RF patterns. In contested environments, an adversary with a $300 SDR can:

- **Strip the hop sequence** in seconds and apply targeted narrowband jamming
- **Read every telemetry packet** including GPS coordinates, home position, and mission waypoints
- **Inject commands** via unauthenticated serial protocols
- **Identify and locate operators** from WiFi beacons and RF fingerprints
- **Extract full mission intelligence** from captured hardware via unencrypted logs

Thalion fixes each of these, layer by layer, while keeping the firmware flashable by non-developers using standard tools like ELRS Configurator and Betaflight Configurator.

## Design principles

- **Encrypt everything.** Packets, logs, stored data, telemetry. If it touches RF or removable storage, it's encrypted.
- **No security through obscurity.** All code is open source. Security comes from proven cryptographic primitives (ASCON-128, Curve25519, ChaCha20-Poly1305), not secret algorithms.
- **Operator-friendly.** If an operator can flash stock firmware, they can flash Thalion firmware. Same hardware, same tools, same workflow.
- **Upstream compatible.** Each fork tracks its upstream project. Upstream bug fixes and features merge in. Thalion code lives behind compile guards so the stock build path never breaks.

## Getting started

### MurmurLRS (available now)

MurmurLRS adds ASCON-128 AEAD encryption to every RC packet between transmitter and receiver. It's a drop-in replacement for stock ExpressLRS firmware.

1. Clone the repo
2. Open in ELRS Configurator (local tab, point at `src/` folder)
3. Set a binding phrase (this auto-enables encryption)
4. Flash both TX and RX
5. Both sides must run MurmurLRS. Mixed firmware will not link.

## Contributing

We need people who can flash hardware and break things. See [CONTRIBUTING.md](https://github.com/thalion-systems/murmur-lrs/blob/master/CONTRIBUTING.md) in each repo.

The most valuable contribution right now: flash MurmurLRS on your hardware and report what works and what doesn't.

## Contact

Open an issue in the relevant repo, or start a discussion on the org Discussions tab.

---

*Thalion: Sindarin for "strong, dauntless." The title of Hurin Thalion, who never broke.*
