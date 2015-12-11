Tools are being built into the workflow to support reading control files from the code repository to modify the workflow or Jenkins UI.

All control files reside under ci_data

### Valid Control Files
* sidebar-links.txt
 * This allows configuring additional links on the sidebar to link to other related data sources
* desc-header.txt
 * Adds a header to the Project's description
* desc-body.txt
 * Replaces the body of the Project's description
* desc-footer.txt
 * Adds a footer to the Project's description
* email.properties
 * Allow some control over the email notifications - Work in progress
* jjb/
 * Directory for yaml files to interact directly with Jenkins Job Builder - Work in progress

### Sample Files
* [testing-repo](https://github.com/devops-workflow/testing-repo) Contains sample files used to test these features

