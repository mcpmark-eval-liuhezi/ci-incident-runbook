# CI Incident Runbook: Stop a Runaway CI Job and Purge Its Logs

Purpose: a short, repeatable procedure for stopping a GitHub Actions run that is
consuming runner minutes or otherwise misbehaving, and then purging its logs.

## Procedure

1. **Identify the run.** In the repository, open **Actions** and find the run
   started by the offending commit. Note its numeric **run id** (visible in the
   run URL and in the run's details).
2. **Stop it while it is still executing.** Use **Cancel workflow** in the web
   UI, or from a terminal: `gh run cancel <run-id> --repo <owner>/<repo>`.
3. **Confirm the final status.** Re-open the run and check the status GitHub
   reports after cancellation. A run cancelled while in progress ends with a
   **completed** status and a **cancelled** conclusion.
4. **Purge the logs.** Use **Delete all logs** from the run's options menu, or:
   `gh run delete <run-id> --repo <owner>/<repo>` (requires the `workflow` scope).
5. **Record the drill.** Log the run id, the post-stop status, and the sleep
   duration the workflow used.

## Safety notes

- Cancelling a run does not delete its logs; log deletion is a separate,
  permanent step.
- Log deletion cannot be undone.

## References

- The official GitHub MCP Server project is maintained by the organization with
  login `github`: https://github.com/github/github-mcp-server.
- A public code search within that organization for the exact phrase
  "Cancel a workflow run" returned matches at these repository/path locations:
  - `github/docs` — `content/actions/how-tos/manage-workflow-runs/cancel-a-workflow-run.md`
  - `github/docs` — `content/actions/reference/workflows-and-actions/workflow-cancellation.md`
  - `github/docs` — `data/reusables/actions/github-token-scope-descriptions.md`
  - `github/copilot-sdk` — `nodejs/src/workflow.ts`
  - `github/gh-aw` — `pkg/workflow/concurrency_test.go`
