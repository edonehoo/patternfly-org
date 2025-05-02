---
id: CLI handbook
title: Command-line interface handbook
source: overview
section: developer-resources
---

# Designing for command-line interfaces

This resource provides best practices and guidelines for designing consistent, usable, and developer-friendly command-line interfaces (CLIs). These principles are intended to support both developers building CLI tooling and designers collaborating on technical products.

These guidelines emphasize clarity, structure, and user-centered design across command syntax, help documentation, error messaging, and interactive behaviors. While they are applicable to a wide range of use cases, they are particularly helpful for teams working in environments where CLI tools serve as the primary or critical part of the user experience.

## Accessibility considerations

While CLIs don't have visual UI elements, many [accessibility principles](/accessibility/about-accessibility/) still apply.

Accessibility matters in CLI design just as much as it does in graphical interfaces. Clear, inclusive output ensures all users—including those using screen readers or alternative input devices—can successfully interact with the tool.

### Color 

| **Don't** | **Do** |
|--------|-----------|
| Don’t rely on color alone to convey meaning. | Do use text alongside color-based cues (such as “Success” and “Error” labels). <br/><br/> Do use text alongside red/green indicators, to make the information accessible to colorblind users. |

### Content

| **Don't** | **Do** |
|--------|-----------|
| Don't use vague terms like “it failed”.| Do be direct and transparent with descriptive, specific language. |
| Don’t assume users can infer meaning from context alone.| Do use descriptive text for prompts and feedback. | 
| Don't use prompt flows that require visual scanning without clear text guidance. | Do ensure all commands and prompts are keyboard-accessible and non-interactive-safe. <br/><br/> Do use flags like `--non-interactive` for scripting or assistive tech users. |
| Don't use overly styled ASCII tables, long walls of text, or dynamic animations. | Do use plain, structured output that screen readers can parse. <br/><br/> Do use clean, labeled output with headings, bullet points, or clear separators. |

### Testing 

It is important to test your output through accessibility tools to reveal subtle issues in contrast, clarity, and verbosity.

| **Don't** | **Do** |
|--------|-----------|
| Don't assume that your design is accessible. | Do test with screen readers and colorblind simulators. |

### Example

**Accessible:**
```plaintext
✅ Deployment successful.
Run `tool status` to check environment health.
```

**Less accessible**
```plaintext
✅ You did it!
```

## CLI inputs

CLI inputs are composed of a few parts: 
- [Command name:](#command-names) Identifies the action a command will perform and the subject that the action applies to.
- [Arguments:](#arguments) Additional details used alongside the command name, which users select to specify the way that a command is applied.
- [Flags:](#flags) Named parameters that modify the behavior of the command.

### Commands

A **command** describes the action that the CLI will trigger. 

Command names should follow a clear and consistent verb-noun structure:
- Verb: The action the command performs.
- Noun*: The resource or object the action applies to.

Verb-noun commands mirror how people think about tasks, which makes the command more intuitive and easier for users to discover or guess. Noun-verb structures can be harder to parse and don't scale well. The verb-noun format also aligns with common patterns seen in widely-used CLIs like `git`, `kubectl`, and `docker`. 

 

```bash
tool create project
tool delete environment
tool scale deployment
```

`create project`, `delete environment`, and `scale deployment` are all commands.

### Arguments

An argument is anything to the right of a command that is not a flag. Often, they will be file paths, project names, or similar unique identifiers.

A command requires an argument in order to execute. There can be multiple arguments in a command, but the order of arguments matters. It is better to use fewer arguments when possible, to make it easier for users to remember and avoid confusion.



```bash
tool delete project my-app
tool deploy environment production
```

`delete project` and `deploy environment` are commands, while `my-app` and `production` are arguments that represent the object being acted on.

### Flags

Flags are named parameters that modify the behavior of a command. They allow users to specify command modifiers, options, or other non-essential configuration. Flags can be added in any order.



```bash
tool deploy --env staging --force
tool create user --role admin --email user@Examples:.com
```

#### Forms 

There are 2 forms for flags: 

##### 1. Long-form flags

Long-form flags are clearer and more descriptive. 

They are written with 2 hyphens (--) and a descriptive name. 



```bash
tool deploy app --enable-autoscaling
tool configure user --assign-admin-privileges
tool update cluster --set-min-replicas 3
```

##### 1. Short-form flags

Short-form flags are more concise than long-form flags. They are and are particularly useful frequently used options, experienced users who prefer speed, and situations where space or typing efficiency matters.

They are written with a single hyphen (-) followed by 1 or 2 letters.
  
When possible, pair short-form flags with long-form equivalents for clarity and discoverability.



```bash
tool --help          # Long-form
tool -h              # Short-form (Help)

tool --verbose       # Long-form
tool -v              # Short-form (Verbose)

tool --config path/to/file
tool -c path/to/file # Short-form (Config file path)
```

#### Types 

1. Boolean flags: Represent on/off or true/false options. 

    - Set a sensible default.
    - Don’t require users to pass an explicit value unless necessary.

    ```bash
    tool deploy --dry-run         # Runs without executing
    tool delete --force           # Skips confirmation prompt
    ```

2. Help flags: Allow users to receive help and load additional documentation that explains:
    - What a command does.
    - What arguments or flags it accepts.
    - How to use it, with clear examples.

    Users should be able to use both a long (`--help` ) and short help (`-h`) flag.