# Jenkins Warning plugin parser for hyperclair

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

This is a 2 part parser:
- Parsing json output into line output
 - Format: Filename:Line Number:Category:Type:Priority:Message
- Parsing line output with Warning plugin

---

**Name:** Custom-hyperclair

**Link Name:** hyperclair

**Trend report name:** hyperclair

**Regular Expression:** `^"([^;]+);([^;]+);([^;]+);([^;]+);([^;]+);(.*)"$`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning
import hudson.plugins.analysis.util.model.Priority

String fileName = matcher.group(1)
String lineNumber = matcher.group(2)
String category = matcher.group(3)
String type = matcher.group(4)
String priority = matcher.group(5)
String message = matcher.group(6)
Priority prio = Priority.NORMAL

if (priority == "High") {
  prio = Priority.HIGH
} else if (priority == "Medium") {
  prio = Priority.NORMAL
} else {
  prio = Priority.LOW
}

return new Warning(fileName, Integer.parseInt(lineNumber), type, category, message, prio);
```
**Example:**
```
"intel/fenix:0.0-636-3edcab1;0;tiff - 4.0.3-12.3+deb8u1;CVE-2015-7313;Unknown;Reference: https://security-tracker.debian.org/tracker/CVE-2015-7313"
"intel/fenix:0.0-636-3edcab1;0;tiff - 4.0.3-12.3+deb8u1;CVE-2015-7554;High;Reference: https://security-tracker.debian.org/tracker/CVE-2015-7554"
"intel/fenix:0.0-636-3edcab1;0;tiff - 4.0.3-12.3+deb8u1;CVE-2015-8668;High;Reference: https://security-tracker.debian.org/tracker/CVE-2015-8668"
"intel/fenix:0.0-636-3edcab1;0;tiff - 4.0.3-12.3+deb8u1;CVE-2016-3186;Medium;Reference: https://security-tracker.debian.org/tracker/CVE-2016-3186"
```

---
JSON parsing using jq

**Command:** jq -f parse-hyperclair.jq json-file | sort -u | tail -n+2 > warning-file

**jq parsing script:** parse-hyperclair.jq
```
#
# jq filter for parsing hyperclair output into 1 liners that Jenkins Warning plugin can parse
#
# Written for jq 1.5
# Author: Steven Nemetz
#
# Output format:
#  filename;line number;category;type;priority;message
#
# Set to variable, then reference after if
# Got lost because of piping a lower level
#.filename = "\(.ImageName):\(.Tag)"
# | (.Layers[].Layer.Features[]
#
# First line is bad. So pipe to
# Will create duplicate and bad lines
# Need to pipe output to cleanup or figure out better way to do this
# | sort -u | tail -n+2
"\(.ImageName):\(.Tag);0;" +
(.Layers[].Layer.Features[]
  | if .Vulnerabilities then
      "\(.Name) - \(.Version);" +
      (.Vulnerabilities[] | "\(.Name);\(.Severity);" +
      if .Message then
        "\(.Message) "
      else
        ""
      end +
      "Reference: \(.Link)")
    else
      ""
    end
 )
```
