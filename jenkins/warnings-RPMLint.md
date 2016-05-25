# Jenkins Warning plugin parser for RPMLint

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

---

**Name:** Custom-RPMLint

**Link Name:** RPMLint

**Trend report name:** RPMLint

**Regular Expression:** `^([^ :]+):(?:(\d+):)? ([EW]): (\S+)(?: (.*))?$`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning
import hudson.plugins.analysis.util.model.Priority

String fileName = matcher.group(1)
String lineNumber = matcher.group(2)
String category = matcher.group(3)
String type = matcher.group(4)
String message = matcher.group(5)
Priority prio = Priority.NORMAL

if (category == "E") {
  prio = Priority.HIGH
  category = "error"
} else {
  category = "warning"
}

// Handle no line number
if (lineNumber == null || lineNumber.empty) {
  lineNumber = "0"
}

// Handle no message.
if (message == null || message.empty) {
  message = "-"
}

// % in type field can cause plugin to crash
type = type.replaceAll(/%/, '_')

return new Warning(fileName, Integer.parseInt(lineNumber), type, category, message, prio);
```
**Example:**
```

```
