<!-- file: .github/copilot-instructions.md -->
<!-- version: 1.0.2 -->
<!-- guid: f6a7b8c9-d0e1-4234-f5a6-b7c8d9e0f1a2 -->

# AI Agent Instructions (Standard)

- Use VS Code tasks for non-git operations; prefer MCP GitHub tools for git operations.
- Commits must use conventional commits (type(scope): description).
- Include versioned headers in docs/configs and bump versions on changes.
- This is a composite GitHub Action that embeds Python logic directly in action.yml.
- All changes to commands should be made in the embedded Python script within action.yml.
- Follow GitHub Actions composite action best practices.
- Provide concise plans and progress updates.
