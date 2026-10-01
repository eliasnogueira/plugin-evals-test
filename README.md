# plugin-evals-test

Minimal Claude Code plugin to reproduce an issue with `claude plugin eval` where `context.add_dirs` in `case.yaml` does not copy fixture files into the eval sandbox working directory.

## The issue

The plugin contains a "Quote Identifier" skill that reads a quote from a file, identifies the author and source, and writes an `information.md` report. The eval suite provides fixture files (`quote.txt`) via `context.add_dirs: ["test/"]` in each case's `case.yaml`, but the files never appear in the sandbox — causing every eval case to fail.

See [issue.md](issue.md) for the full bug report with error logs, traces, and reproduction steps.

## Project structure

```text
plugin-evals-test/
├── .claude-plugin/plugin.json          # plugin manifest
├── skills/plugins-evals-test/SKILL.md  # Quote Identifier skill
├── evals/
│   ├── 01-fear-itself/
│   │   ├── prompt.md                   # eval prompt
│   │   ├── case.yaml                   # context.add_dirs: ["test/"]
│   │   ├── test/quote.txt              # fixture file (not copied to sandbox)
│   │   └── graders/                    # file_exists + regex + llm graders
│   ├── 02-i-think-therefore/           # same structure
│   ├── 03-injustice-anywhere/          # same structure
│   ├── 04-to-be-or-not/               # same structure
│   └── 05-neg-summarize-file/          # negative case (skill should NOT fire)
├── eval-sandbox-debug.zip              # sandbox temp dirs showing empty cwd/
├── issue.md                            # full bug report
└── README.md
```

## Running the eval

```bash
claude plugin eval . --runs 1 --ablation with-without --allow-tools Write --keep-temp
```

All fire cases will score 0.00 because `quote.txt` is never copied into the sandbox.
