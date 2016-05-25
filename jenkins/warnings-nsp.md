# Jenkins Warning plugin parser for nsp (Node Security Project)

Custom parsers for the Warnings plugin can be added on the Jenkins global configuration page

---

**Name:** Custom-nsp

**Link Name:** Node Security Project

**Trend report name:** Detected Vulnerabilities

**Regular Expression:** `([\w+-]+)\s+([\w\.-@]+)\s+>= *([\w\.-@]+)\s+(.*)`

**Mapping Script:**
```
import hudson.plugins.warnings.parser.Warning  
import hudson.plugins.analysis.util.model.Priority

String msg = "Vulnerability found in '" + matcher.group(1) + "', version '" +
matcher.group(4) + "', patched in '" + matcher.group(3) + "'"

return new Warning('package.json', 0, "NSP Warning", 'VULN1', msg, Priority.HIGH);
```

**Example:**
```
Name Installed Patched Vulnerable Dependency
connect 2.7.5 >=2.8.1 nodesecurity-jobs > kue > express
qs 0.6.6 >= 1.x essis-management@1.0.319 > restler@3.2.2 > qs@0.6.6
```
