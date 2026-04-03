# Usage Examples

This document provides practical examples of how to work with and extend the ubiquibot-whilefoo-testing/testing repository.

## Example 1: Creating a Test Issue

To test issue labeling or bot responses:

1. Navigate to the **Issues** tab in the repository.
2. Click **New Issue**.
3. Use a descriptive title, e.g., "Test: Feature request for new module".
4. In the body, provide a clear description:
   ```markdown
   **Description**
   This is a test issue to verify that the Ubiquibot correctly labels feature requests.
   
   **Expected Behavior**
   The issue should receive a `feature` label automatically.
   ```
5. Submit the issue and observe the automated labeling.

## Example 2: Submitting a Pull Request for Documentation

Follow the steps in [CONTRIBUTING.md](../CONTRIBUTING.md) to submit a PR. Here's a concrete example:

1. Fork the repository on GitHub.
2. Clone your fork locally:
   ```bash
   git clone https://github.com/your-username/testing.git
   cd testing
   ```
3. Create a new branch:
   ```bash
   git checkout -b docs/update-examples
   ```
4. Create a new file `docs/NEW_EXAMPLE.md` with your content.
5. Stage and commit:
   ```bash
   git add docs/NEW_EXAMPLE.md
   git commit -m "docs: add new usage example for configuration"
   ```
6. Push and open a PR:
   ```bash
   git push origin docs/update-examples
   ```
7. On GitHub, navigate to your fork, click **Compare & pull request**, and fill in the PR template.

## Example 3: Modifying Bot Configuration

Suppose you want to add a new label trigger for issues containing "[BUG]".

1. Edit the file `.github/.ubiquibot-config.yml` (or the relevant config).
2. Add a new rule under the appropriate section. Example structure:
   ```yaml
   rules:
     - name: Label bug reports
       conditions:
         - title_contains: "[BUG]"
       actions:
         - type: add_label
           label: "bug"
   ```
3. Test the change by creating an issue with "[BUG]" in the title.
4. Submit a PR with your configuration change, explaining the rationale.

## Example 4: Running Automated Checks Locally

If the project uses GitHub Actions, you can often run the same checks locally using `act` or other tools. For a simple lint check:

```bash
# If using Node.js
npm run lint

# If using Python
pip install pre-commit
pre-commit run --all-files
```

## Example 5: Adding a New Script Module

To add a reusable script:

1. Create a new directory `scripts/` if it doesn't exist.
2. Add your script, e.g., `scripts/validate_config.py`:
   ```python
   #!/usr/bin/env python3
   """Validate configuration files."""
   
   import yaml
   import sys
   
   def validate_config(path):
       try:
           with open(path, 'r') as f:
               yaml.safe_load(f)
           print(f"✓ {path} is valid YAML")
           return True
       except yaml.YAMLError as e:
           print(f"✗ {path} has YAML error: {e}")
           return False
   
   if __name__ == "__main__":
       if len(sys.argv) < 2:
           print("Usage: python validate_config.py <config_file>") 
           sys.exit(1)
       success = validate_config(sys.argv[1])
       sys.exit(0 if success else 1)
   ```
3. Make it executable: `chmod +x scripts/validate_config.py`.
4. Update documentation to mention the new script.
5. Optionally, add a GitHub Actions step to run it automatically.

## Example 6: Responding to Review Comments

After submitting a PR, you may receive review comments. To address them:

1. On your local branch, make the requested changes.
2. Commit with a message that references the feedback:
   ```bash
   git commit -m "fix: address review comments on config validation"
   ```
3. Push to the same branch; the PR will update automatically.
4. Respond to each comment on GitHub to indicate it's been resolved.

## Example 7: Testing with Different Environments

If the project supports multiple environments (e.g., staging/production), you can test configurations by creating a branch with environment-specific changes. Use feature flags or environment variables to toggle behavior.

## Common Pitfalls and Solutions

- **Configuration syntax errors**: Use a YAML linter (`yamllint`) before committing.
- **Missing permissions**: Ensure the Ubiquibot app is installed on the repository and has write access.
- **Tests failing locally but passing on CI**: Check for environment differences; use Docker to match the CI environment.
- **Merge conflicts**: Regularly rebase your branch on `main` to avoid conflicts:
  ```bash
  git fetch upstream
  git rebase upstream/main
  ```

## Need More Help?

Refer to the project's main documentation or open an issue with your specific question.
