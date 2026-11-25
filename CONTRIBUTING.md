# Contributing to Awesome GTA VI Reverse Engineering

Thanks for helping keep this index accurate, useful, and clean-room.

## Inclusion Rules

Every entry must satisfy all of the following:

1. **Real code or technical research:** The repository contains working tools, libraries, emulation/recompilation code, concrete byte-level format documentation, or useful developer reference data with actual commit history. Empty placeholders, single-file stubs, and vaporware repos are not accepted.
2. **No leaks or malware:** Do not submit leaked game executables, pirated build downloads, or README-only repos shipping unverified binaries. Real open-source projects with public source code that also publish prebuilt releases are fine.
3. **Accurate status:** Experimental runtimes, partial decoders, and early translation layers must state clearly what works and what does not yet boot.
4. **No spam or malware bait:** No clickbait PC installers, cheat code sites, SEO farms, uncredited copies, or AI-slop wrapper pages.

## Pull Request Format

- Add one item per PR (or one tightly scoped category update) in alphabetical or logical order inside the matching table in `README.md`.
- Tables have two columns. The first holds the project name, its star badge, and a small line with stack, license, and platform:
  `| **[Name](https://github.com/owner/repo)**&nbsp;<star badge><br><sub>Rust (`MIT`) · Windows</sub> | Description |` (copy a neighbouring row).
- Keep descriptions to one to three sentences, concrete and free of hype: what the repo actually implements and what does not work yet. Version-by-version detail belongs in the project's own README.
- Verify that the repository is public and actively accessible.
