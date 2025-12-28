1) proposal (objeto de intenção + plano imutável)
{
  "proposal_id": "prop_20251228_193355Z_ab12cd",
  "job_id": "job_20251228_193300Z_01",
  "project_id": "DBB",
  "created_at": "2025-12-28T19:33:55Z",
  "expires_at": "2025-12-28T20:03:55Z",

  "status": "proposed",
  "risk_level": "medium",
  "executor_type": "n8n_cli_agent",
  "deliverable_type": "patch",

  "target_paths": ["docs/canon/APPROVAL_POLICY.md"],
  "path_allowlist": ["docs/", "vault/", "snapshots/"],
  "command_allowlist": ["git", "python", "node"],

  "intent_summary": [
    "Atualizar policy de aprovação para v1.1",
    "Adicionar plan_hash + dry-run + preflight",
    "Registrar evidências no VAULT"
  ],

  "changes_preview": {
    "format": "unified_diff",
    "content_redacted": "*** REDACTED DIFF HERE ***",
    "redaction": { "applied": true, "suspicious": false, "findings_count": 0 }
  },

  "plan_canonical_json": {
    "base_revision": {
      "git_head": "abc1234",
      "target_paths_sha256": { "docs/canon/APPROVAL_POLICY.md": "..." }
    },
    "steps": [
      { "step_id": "s1", "action_type": "git_apply_check", "params": { "patch_ref": "vault://patches/p1.diff" } },
      { "step_id": "s2", "action_type": "apply_patch", "params": { "patch_ref": "vault://patches/p1.diff" } },
      { "step_id": "s3", "action_type": "git_status_check", "params": { "expected_git_state": "clean_or_expected" } }
    ],
    "success_criteria": [
      { "check_type": "git_status", "expected": "clean_or_expected" }
    ],
    "verification_plan": [
      { "check_id": "v1", "check_type": "git_apply_check" },
      { "check_id": "v2", "check_type": "git_diff_paths_only", "params": { "allowed_paths": ["docs/canon/APPROVAL_POLICY.md"] } }
    ]
  },

  "plan_hash": "sha256:....",

  "dry_run": {
    "performed": true,
    "status": "pass",
    "summary": "git apply --check passou; paths ok",
    "evidence_refs": ["vault://logs/dry_run_...jsonl"]
  },

  "constraints": {
    "require_ok": true,
    "approval_timeout_sec": 1800,
    "require_preflight": true,
    "require_redaction": true
  }
}


Obrigatórios (required):
proposal_id, job_id, project_id, created_at, status, risk_level, executor_type, deliverable_type, target_paths, plan_canonical_json, plan_hash, constraints.require_ok

2) approval (seu OK rastreável)
{
  "approval_id": "appr_20251228_193600Z_ef45gh",
  "proposal_id": "prop_20251228_193355Z_ab12cd",
  "job_id": "job_20251228_193300Z_01",

  "decision": "approve",
  "approved_by": "user",
  "approved_at": "2025-12-28T19:36:00Z",

  "approved_plan_hash": "sha256:....",
  "notes": "OK",
  "constraints_override": {
    "allow_git_commit": true,
    "hydration_level": "L0"
  }
}


Obrigatórios:
approval_id, proposal_id, job_id, decision, approved_by, approved_at, approved_plan_hash

3) execution_result (só “delivered” com verificação pass)
{
  "result_id": "res_20251228_193820Z_zz99yy",
  "proposal_id": "prop_20251228_193355Z_ab12cd",
  "job_id": "job_20251228_193300Z_01",
  "project_id": "DBB",

  "started_at": "2025-12-28T19:36:10Z",
  "finished_at": "2025-12-28T19:38:20Z",

  "status": "delivered",
  "failure_reason": null,
  "rollback_performed": false,

  "plan_hash_verified": true,
  "preflight": { "status": "pass", "details_ref": "vault://logs/preflight_...jsonl" },

  "verification": {
    "pass": true,
    "checks": [
      { "check_id": "v1", "check_type": "git_apply_check", "status": "pass" },
      { "check_id": "v2", "check_type": "git_diff_paths_only", "status": "pass" }
    ]
  },

  "evidence": [
    { "type": "git_commit", "value": "commit:deadbeef", "sha256": null },
    { "type": "file_sha256", "value": "docs/canon/APPROVAL_POLICY.md", "sha256": "..." },
    { "type": "git_status", "value": "clean_or_expected", "sha256": null }
  ],

  "logs": {
    "events_ref": "vault://logs/events_job_...jsonl",
    "stdout_ref": "vault://logs/stdout_job_...txt",
    "stderr_ref": "vault://logs/stderr_job_...txt"
  }
}


Obrigatórios:
result_id, proposal_id, job_id, project_id, started_at, finished_at, status, plan_hash_verified, verification.pass, evidence[], logs.events_ref

4) event (para observabilidade em tempo real)
{
  "ts": "2025-12-28T19:36:12Z",
  "job_id": "job_20251228_193300Z_01",
  "proposal_id": "prop_20251228_193355Z_ab12cd",
  "level": "info",
  "event_type": "STEP_START",
  "step_id": "s1",
  "message": "Running git apply --check",
  "redaction": { "applied": true, "suspicious": false }
}


Obrigatórios:
ts, job_id, event_type, message

5) evidence (tipos recomendados)

file_sha256 → {path, sha256}

git_commit → {commit_id}

git_status → {status}

exit_code → {command, code}

artifact_ref → {ref, sha256?}

screenshot_ref → {ref}
