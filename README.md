# GitHub Expression Evaluation Test

This repository tests the difference between expression evaluation in different GitHub Actions contexts:

- `schedule`: Runs every minute to test scheduled context
- `workflow_dispatch`: Manual trigger to test dispatch context
- `pull_request`: Tests PR context when PRs are created

## The Issue

In scheduled workflows, `github.event.pull_request` doesn't exist, so accessing `github.event.pull_request.number` can cause evaluation errors.

## Test Cases

1. **Original Expression**: `${{ github.event.pull_request.number || 0 }}`
   - Should fail in scheduled context
   - Works in PR context

2. **Fixed Expression**: `${{ github.event.pull_request && github.event.pull_request.number || 0 }}`
   - Should work in all contexts
   - Safe null checking

## Running Tests

1. Push to main branch
2. Watch scheduled runs (every minute)
3. Create a PR to test PR context
4. Use workflow_dispatch to test manual context