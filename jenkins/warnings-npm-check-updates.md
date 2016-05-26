# Jenkins Warning plugin parser for npm-check-updates

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

Updated for version 2.6.5

---

**Name:** Custom-npm-check-updates

**Link Name:** Outdated Modules

**Trend report name:** Outdated NPM Modules

**Regular Expression:** `((?:\w|[+-])+)\s+\^([0-9]+\.[0-9]+\.[0-9]+)\s+→\s+\^([0-9]+\.[0-9]+\.[0-9]+)`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning

String module = matcher.group(1)
String versionOld = matcher.group(2)
String versionNew = matcher.group(3)

String msg = "Outdated module found: '" + module + "', version '" + versionOld + "', available: '" + versionNew + "'"

return new Warning('package.json', 0, "Outdated Module", 'OUTD1', msg);  
```
**Example:**
```
jsonwebtoken  ^5.5.4  →  ^7.0.0 
node-jose     ^0.7.0  →  ^0.8.0 
shelljs       ^0.5.3  →  ^0.7.0 
validator     ^4.5.0  →  ^5.2.0 
```
