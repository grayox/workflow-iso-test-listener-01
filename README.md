 ===================================================================================
 ==                            DEPRECATION NOTICE                                 ==
 ===================================================================================

This workflow (or trigger configuration) represents an attempt to use GitHub's
native `workflow_run` event for cross-repository communication to trigger a
GitHub App.

Our extensive diagnostics, including pure isolation tests (e.g., using
`source-repo-test` → `listener-repo-test`), conclusively demonstrated that:

1.  `workflow_run` events work perfectly for *same-repository* triggers.
2.  `workflow_run` events *do not reliably deliver* across repository boundaries
    to a subscribed GitHub App, despite all App permissions and event subscriptions
    being correctly configured (API-verified). The event simply does not arrive.

DECISION TO DEPRECATE:
Due to the proven unreliability of cross-repository `workflow_run` event delivery
to GitHub Apps, this approach has been deprecated. We have pivoted to an
**API-driven (repository_dispatch) model** where client repositories explicitly
notify the `debug-01` App of failures.

INSTITUTIONAL KNOWLEDGE GAINED:
-   Do NOT rely on cross-repository `workflow_run` triggers for GitHub Apps
    when designing mission-critical event delivery mechanisms.
-   Instead, opt for an explicit `repository_dispatch` API call from the source
    repository, using `actions/github-script` for robust control and transparency.
-   `repository_dispatch` is a reliable, imperative "API" for workflow-to-workflow
    communication, whereas `workflow_run` as a cross-repo App trigger is not.

This file is retained for historical diagnostic context. Do not use this trigger
configuration for active development.

 ===================================================================================