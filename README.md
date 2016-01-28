
This is a project for creating an automated, DevOps type, workflow for Puppet and Jenkins. Additions workflows could be added in the future. Designed to be an expandable framework, that should be able to support any workflow.

Tools used: Puppet, Jenkins, Jenkins Job Builder

Repository naming. Some of the automation is tied to the repository names. So, the naming is very important.
- Jenkins repositories need to start with "jenkins-"
- Puppet Module repositories need to start with "puppet_module_" followed by the module name
 
Currently puppet module jobs build sidebar links for the job. These can be specified by adding a "sidebar-links.txt" file to the top of the repository. The file format is: Title;url;icon filename

Start with the "test-environment" repository... the "git-sync" script will synchronize all repositories to the local directory (Note: sub-repos are NOT currently used, here).

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/devops-workflow/readme/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

