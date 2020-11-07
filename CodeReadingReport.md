# Code Reading Report

I will mainly read the checkers located in analyzer/apisan/check.

## retval.py

```python
def get_bugs(self):
    bugs = []
    for key, value in self.ctx_uses.items():
        total = self.total_uses[key]
        diff = copy.copy(total)
        scores = {}
        for ctx, codes in value.items():
            score = len(codes) / len(total)
            if score >= config.THRESHOLD and score != 1:
                diff = diff - codes
                for bug in diff:
                    scores[bug] = score

        if len(diff) != len(total):
            added = set()
            for bug in diff:
                if bug in added:
                    continue
                added.add(bug)
                br = BugReport(scores[bug], bug, key, ctx)
                bugs.append(br)
    return bugs
```
