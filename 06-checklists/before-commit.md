# Pre-Commit Quality Assurance Checklist

## Purpose
To ensure that all AI-generated and human-written code meets enterprise quality standards before being committed to version control. This workflow prevents hallucinations, leftover debugging code, and regressions from polluting the repository.

## 1. Human Verification (The "No Blind Trust" Rule)
Before staging any files, review the Git Diff manually or using Cursor's Source Control tab.

- [ ] **Scope Check:** Did the AI modify any files outside the intended scope of this feature/bug fix? If yes, revert those specific files.
- [ ] **Debug Debris:** Are all `console.log`, `print()`, `debugger`, and commented-out legacy code blocks removed?
- [ ] **Placeholder Check:** Did the AI leave any placeholders like `// TODO: Implement this` or `YOUR_API_KEY_HERE`?

## 2. Security & Architecture Validation
- [ ] **Secrets:** Verify no environment variables, API keys, or database credentials are hardcoded in the staged files.
- [ ] **Typing:** Ensure no implicit or explicit `any` types were introduced by the AI as a shortcut.
- [ ] **Imports:** Check for unused imports or imports from incorrect relative paths that the AI might have hallucinated.

## 3. Automated Local Checks
Run your local verification commands before committing.

- [ ] **Formatting:** Run the code formatter (e.g., Prettier, Black) to ensure consistent styling.
- [ ] **Linting:** Run the linter (e.g., ESLint, Ruff) and ensure there are zero warnings or errors.
- [ ] **Tests:** Run the local unit/integration test suite. Ensure all tests pass. If the AI wrote new tests, verify they actually test the business logic and aren't just tautologies (e.g., `expect(true).toBe(true)`).

## 4. AI-Assisted Commit Message Generation
Once the code passes all checks, use Cursor to generate a standardized commit message.

**The Commit Prompt (Use in Cursor Chat or CMD/CTRL + K in Source Control input):**
> "Review my staged changes. Generate a conventional commit message. 
> Format: `<type>(<scope>): <short description>`
> Followed by a bulleted list of what changed. 
> Types allowed: feat, fix, chore, refactor, docs, test. 
> Do not invent features that are not in the diff."

## Final Review
- [ ] The generated commit message accurately reflects the changes and follows the Conventional Commits standard.