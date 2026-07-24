# Global Engineering

## Principles

- Follow existing architecture
- SOLID, DRY, KISS, YAGNI
- Reuse existing code
- Prefer incremental changes
- Strong typing
- REST conventions
- OWASP security
- Validate all inputs
- Log actionable errors
- Keep responses concise

## MCP Usage

- Playwright MCP: Run tests only in headless/background mode. Never launch or require a visible browser.
- Database MCP: Read-only operations only. Never perform INSERT, UPDATE, DELETE, CREATE, ALTER, DROP, TRUNCATE, or EXECUTE statements.

## Validate Before Completion

- Secure
- Backward compatible
- Maintainable

## Never

- Rewrite unrelated code
- Modify files unrelated to the task
- Duplicate existing logic
- Add dependencies without approval