# Canvas — Lab Eksperimen ProofForge 3D

> **Lab eksperimen ringan (single-file HTML) untuk menguji konsep visual & interaksi 3D sebelum diadopsi ke sumber utama [`proofforge-3d`](https://github.com/Niumination/proofforge-3d).**
> Kompetisi: **Gemini 4 NYC Hackathon** (Cerebral Valley × Google DeepMind — Nov 21, 2026)

[![Relasi](https://img.shields.io/badge/Relasi-Spin--off%20Eksperimen%20dari%20proofforge--3d-8E24AA)](https://github.com/Niumination/proofforge-3d)
[![Mode](https://img.shields.io/badge/Mode-Non--AI%20%2F%20Statis%20HTML-34A853)](#)
[![Status](https://img.shields.io/badge/Status-Aktif%20Eksperimen-FBBC05)](#)

---

## 1. Kenapa repo ini ada

[`proofforge-3d`](https://github.com/Niumination/proofforge-3d) adalah **sumber utama** (main source) proyek kompetisi: *Self-Verifying Systems Compiler & 3D Visual Counterexample Engine* — loop tertutup `co-generation (Gemini 4 Argon) → pembuktian Z3 → replay kegagalan 3D → surgical patch`.

Repo ini adalah **pengembangan eksperimental** yang sengaja terpisah:

| | `proofforge-3d` (sumber utama) | `canvas` (lab ini) |
| :--- | :--- | :--- |
| Peran | Arsitektur, kontrak data, rencana Phase 0–3 | Uji coba cepat konsep 3D & interaksi |
| Bentuk | Dokumen blueprint → monorepo penuh | **Satu file HTML per eksperimen**, tanpa build step |
| AI | Inti (Gemini 4 Argon, Managed Agents) | **Tidak ada** — murni rendering & simulasi deterministik |
| Kepatuhan hackathon | Phase 3 baru boleh hari-H (21 Nov) | **Aman untuk pra-event**: eksperimen non-AI termasuk kategori "kerangka UI non-AI umum (viewport Three.js)" yang diizinkan Phase 0–2 |

Prinsip: **apa yang terbukti enak dipakai di lab ini, diangkat ke sumber utama**; apa yang gagal, dicatat lalu dibuang tanpa menyentuh `proofforge-3d`.

## 2. Isi repo

```
canvas/
├── README.md                  ← dokumen ini
├── experiments/               ← satu file HTML = satu eksperimen, buka langsung di browser
│   └── kabupaten-aceh-tengah-3d-google-earth.html
└── pendamping/                ← snapshot dokumen dari sumber utama (BUKAN pengganti)
    ├── KONTRAK-DATA.md        ← kontrak JSON/SSE §4 ARCHITECTURE.md (dipakai eksperimen)
    ├── Z3-SCENARIO-MATH-SPECS.md
    └── BRAINSTORMING-AND-DEMO-SCENARIOS.md
```

> **Aturan pendamping**: file di `pendamping/` adalah **snapshot** — sumber kebenaran tetap [`proofforge-3d`](https://github.com/Niumination/proofforge-3d). Snapshot diperbarui hanya saat kontrak/skenario di sumber berubah.

## 3. Katalog eksperimen

### E-000 — `kabupaten-aceh-tengah-3d-google-earth.html`
Visualisasi 3D wilayah Kabupaten Aceh Tengah (gaya Google Earth, satu file HTML).
**Yang diuji**: performa kamera orbit/pastel terrain di file tunggal, kontrol navigasi, keterbacaan label lokasi.
**Belinya untuk ProofForge**: fondasi *3D State-Space Viewer* — kamera, lighting, dan ergonomi navigasi yang sama dipakai saat menggambar entitas (`entities[]`) & *safety sphere* (`constraints_overlay[]`).

### (Berikutnya) E-001 — Timeline Scrubber deterministik
Memutar `SceneTimelineFrame[]` (lihat [`pendamping/KONTRAK-DATA.md`](./pendamping/KONTRAK-DATA.md)) dengan time scrubber `[0..T]`, frame merah pada `is_violation: true`, toggle before/after. **Sesuai kontrak §4.3 — nol perubahan skema.**

### (Berikutnya) E-002 — Drone Swarm toy sim (Skenario A)
Simulasi linier `drone_swarm` dari [`pendamping/Z3-SCENARIO-MATH-SPECS.md`](./pendamping/Z3-SCENARIO-MATH-SPECS.md) versi deterministik (rumus posisi & counterexample demo yang sudah dispesifikasikan), dengan overlay `safety_sphere` yang `breached` saat TABRAK.

### (Berikutnya) E-003 — Fake SSE playback
Memutar ulang urutan `SSEStreamEvent` (`STAGE_CHANGED` → `VERIFICATION_RESULT` → `PATCH_DIFF`) sebagai "rekaman" untuk menyambungkan UI cockpit tanpa backend.

## 4. Yang diambil dari sumber utama (dan alasannya)

| Diambil dari `proofforge-3d` | Dipakai untuk | Sejalan? |
| :--- | :--- | :--- |
| Kontrak `SceneTimelineFrame`, `VerificationResult`, `ForgeRunRequest`, `SSEStreamEvent` (`pendamping/KONTRAK-DATA.md`) | Standar data semua eksperimen viewer, agar porting ke R3F backend nanti mulus | ✅ |
| `Z3-SCENARIO-MATH-SPECS.md` (rumus linier, domain variabel, counterexample demo, rumus fix) | Simulasi deterministik E-002 tanpa perlu Z3 di browser | ✅ |
| `BRAINSTORMING-AND-DEMO-SCENARIOS.md` (kriteria demo, wow-factor, matriks fallback) | Filter ide eksperimen: hanya yang memperkuat 3 skenario flagship | ✅ |
| Stack 3D (Three.js) | Eksperimen memakai Three.js via CDN — keluarga yang sama dengan `@react-three/fiber` di sumber utama | ✅ |
| Model keamanan (Protected Hooks, Credential Proxy) | **Tidak diambil** — milik Phase 3, di luar jangkauan lab non-AI | ❌ sengaja |

## 5. Lihat juga

- [`Niumination/proofforge-3d`](https://github.com/Niumination/proofforge-3d) — sumber utama & rencana induk
- [`Niumination-VpsBaik/cyber-city-ai-gov-sim.html`](https://github.com/Niumination/Niumination-VpsBaik) — eksperimen 3D lain di luar repo ini (Cyber City AI Gov-Sim)
