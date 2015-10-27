Designed to be an expandable framework and to support 100% automation of Puppet module workflow from repository creation to creation of workflow and eventually deployment into production. Generally follows Puppet best practices

Puppet Module user workflow example:

# Create git repository for Puppet module
# Create new Puppet module using module skeleton
# Write and document Puppet module by updating parts of skeleton and adding code
#  Commit and push Puppet module
# Jenkins job will be running a script to monitor github organization and automatically create new workflows for any new Puppet module repositories found
# User will get an email if new Puppet module fails any of the tests
 
