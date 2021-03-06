
## 1.3.0

### Internal feature

* Better handling of archive clean up to stop the module leaving large installer files on the system: https://github.com/crayfishx/puppet-db2/pull/21

### 1.2.1

* Bumped dependency version for puppet/archive to < 2.0.0

## 1.2.0

### Feature

* Attributes for `db2_catalog_node`, `db2_catalog_database` and `db2_catalog_dcs` are now puppet properties, rather than parameters, which means Puppet will now enforce changes after creation and maintain the state of the catalog entries... eg:

```
Notice: /Stage[main]/Db2/Db2::Instance[db2test4]/Db2_catalog_node[DB2NODE1]/remote: remote changed 'db12.example.com' to 'db15.example.com'
```

This addresses the idempotency limitations of the resource types. Previously Puppet would use the attributes to create the catalog entry but if they were later changed in Puppet, it would not enforce the changes.  

### Bugfixes

* Bugfix: quotes for `comment` attribute and `params` attributes
* Bugfix: renamed `PARAMS` to `PARMS` for the catalog_dcs provider

### Other

* Fixed some validation issues with types
* Added more spec tests


### 1.1.1

Bugfix release.  This release addresses issues around the types and providers for managing DB2 catalog entries.

##### Fix errors when configuring a new instance

When a newly created instance is configured, puppet fails when
determining whether or not nodes, dcs and database entries exist because
the command "`db2 LIST * DIRECTORY`" will cause db2 to throw an error
because the directory doesn't exist.  There are various scenarios here
so the best way is to allow `db2_exec` to fail when being called from the
exists? method.  Therefore all exists? methods now call `db2_exec_nofail`

##### Fix idempotency problems with admin node entries

Previously, any nodes added as `admin => true` would cause puppet to try
and configure them again on the next run, thats because we need to
separate commands to list the configured nodes, list node directory and
list admin node directory.  This change adds support to the
`db2_catalog_node` provider to query both types of node entry and return a
unified result. https://github.com/crayfishx/puppet-db2/pull/10

##### server parameter for `db2_catalog_node` now optional

https://github.com/crayfishx/puppet-db2/pull/11


## 1.1.0

Feature release.  This release adds several types and providers for configuring DB2 catalog entries in an instance.  Also added is a `db2_instance` type for managing DB2 instances themselves, replacing the exec resource that previously existed in the `db2::instance` defined type.

New types and providers added:

* `db2_instance`
* `db2_catalog_node`
* `db2_catalog_database`
* `db2_catalog_dcs`

See the docs for usage instructions.

Reference: https://github.com/crayfishx/puppet-db2/pull/9


### 1.0.1

* Forge code quality (lint) release

# 1.0.0

* Issue #4 : Added 'port' parameter to `db2::instance`
* DB2 runtime installations and instances now supported (see docs)


### 0.3.1

* Issue #1 : Add ability to specify forcelocal for user creation

### 0.3.0

* BREAK: More sensible default for install_folder in db2::install, now uses universal instead of server_t

### 0.2.0

* Added `license_source` parameter, licenses can now either be added as strings with `license_content` or a file location given with `license_source`

### 0.1.2

* No functional changes, forge quality control release.

