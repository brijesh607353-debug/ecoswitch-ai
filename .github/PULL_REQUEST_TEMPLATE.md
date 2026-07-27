## Summary

<!-- Describe the change in 2-3 sentences. What does this PR do and why? -->

## Type of change

- [ ] 🐛 Bug fix (non-breaking change that fixes an issue)
- [ ] ✨ New feature (non-breaking change that adds functionality)
- [ ] 💥 Breaking change (fix or feature that would cause existing functionality to change)
- [ ] 📖 Documentation update
- [ ] 🔧 Chore / refactor (no functional change)
- [ ] ⚡ Performance improvement

## Related issues

<!-- Link issues this PR closes or relates to -->
Closes #

## Changes made

<!-- Bullet-point list of what changed -->
- 

## Checklist

- [ ] My branch is up to date with `main` (`git fetch upstream && git rebase upstream/main`)
- [ ] `pnpm run typecheck` passes with no errors
- [ ] I have not committed `.env`, `.env.local`, or any file containing secrets
- [ ] I ran codegen after changing `lib/api-spec/openapi.yaml` (`pnpm --filter @workspace/api-spec run codegen`)
- [ ] I have added or updated documentation where relevant
- [ ] I have added tests (if applicable)
- [ ] My commit messages follow the [Conventional Commits](https://www.conventionalcommits.org/) format

## Screenshots / recordings

<!-- For UI changes, attach before/after screenshots or a screen recording -->

## Notes for reviewers

<!-- Anything specific you want reviewers to focus on? -->
