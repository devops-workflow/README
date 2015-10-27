Designed to be an expandable framework and to support 100% automation of Puppet module workflow from repository creation to creation of workflow and eventually deployment into production. Generally follows Puppet best practices

Puppet Module user workflow example:

1) Create git repository for Puppet module
2) Create new Puppet module using module skeleton
3) Write and document Puppet module by updating parts of skeleton and adding code
4) Commit and push Puppet module
5) Jenkins job will be running a script to monitor github organization and automatically create new workflows for any new Puppet module repositories found
6) User will get an email if new Puppet module fails any of the tests
 
