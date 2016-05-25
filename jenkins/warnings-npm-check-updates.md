# Jenkins Warning plugin parser for npm-check-updates

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

---

**Name:** Custom-npm-check-updates

**Link Name:** Outdated Modules

**Trend report name:** Outdated NPM Modules

**Regular Expression:** `([\w+-]+)\s+(\bv?(?:0|[1-9][0-9]*)\.(?:0|[1-9][0-9]*)\.(?:0|[1-9][0-9]*)(?:-[\da-z\-]+(?:\.[\da-z\-]+)*)?(?:\+[\da-z\-]+(?:\.[\da-z\-]+)*)?\b)\s+(\bv?(?:0|[1-9][0-9]*)\.(?:0|[1-9][0-9]*)\.(?:0|[1-9][0-9]*)(?:-[\da-z\-]+(?:\.[\da-z\-]+)*)?(?:\+[\da-z\-]+(?:\.[\da-z\-]+)*)?\b)\s+(\bv?(?:0|[1-9][0-9]*)\.(?:0|[1-9][0-9]*)\.(?:0|[1-9][0-9]*)(?:-[\da-z\-]+(?:\.[\da-z\-]+)*)?(?:\+[\da-z\-]+(?:\.[\da-z\-]+)*)?\b)\s+(.*)`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning

String msg = "Outdated module found: '" + matcher.group(1) + "', version '" + matcher.group(2) + "', available: '" + matcher.group(4) + "'"

return new Warning('package.json', 0, "Outdated Module", 'OUTD1', msg);  
```
**Example:**
```
Package                    Current   Wanted   Latest  Location  
essis-core                   1.0.0  1.0.185  1.0.185  essis-core  
essis-infrastructure-http    0.1.0   0.1.82   0.1.82  essis-infrastructure-http  
hp-logging                   0.1.0   0.1.74   0.1.74  hp-logging  
hp-orm                       0.1.0   0.1.17   0.1.17  hp-orm  
zbac                         1.0.0  1.0.210  1.0.210  zbac  
```
