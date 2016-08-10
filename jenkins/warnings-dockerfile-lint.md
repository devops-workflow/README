# Jenkins Warning plugin parser for dockerfile-lint

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

This is a 2 part parser:
- Parsing json output into line output
 - Format: Filename:Priority:Line Number:Category:Message
- Parsing line output with plugin

---

**Name:** Custom-dockerfile-lint

**Link Name:** dockerfile-lint

**Trend report name:** dockerfile-lint

**Regular Expression:** `^([^:]+):([^:]+):([^:]+):([^:]+):(.*)$`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning
import hudson.plugins.analysis.util.model.Priority

String fileName = matcher.group(1)
String priority = matcher.group(2)
String lineNumber = matcher.group(3)
String category = matcher.group(4)
//String type = matcher.group(4)
String message = matcher.group(5)
Priority prio = Priority.NORMAL
type = ""

if (priority == "error") {
  prio = Priority.HIGH
} else if (priority == "warn") {
  prio = Priority.NORMAL
} else {
  prio = Priority.LOW
}

return new Warning(fileName, Integer.parseInt(lineNumber), type, category, message, prio);
```
**Example:**
```
"Dockerfile:error:0:ARG:No ARGs are defined Reason=ARG statements are required for dynamic variables"
"Dockerfile:error:0:LABEL:No LABELs are defined Reason=Labels are required.... Reference=https://docs.docker.com/reference/builder/#label"
"Dockerfile:error:0:misc:Required LABEL name/key 'org.label-schema.build-date' is not defined Reference=http://label-schema.org/"
```

---
JSON parsing using jq

**Command:** jq --arg file Dockerfile -f parse-dockerfile-lint.jq json-file > warning-file

**jq parsing script:** parse-dockerfile-lint.jq
```
#
# jq filter for parsing dockerfile-lint output into 1 liners that Jenkins Warning plugin can parse
#
# Arguments:
#   file        relative path to Dockerfile or image
#
# Written for jq 1.5
# Author: Steven Nemetz
#
# Output format:
#  filename:priority:line number:category:message
#
.[].data?[]
 | "\($file):\(.level):" +
   if .line > 0 then
     "\(.line)"
   else
     "0"
   end +
   ":" +
   if .instruction then
     "\(.instruction)"
   elif .label then
     "\(.label)"
   else
     "misc"
   end +
   ":" +
   # Form message field by combining: message, description, reference_url array, and lineContent
   # Might reformat this later
   "\(.message)" +
   if .lineContent and (.lineContent | length > 0) then
     " Line=\(.lineContent)"
   else
     ""
   end +
   if .description then
     " Reason=\(.description)"
   else
     ""
   end +
   if .reference_url then
     " Reference=" + (.reference_url | join(""))
   else
     ""
   end
```
