---
id: CLI handbook
title: Command-line interface handbook
source: writing-guidelines
section: developer-resources
---

## Help documentation

Users must have access to help within a CLI. 

When writing help documentation: 
- Keep help text brief but meaningful.
- Avoid internal jargon and acronyms without explanation.
- Ensure consistency across all commands.
- If your CLI supports [interactive prompts](#interactive-prompts), reference `--help` in the prompt.

**Tip:** Think like a user
- Ask: “If I saw this message for the first time, would I know what to do?”
- Avoid hidden meanings, technical slang, or filler words
- Be intentional with every line of output — it’s the primary UX for many CLI tools

### Writing help output
Help output should be consistent across commands and follow a clear structure:
1. Description: What the command does in plain language.
2. Usage: Syntax with required arguments and flags.
3. Examples: At least 1 clear, real-world usage example.
4. Flags: A list of available options with brief, actionable descriptions.
5. Documentation link (optional): If more detail exists elsewhere.

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

### Flag documentation
Well-documented flags improve discoverability, reduce user error, and make it easier for contributors and users to understand the CLI’s capabilities.

Clear flag documentation should be included in:
- `--help` output for the command.
- Official CLI documentation or reference guides.
- Interactive prompt hints (if applicable).

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

#### Best practices
- Use **descriptive names** that clearly indicate the flag’s purpose. 
- Document the **flag’s input type** (string, boolean, int, and so on). 
- Show the **default value** if one exists.  
- Mention whether the flag is optional or required.  
- Avoid vague or ambiguous flag names (like `--flag1`, `--optionX`).

## Interactive prompts

Interactive prompts guide users through a process step-by-step. They’re commonly used for setup wizards, configuration flows, or when input is too complex to pass in a single command.


### When to use interactive mode
- Recommended:
-- Setup or initialization wizards
-- Optional configuration flows
-- Profile or environment selection

- Avoid:
-- Simple one-off tasks
-- Repetitive actions (e.g., run, delete, status)
-- Commands that will be run in automation, CI, or pipelines

### Best Practices

- Use prompts for guided setup, not quick or repeatable tasks.
- Let users bypass prompts with a `--non-interactive` or `--yes` flag.
- Clearly show default values and how to accept them.
- Offer inline help (fore example "? for help").
- Don’t require prompts for essential inputs that could be passed as flags.

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

### Writing interactive prompts 

When writing interactive prompts, follow these best practices:

#### Default values: 

Choose defaults based on common use cases or sensible fallbacks. 
    - Tell users that pressing Enter will accept the default andwWill proceed with a pre-set value if left blank. 
    - Always indicate the default value in a clear, visual way. For example, `[default]`, `(default)` or `(Y/n)`/`(y/N)`.
    - Avoid making the first item the default unless it is truly the most likely choice.
    - Don’t hide defaults in help text or assume the user knows them.
    - Log or echo the selected value for confirmation (even if it was a default).

#### Clarity: 
    - Use question-style phrasing, such as "Enable autoscaling? (Y/n)".
    - Be clear if a prompt is optional.
    - Offer help inline, such as "Output directory [? for help]".
    - Avoid chaining too many prompts.
    - CLIs should clearly communicate when an action completes successfully.
    - Confirm successful actions with a short, clear message  
- Use consistent formatting, symbols, or icons for different message types  
- Include optional next steps when appropriate (e.g., view status, open logs)  
- Avoid silent success unless explicitly requested (e.g., in `--quiet` mode)

#### User control: 
    -  Cancel with Ctrl+C
    - Provide equivalent flags to bypass prompts

#### Multi-step flows: 
    - Consider using a `--guided` flag to trigger multi-step flows intentionally.
    - Allow users to exit early if needed. 

#### Error messaging and corrections

When something goes wrong, the CLI should help users understand:
- What happened
- Why it happened
- How to fix it

Error messages are a core part of the user experience. They should be written in plain language, not internal jargon, and provide actionable next steps.


- Make error messages human-readable  
- Avoid stack traces unless a debug flag is enabled  
-  Place the most important information at the end (where scanning starts)  
-  Suggest solutions whenever possible

**Generic failure with correction:**

```plaintext
❌ Error: Cannot connect to remote service.
Check your network connection or try again with `--offline` mode.
```

**File permission issue:**
```plaintext
❌ Error: Unable to write to file.txt
You may need to change the file’s permissions or run the command with elevated privileges.
```

**Command syntax error**
```plaintext
❌ Error: Unrecognized flag --versoin
Did you mean: --version ?
```

### Auto-correction and suggestions
Where appropriate, CLIs may offer suggestions or corrections for common typos:

```plaintext
❌ Error: Unknown command “initaite”
Did you mean: “initiate”?
```

### Debugging information
For unexpected or low-level errors:
- Provide a clean message by default
- Use --debug or --trace flags to expose full stack traces or internal logs

```plaintext
❌ Unexpected error: Operation timed out.
Run with --debug for more details.
```

## Output patterns

- **Success**: Action completed  
  `✅ Dataset uploaded successfully.`

- **Warning**: Action completed but with something to review  
  `⚠️ Model trained, but validation accuracy is low.`

- ❌ **Error**: Something failed (covered in previous section)

### Examples: Output Patterns

```plaintext
✅ Project "my-app" deployed successfully.

Next steps:
- Run `tool status my-app` to check deployment health
- Run `tool logs my-app` to view runtime output
```

### Structuring output for readability
- Use spacing, dividers, or indentation to break up large blocks of output.
- Use consistent headers or labels for repeated outputs (e.g., listing results or summaries).

```plaintext
Results:

✓ 12 files processed
✓ 2 files skipped (already up to date)
✓ 1 warning: Unused flag --optimize
```

### Version listing and pagination

When your CLI displays lists of items — such as software versions, available resources, or deployment options — it should be structured for easy scanning and include guidance when the list becomes long.

Pagination and sensible ordering improve readability and reduce cognitive overload, especially when the output may exceed the visible terminal window.

### Best Practices

- **Show the most relevant or default item first** (e.g., the currently installed version)
- **Clearly label the default or active item** (e.g., with a "Yes" or "*")
- **Paginate output when the list exceeds 5–7 items**
- **Use table formatting or columns for consistency**
- Avoid dumping unstructured text or long, scrollable blobs

### Examples:: Version Listing Output

```plaintext
Available Versions
-
VERSION     DEFAULT    AVAILABLE UPGRADES
1.4.3         Yes       1.4.4, 1.4.5, 1.5.0
1.3.9                   1.4.0, 1.4.1
1.3.8                   1.3.9
```
- Versions are ordered top-down from newest to oldest
- The current default is clearly marked
- Upgrade paths are shown next to each item

### Pagination tips
For long lists:
- Break into pages of 5–7 items at a time
- Prompt the user with --more, --page, or allow Enter to continue

**Examples:**
```bash
tool versions list --limit 5
tool resources list --page 2
```

### Sorting and Filtering
- Default sort should be by relevance or recency (e.g., newest version first)
- Offer flags like --sort, --filter, or --status for flexible output
- Make sure the default view meets the needs of most users — avoid requiring flags to make output usable

