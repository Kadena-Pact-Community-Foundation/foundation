# Teams and Initial Permissions

This document describes the teams to create for the Pact Community Organization and suggested initial permissions.

## Teams to create

- **Core-Maintainers**: permission `maintain` on core repositories. Responsibilities: code review, merges, releases, governance.
- **Website-Team**: permission `maintain` on `website`. Responsibilities: website content, pages, deployments.
- **Catalog-Team**: permission `maintain` on `pact-contract-catalog`. Responsibilities: catalog maintenance, metadata validation, contract onboarding.
- **Security-WG**: permission `triage` (or `maintain`) across repos as needed. Responsibilities: vulnerability triage and coordination.

## Initial assignments

- `DaisukeFlowers`: org owner and team maintainer for all teams.

## Notes

- Creating teams requires org admin privileges. After this PR is merged, a follow-up step will create the teams via `gh team create` (or org admin UI). This PR documents the desired team names and permissions.

References: Issue: https://github.com/Pact-Community-Organization/foundation/issues/57

