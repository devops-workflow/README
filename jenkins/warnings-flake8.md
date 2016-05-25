# Jenkins Warning plugin parser for flake8

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

---

**Name:** Custom-Flake8

**Link Name:** Flake8

**Trend report name:** Flake8

**Regular Expression:** `^(.*):([0-9]+):([0-9]+): ([A-Z])(I?[0-9]*) (.*)$`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning
import hudson.plugins.analysis.util.model.Priority

// Code to description mapping table
def codes = [
  B:[desc:'builtins', prio:Priority.NORMAL ],
  B9:[desc:'Blind Except', prio:Priority.NORMAL ],
  C:[desc:'mccabe', prio:Priority.LOW ],
  C1:[desc:'Coding', prio:Priority.LOW ],
  C8:[desc:'Commas, Copyright', prio:Priority.LOW ],
  C9:[desc:'McCabe Complexity', prio:Priority.LOW ],
  D:[desc:'pep257', prio:Priority.NORMAL ],
  // D0 Deprecated
  E:[desc:'pep8 errors', prio:Priority.HIGH ],
  F:[desc:'PyFlakes', prio:Priority.NORMAL ],
  FI:[desc:'Future Import', prio:Priority.NORMAL ],
  F4:[desc:'Future', prio:Priority.NORMAL ],
  H:[desc:'hacking', prio:Priority.NORMAL ],
  I:[desc:'import-order, tidy-imports', prio:Priority.NORMAL ],
  I0:[dec:'isort', prio:Priority.NORMAL ],
  I2:[desc:'Tidy Imports', prio:Priority.NORMAL ],
  M:[desc:'mock, mastool', prio:Priority.NORMAL ],
  // M0 mastool, mock
  N:[desc:'pep8-naming', prio:Priority.NORMAL ],
  N8:[desc:'PEP-8 Naming', prio:Priority.NORMAL ],
  P:[desc:'string-format', prio:Priority.NORMAL ],
  // P1,P2,P3 String Format
  P0:[desc:'Exact Pin', prio:Priority.NORMAL ],
  R:[desc:'regex, rhsm-stylish', prio:Priority.NORMAL ],
  S:[desc:'strict', prio:Priority.NORMAL ],
  S1:[desc:'Snippets, Strict', prio:Priority.NORMAL ],
  T:[desc:'Print, ToDo', prio:Priority.LOW ],
  // T0 Print, ToDo
  // T1 Print
  T8:[desc:'Tuple', prio:Priority.NORMAL ],
  W:[desc:'pep8 warnings', prio:Priority.NORMAL ]
]
String fileName = matcher.group(1)
String lineNumber = matcher.group(2)
String code = matcher.group(4)
String type = matcher.group(5)
String message = matcher.group(6)
Priority prio = Priority.NORMAL

if (codes.containsKey(code)) {
  // Check for key = code + first digit
  code2 = code.concat(type[0])
  if (codes.containsKey(code2)) {
    category = codes[code2]['desc']
    prio = codes[code2]['prio']
  } else {
    category = codes[code]['desc']
    prio = codes[code]['prio']
  }
} else {
  category = code
}
return new Warning(fileName, Integer.parseInt(lineNumber), type, category, message, prio);
```
**Example:**
```

```
