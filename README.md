# Kadena Pact Community Foundation

Welcome. This repository is the front door for our community work on Kadena smart contracts and tooling.

## Mission
Make it easy and safe for businesses to start building on Kadena.

## Vision
A thriving Kadena ecosystem where new and existing businesses can launch confidently, supported by trusted, audited smart contracts and ready-to-use building blocks.

## Resources
- [Wiki](https://github.com/Kadena-Pact-Community-Foundation/foundation/wiki) - Primary documentation (automatically synced to Pages)
- [Pages](https://Kadena-Pact-Community-Foundation.github.io/foundation) - Public-facing site with Wiki content
- [Discussions](https://github.com/Kadena-Pact-Community-Foundation/foundation/discussions)

## Wiki to Pages Sync
Wiki content is automatically mirrored to GitHub Pages for public visibility. The sync happens:
- When Wiki pages are updated (via `gollum` event)
- When changes are pushed to the main branch
- Manually via the Actions tab (workflow_dispatch)

The workflow copies all Wiki markdown files to the `docs/` directory and transforms Wiki-style links to work with GitHub Pages.
