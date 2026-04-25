# Rich Card Note Template

Use this structure when creating companion notes for rich cards (items needing structured tracking). Store notes in `Projects/<SLUG>/`.

## Template

```markdown
# SLUG-XX: Title

#type-tag #priority #domain-tag #status-tag
**From** @person

## Summary
One-paragraph description of the issue or task. Include: what's happening, what area/service is affected, what the expected outcome is.

## Updates
- Chronological bullet list of investigation steps, responses, findings
- Each update prefixed with date if spanning multiple days

## Next
Current next action or what we're waiting on.
```

## Example

```markdown
# BUG-01: Production API latency spike

#bug #urgent #backend #in-progress
**From** @oncall-lead

## Summary
API response times jumped from ~200ms to ~2s for authenticated endpoints. Started after the 14:00 deployment. Unauthenticated health checks unaffected. Users reporting timeouts on the mobile app.

## Updates
- Checked deployment diff - new middleware added for request logging
- Middleware was doing synchronous log writes to disk
- Confirmed: disabling the middleware restores normal latency
- Filed ticket to make log writes async

## Next
Deploying hotfix to disable sync logging. Async version scheduled for next sprint.
```
