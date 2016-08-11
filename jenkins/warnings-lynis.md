# Jenkins Warning plugin parser for lynis

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

This is a parser for lynis-report.dat

---

**Name:** Custom-lynis

**Link Name:** lynis

**Trend report name:** lynis

**Regular Expression:** `^(.+)\[\]=(.+)\|(.+)\|(.+)\|(.+)\|$`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning
import hudson.plugins.analysis.util.model.Priority

String fileName = matcher.group(2)
//String lineNumber = matcher.group(2)
//String category = matcher.group(3)
//String type = matcher.group(4)
String priority = matcher.group(1)
String message = matcher.group(3)
Priority prio = Priority.NORMAL
lineNumber = "0"
category = ""
type = ""

if (priority == "error") {
  prio = Priority.HIGH
} else if (priority == "warning") {
  prio = Priority.NORMAL
} else {
  prio = Priority.LOW
}

return new Warning(fileName, Integer.parseInt(lineNumber), type, category, message, prio);
```
**Example:**
```
warning[]=dockerfile||L|-|
suggestion[]=dockerfile|Don't use OpenSSH in container, use 'docker exec' instead|-|-|
warning[]=dockerfile|Don't use OpenSSH in container, use 'docker exec' instead|L|-|
```
