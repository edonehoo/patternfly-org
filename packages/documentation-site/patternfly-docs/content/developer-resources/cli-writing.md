---
id: CLI handbook
title: Command-line interface handbook
source: writing-guidelines
section: developer-resources
---

## CLI outputs

CLI output messages typically fall into one of 3 categories:

| Category | Usage | Example |
| --- | --- | --- |
| Success | Completed actions. | `✅ Dataset uploaded successfully.` |
| Warning | Completed actions that require review. |  `⚠️ Model trained, but validation accuracy is low.` |
| Error | Failed actions. |  `❌ Error: Cannot connect to remote service. Check your network connection or try again with --offline mode.` |


### Output patterns

#### Formatting 

To ensure that output is readable: 

- Use spacing, dividers, or indentation to break up large blocks of output.
- Use consistent headers or labels for repeated outputs (such as listing results or summaries).
- When your CLI displays lists of items&mdash;such as software versions, available resources, or deployment options&mdash;it should be structured for easy scanning and include guidance when the list becomes long.
- Show the most relevant or default item first (like the currently installed version).
- Clearly label the default or active item (like with a "Yes" or "*").
- [Paginate](#pagination) output when the list exceeds 5–7 items.
- Use table formatting or columns for consistency.
- Avoid dumping unstructured text or long, scrollable blobs.

```plaintext
Results:

✓ 12 files processed
✓ 2 files skipped (already up to date)
✓ 1 warning: Unused flag --optimize
```

#### Success messages

Clearly communicate when an action completes successfully to build trust, reinforce that a command worked as intended, and guide users toward their next step.

```plaintext
✅ Project "my-app" deployed successfully.

Next steps:
- Run `tool status my-app` to check deployment health
- Run `tool logs my-app` to view runtime output
```

#### Error messages

When something goes wrong, help users understand what happened, why it happened, and how to fix it. 

Like with our standard [UI error writing guidelines](/ux-writing/error-messages), CLI errors should:
- Be written in plain language. 
- Not use internal jargon. 
- Provide suggestions and actionable next steps.
- End with the most important information, rather than lead with it.
- Not display stack traces unless a debug flag is enabled.  

For unexpected or low-level errors:
- Provide a clean message by default.
- Use `--debug` or `--trace` flags to expose full stack traces or internal logs.

#### Version listing

When listing version information: 
- Arrange versions top-down from newest to oldest.
- Clearly mark the current default.
- Display upgrade paths beside each item.

```plaintext
Available Versions
-
VERSION     DEFAULT    AVAILABLE UPGRADES
1.4.3         Yes       1.4.4, 1.4.5, 1.5.0
1.3.9                   1.4.0, 1.4.1
1.3.8                   1.3.9
```

#### Pagination

Use pagination to break long lists into pages of 5–7 items at a time. When using pagination, prompt the user with `--more`, `--page`, or allow them to press **Enter** to continue

```bash
tool versions list --limit 5
tool resources list --page 2
```

#### Sorting and filtering

To help users make sense of large CLI output, allow them to sort and filter as needed. When using sorting and filtering:

- Default sort should be by relevance or recency.
- Offer flags like `--sort`, `--filter`, or `--status` for flexible output.
- Make sure the default view meets the needs of most users and avoid requiring flags to make output usable

## Help documentation

Users must have access to help within a CLI. 

When considering the relevance of help documentation, think like a user and ask yourself: “If I saw this message for the first time, would I know what to do?”

When writing help documentation: 
- Keep help text brief but meaningful.
- Avoid internal jargon and acronyms without explanation.
- Ensure consistency across all commands.
- If your CLI supports [interactive prompts](#interactive-prompts), reference `--help` in the prompt.

### Writing help output
Help output should be consistent across commands and follow a clear structure, containing these elements:
- **Description:** What the command does in plain language.
- **Usage:** Syntax with required arguments and flags.
- **Examples:** At least 1 clear, real-world usage example.
- **Flags:** A list of available options with brief, actionable descriptions.
- **Documentation link (optional):** If more detail exists elsewhere.

```bash
Usage:
  tool deploy <project_name> [flags]

Description:
  Deploys the specified project to your active environment.

Examples:
  tool deploy my-app --env staging

Flags:
  -e, --env string        Environment to deploy to
  -f, --force             Force deployment even if conflicts exist
  -h, --help              Show help for the deploy command

For more information, visit: https://Examples:.com/docs/deploy
```

### Documenting flags
Well-documented flags improve discoverability, reduce user error, and make it easier for contributors and users to understand the CLI’s capabilities.

Clear flag documentation should be included in:
- `--help` output for the command.
- Official CLI documentation or reference guides.
- Interactive prompt hints (if applicable).

#### Best practices
- Use **descriptive names** that clearly indicate the flag’s purpose. 
- Document the **flag’s input type** (string, boolean, int, and so on). 
- Show the **default value** if one exists.  
- Mention whether the flag is optional or required.  
- Avoid vague or ambiguous flag names (like `--flag1`, `--optionX`).

Flag documentation should show the following:

```plaintext
--flag-name <type>     Description of what this flag does (default: value)
```

```plaintext
Flags:
  -n, --name string             Name of the project to create
  -e, --env string              Target environment (e.g., staging, prod)
  -f, --force                   Skip confirmation prompts (default: false)
  -o, --output string           Output format: json, yaml, or table (default: table)
  -h, --help                    Show help for this command
```

For boolean flags that act as switches (true/false):
- Default to false unless enabled.
- Do not require an explicit value (for example, use `--force`, not `--force=true`).
- Clearly state what enabling the flag does.
- When possible, explain how the flag affects the command’s behavior or when it’s most useful:

```plaintext
--dry-run     Simulate the command without making changes. Useful for validation or preview.
--watch       Continuously stream status updates until completion.
```

## Interactive mode

The CLI's interactive mode uses prompts to guide users through a process step-by-step., commonly for setup wizards, configuration flows, or  input that is too complex to pass in a single command.

### When to use interactive mode

User interactive mode for:
- Setup or initialization wizards.
- Optional configuration flows.
- Profile or environment selection.

### When not to use interactive mode

Do not use interactive mode for: 
- Simple one-off tasks.
- Repetitive actions (such as run, delete, status).
- Commands that will be run in automation, CI, or pipelines.

### Writing interactive prompts 

When writing interactive prompts, follow these best practices:

- Use prompts for guided setup, not quick or repeatable tasks.
- Let users bypass prompts with a `--non-interactive` or `--yes` flag.
- Clearly show default values and how to accept them.
- Offer inline help (for example "? for help").
- Don’t require prompts for essential inputs that could be passed as flags.

#### Default values

Choose defaults based on common use cases or sensible fallbacks.

    - Tell users that pressing Enter will accept the default and will proceed with a pre-set value if left blank. 
    - Always indicate the default value in a clear, visual way. For example, `[default]`, `(default)` or `(Y/n)`/`(y/N)`.
    - Avoid making the first item the default unless it is truly the most likely choice.
    - Don’t hide defaults in help text or assume the user knows them.
    - Log or echo the selected value for confirmation (even if it was a default).

#### Clarity

Be as clear as possible in all messaging.

    - Use question-style phrasing, such as "Enable autoscaling? (Y/n)".
    - Be clear if a prompt is optional.
    - Offer help inline, such as "Output directory [? for help]".
    - Avoid chaining too many prompts.
    - CLIs should clearly communicate when an action completes successfully.
    - Confirm successful actions with a short, clear message  
- Use consistent formatting, symbols, or icons for different message types  
- Include optional next steps when appropriate (e.g., view status, open logs)  
- Avoid silent success unless explicitly requested (e.g., in `--quiet` mode)

#### User control

Give users control over their experience in the CLI.

    - Consider using a `--guided` flag to trigger multi-step flows intentionally.
    - Provide flags to bypass prompts.
    - Allow users to cancel or exit flows early with **Ctrl**+**C**.

### Example 

```plaintext
Welcome! Let's configure your project.

Project name: my-app
Language (js, py, go) [py]: 
Use Docker? (y/N): y

✅ Project "my-app" configured successfully.
```

```plaintext
Choose deployment region:
[1] US East (N. Virginia)
[2] US West (Oregon) (default)
[3] Europe (Frankfurt)

Enable telemetry? (y/N)
```

```plaintext
Select output format (default = 'json') [Use arrows to move, type to filter, ? for help]:
```




