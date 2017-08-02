# Chef Development Style and Standards
The purpose of this guide is to outline the standards and best practices for developing Chef cookbooks and related items.

The guide outlines the common attributes shared between TI Auto, ETS, and Malvern, as well as governance specific to the individual organizations.

<details><summary>Common</summary>

### Style Guide

#### Naming Convention
:warning: All names should contain underscores instead of hyphens.

##### Cookbook
- The cookbook name should be clear and describe the unit of configuration the cookbook is managing.

##### Recipes
- The recipe name should describe the purpose of the recipe.

##### Attribute
- The attribute name should be lowercase.
- Attributes within a cookbook must have the cookbook name first and the attribute name second. This will avoid potential override conflicts with attributes in other cookbooks or environment files.

##### Role
- The role name should be descriptive and named according to the value set within the role.

#### Documentation

##### README.md
- All cookbooks should contain a README.md file with the following sections.
    - Recipes to consume
    - Requirements
        - Required platform
        - Chef version
        - Required cookbooks
    - Resources
        - Custom resource actions
        - Parameters
        - Usage
    - Default attributes
    - Usage
    - Testing instructions
    - Issue tracking
        - Link to JIRA queue used to log issues related to the cookbook
    - uCern
        - Link to related uCern group and wiki page
    - Contributing
        - Steps necessary to contribute to the cookbook

##### Headers
- All Chef managed files and templates should have a header indicating that the file is managed by Chef with a link to the owning cookbook's source control repository.

##### Artifacts
- All cookbooks must be uploaded to Cerner Chef Supermarket and Chef environments and Spork.
- All Supermarket and Chef uploads must occur from Electric Commander.

#### Formatting
- Use two spaces for indentation.
- Limit lines to 120 characters.
- Use snake case for naming variables, methods, files, and directories.
- Use camel case for naming classes and modules.
- Code comments should be succinct. Remove irrelevant comments from code.

### Behavior

#### Idempotency
- All recipes, cookbooks, and custom resources should be safe to run multiple times in sequence with identical results and should only act when necessary.

#### Attribute Handling

##### Validation
- Validate attributes prior to use.
    - Raise an exception if the required attribute is in an invalid format.
    - Raise an exception or log a warning and return (depending on the criticality) if the required attribute is missing.

##### Default Attributes
- Default attributes should be used unless a higher level is required.
    - It is necessary to allow consumers to override attributes in roles, environment, or wrapper cookbooks.

#### Configuration

##### System Configuration
 - When altering system configuration, use the conf.d custom configuration methodology.
 - http://blog.siphos.be/2013/05/the-linux-d-approach/

##### Local Mode
- When executing the Chef client in local mode, a separate configuration file must be created instead of using the standard client.rb in the Chef configuration directory.

##### Configuration Repositories
| Organization | Repo location |
| ------------ | ------------- |
| ETS | https://github.cerner.com/ets/ets_chef-repo |
| CWXTI Auto | https://github.cerner.com/CWXAutomation |
| Malvern | https://github.cerner.com/cernerhs-cwx-ti |

### Process

#### Static Code Analysis/Linting
Use cookstyle via rubocop to validate cookbooks are following established Chef/Ruby coding style guidelines.

#### Testing

##### Unit
- Leverage ChefSpec for unit testing when possible
- Libraries should be unit tested.

##### Integration
- Test local through frameworks such as Test Kitchen.
- Inspec tests are preferred, but serverspec is also acceptable.
- Dependency on external services should be avoided. Leverage fixture cookbooks as needed.

##### Pull Request
- Unit testing execution and integration testing via openstack is recommended.

#### Source Control
- All code and configuration must be committed to a source control repository.
- Source controlled configuration will be uploaded to Chef via a CI/CD tool such as Jenkins or Spork.

#### Code Review

##### Dev or non-master branch
- Two +1 with at least one architect reviewing.
- Test evidence can be provided by the developer.

##### Prod or master branch
- Two +1 with at least one architect reviewing.
- Test evidence is required and must be executed by a non-contributor to the code changes.

#### Releasing
- Use semantic versioning for applying version numbers.
    - http://semver.org/

</details>

<details><summary>TI Auto</summary>

### Style Guide

#### Naming Convention

##### Cookbook
- Cookbooks should be named according to the following table.
| Cookbook Type | Naming Standard | Example Name |
| ------------- | --------------- | ------------ |
| CernerWorks Tech Stack Role Cookbook | cwx_<tech_stack>_role | cwx_linux_role |
| CernerWorks Tech Improvement Automation Developed Cookbook | cwxtiauto_<cookbook_name> | cwxtiauto_logdir |
| Client Ops Developed Cookbook | clientops_<cookbook_name> | clientops_zabbix |

##### Roles
- All roles should be created in the cwx_chef_config GitHub repository (https://github.cerner.com/CWxAutomation/cwx_chef_config) within the roles directory.

:white_check_mark: **Example:** roles/example_region.rb
```
name "example_region"

description "Example Region Nodes"

default_attributes(
  cwx: {
    region: 'example'
  }
}
```

##### Conf.d
```
zzz-cerner-<subsys name>.<ext>
```
:warning:Some services on Red Hat Enterprise Linux do not support the conf.d convention.

#### Documentation

##### Readme.md

##### Headers

### Behavior

#### Attribute Handling

##### Standard Attributes

##### Feature Attributes

#### Logging

##### Location

##### Configuration

##### Rotation
Logs are rotated every week using [logrotate](https://linux.die.net/man/8/logrotate) and kept for four weeks as compressed files.

**Location**

The logrotate configuration file for Chef logs is found within the **/etc/logrotate.d/chef** file.

```
view /etc/logrotate.d/chef
```

**Configuration**

The following configuration items are currently utilized in the logrotate configuration.

| Item | Description |
| ---- | ----------- |
| weekly | Logs are rotated weekly. |
| rotate 4 | Logs are kept four weeks. |
| compress | Logs are compressed when located. |

**Example**

An example of a correct logrotate configuration file for Chef is as follows.

```
/var/log/chef/chef-client.log {
        weekly
        rotate 4
        compress
}
```
:heavy_exclamation_mark: Manual modifications to any of the logging files managed by Chef will be overwritten when 'chef-client' is ran.

</details>

<details><summary>ETS</summary>

</details>

<details><summary>Malvern</summary>

</details>