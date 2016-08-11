# Jenkins Warning plugin parser for whale-linter

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

This is a 2 part parser:
- Parsing output into line output
 - Format: Priority;Line Number;Category;Message
- Parsing line output with Warning plugin

This is for whale-linter v0.0.4
---

**Name:** Custom-whale-linter

**Link Name:** whale-linter

**Trend report name:** whale-linter

**Regular Expression:** `^([\w]+);([\d]+);([^;]+);(.*)$`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning
import hudson.plugins.analysis.util.model.Priority

String priority = matcher.group(1)
String lineNumber = matcher.group(2)
String category = matcher.group(3)
String message = matcher.group(4)
//String fileName = matcher.group(1)
//String type = matcher.group(4)
Priority prio = Priority.NORMAL
fileName = "Dockerfile"
type = ""

if (priority == "CRITICAL") {
  prio = Priority.HIGH
} else if (priority == "WARNING") {
  prio = Priority.NORMAL
} else {
  prio = Priority.LOW
}

return new Warning(fileName, Integer.parseInt(lineNumber), type, category, message, prio);
```
**Example:**
```
WARNING;66;BadPractice;There is two consecutive 'RUN'. Consider chaining them with '\' and '&&'
ENHANCEMENT;[94mBestPractice;Consider removing APT cache;'rm -rf /var/lib/apt/lists/*' after line 61
ENHANCEMENT;10;Immutability;A version should be specified for the package 'apt-transport-https' in order to improve immutability
```

---
whale-linter output parsing using bash

**Command:** parse-whale-linter.sh fileName > whale-linter.warnings

**bash parsing script:** parse-whale-linter.sh
```
#!/bin/bash
input=$1
while read -r line || [[ -n "$line" ]]; do
  # Skip lines
  if [[ ! $(echo $line | grep ':' | wc -l) -gt 0 ]]; then
    continue
  fi
  # Check for priority change and get
  if [[ $(echo $line | grep -E '[\[0-9m]*CRITICAL\s*:' | wc -l) -gt 0 ]]; then
    priority='CRITICAL'
    continue
  elif [[ $(echo "$line" | grep -E '[\[0-9m]*WARNING\s*:' | wc -l) -gt 0 ]]; then
    priority='WARNING'
    continue
  elif [[ $(echo "$line" | grep -E '[\[0-9m]*ENHANCEMENT\s*:' | wc -l) -gt 0 ]]; then
    priority='ENHANCEMENT'
    continue
  fi
  # Parse message lines
  #   21: [93mBadPractice : [0mThere is two consecutive 'RUN'. Consider chaining them with '\' and '&&'
  lineNumber=${line%%:*}
  lineNumber=$(echo $lineNumber | sed 's/^\s+//')
  if [ -z "$lineNumber" ]; then
    continue
  fi
  # if lineNumber is NOT a number, it is the category instead
  if [[ $lineNumber =~ ^[0-9]+$ ]]; then
    category=${line#*:}
    category=${category%%:*}
    category=$(echo $category | sed 's/^\s*.[\[0-9m]*//')
  else
    category=$(echo $lineNumber | sed 's/^\s*.[\[0-9m]*//')
    lineNumber=0
  fi
  msg=${line##*:}
  msg=$(echo $msg | sed 's/^\s*.[\[0-9m]*//')
  echo "${priority};${lineNumber};${category};${msg}"
done < "$input"
```
