# KONTRAK-DATA.md — Kontrak Data ProofForge 3D (Snapshot Pendamping)

> **Sumber kebenaran**: [`ARCHITECTURE.md` §4 di `Niumination/proofforge-3d`](https://github.com/Niumination/proofforge-3d/blob/main/ARCHITECTURE.md).
> Snapshot ini dibuat `2026-11-17` untuk kebutuhan lab eksperimen `canvas`. Jika berubah di sumber utama, perbarui di sini.

Eksperimen di repo ini memakai kontrak yang sama persis agar hasil lab bisa "ditancapkan" ke ProofForge 3D tanpa penulisan ulang.

---

## 4.1 `ForgeRunRequest`
```json
{
  "scenario_id": "drone-swarm-v1",
  "mode": "live_gemini | deterministic_fallback",
  "custom_spec": "Optional natural language override",
  "inject_initial_bug": true,
  "max_iterations": 5,
  "enable_adversarial_hook_test": false
}
```

## 4.2 `VerificationResult`
```json
{
  "iteration": 1,
  "status": "SAT_VIOLATION | UNSAT_PROVEN | ERROR",
  "invariant_hash_valid": true,
  "violated_invariant": "min_pairwise_distance >= 1.50m",
  "counterexample_inputs": {
    "drone_a_speed": 14.2,
    "drone_b_speed": 13.8,
    "comm_delay_ms": 240
  },
  "violated_tick": 42,
  "violated_time_sec": 4.2,
  "solver_duration_ms": 184
}
```

## 4.3 `SceneTimelineFrame` (Untuk Three.js / R3F)
```json
{
  "tick": 42,
  "time_sec": 4.2,
  "is_violation": true,
  "entities": [
    {
      "id": "drone-alpha",
      "kind": "quadrotor",
      "position": [1.24, 3.0, -0.45],
      "rotation_euler": [0.12, 0.0, -0.25],
      "velocity": [14.2, 0.0, 0.0],
      "status": "COLLISION"
    }
  ],
  "constraints_overlay": [
    {
      "type": "safety_sphere",
      "center": [1.24, 3.0, -0.45],
      "radius": 1.5,
      "breached": true
    }
  ]
}
```

## 4.4 `SSEStreamEvent` (Real-time Telemetry ke Frontend)
Event types yang mengalir lewat `GET /api/forge/stream/{run_id}`:
* `STAGE_CHANGED` (`LOCK_INVARIANTS`, `SYNTHESIZE`, `VERIFY`, `PATCH`, `COMPLETE`)
* `AGENT_LOG` (streaming reasoning summary & tool execution dari Managed Agent)
* `HOOK_AUDIT` (`ALLOW` / `BLOCKED` beserta alasan & hash check)
* `PROXY_AUDIT` (`domain`, `credential_ref`, `redacted: true`, `status_code`)
* `VERIFICATION_RESULT` (payload `VerificationResult` + `SceneTimelineFrame[]`)
* `PATCH_DIFF` (unified diff + penjelasan perubahan matematis)
