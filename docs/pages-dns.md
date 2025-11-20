# GitHub Pages & DNS Checklist for pact-community.org

This page lists the manual steps repository owners or organization admins must perform to publish the site at `pact-community.org` and to forward `pactcommunity.org` to the canonical domain.

## High-level steps

1. In the repository Settings → Pages:
   - Set the publishing source (e.g., `gh-pages` branch or `main`/`docs`).
   - Set the **Custom domain** to `pact-community.org`.
   - Enable **Enforce HTTPS** once DNS has propagated and TLS is issued.

2. Ensure a `CNAME` file is present at the publishing root (the branch that serves Pages):
```
pact-community.org
```

3. DNS records for the primary domain (`pact-community.org`):
   - Apex A records (required):
     - `@  A  185.199.108.153`
     - `@  A  185.199.109.153`
     - `@  A  185.199.110.153`
     - `@  A  185.199.111.153`
   - Apex AAAA records (recommended for IPv6):
     - `@  AAAA  2606:50c0:8000::153`
     - `@  AAAA  2606:50c0:8001::153`
     - `@  AAAA  2606:50c0:8002::153`
     - `@  AAAA  2606:50c0:8003::153`
   - `www` CNAME:
     - `www  CNAME  pact-community-organization.github.io.`

4. Secondary domain (`pactcommunity.org`) — forwarding only
   - Configure registrar- or provider-level forwarding (301 permanent) for both apex and `www` to `https://pact-community.org/`.
   - Do NOT add the secondary domain as a GitHub Pages custom domain (GitHub Pages supports a single custom domain per site).

## Verification

Run these checks after DNS changes propagate:

```bash
dig pact-community.org A +short
dig pact-community.org AAAA +short
dig www.pact-community.org CNAME +short

curl -I https://pact-community.org/
curl -I http://pactcommunity.org/  # expect 301 to https://pact-community.org/
```

## Notes

- TLS certificate issuance by GitHub can take several minutes to a few hours after DNS updates.
- If you change the Pages publishing source (e.g., from `docs/` to `gh-pages`), ensure the `CNAME` file is present in the new source branch.
- Update the `www` CNAME target if the Pages host changes (e.g., moving to another organization).
