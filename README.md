# Scanner output

The code and scanner outputs that enabled us to perform the triage are provided here.

**Careful:** everything under `flagged/` is real malicious code pulled off the Hub -
load-time RCE, environment exfiltration, a remote second stage. Read it, don't run it.

## What's here

Fourteen repos, split by type:

```
Model/      5 repos
Dataset/    8 repos
   audit/      one JSON per repo - the whole run
   flagged/    the files that tripped something, each with its verdicts
```

All fourteen came back `malicious`. That's the point - this is the flagged set, not a
random sample.

## Reading the JSON

`audit/<owner>__<repo>__<timestamp>.json` is the repo-level result: how many files were
scanned, which ones flagged, which scanners fired, plus a `file_results` entry per file.

Sitting next to each flagged file is a `<name>.verdicts.json` - the same thing at file
level. `flagged_by` says which scanners hit it, and each verdict carries the rule id, the
message, the line number, and the snippet that matched.

Six scanners ran over every file: opengrep, bandit, trivy, pysentry, pip_audit,
modelaudit. So a scanner missing from `flagged_by` **ran and found nothing** - that
absence is what most of this evidence is actually about.

## Things worth knowing before you dig in

Only flagged files were staged, so these aren't full repo clones. The paths are real; the
gaps are files that came back clean.

