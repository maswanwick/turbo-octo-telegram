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

#### Attribute Handling

##### Validation

##### Default Attributes

#### Configuration

##### System Configuration
 - When altering system configuration, use the conf.d custom configuration methodology.
 - http://blog.siphos.be/2013/05/the-linux-d-approach/

##### Local Mode

##### Configuration Repositories
| Organization | Repo location |
| ------------ | ------------- |
| ETS | https://github.cerner.com/ets/ets_chef-repo |
| CWXTI Auto | https://github.cerner.com/CWXAutomation |
| Malvern | https://github.cerner.com/cernerhs-cwx-ti |

### Process

#### Static Code Analysis/Linting

#### Testing

##### Unit

##### Integration

##### Pull Request

#### Source Control

#### Code Review

##### Dev or non-master branch

##### Prod or master branch

#### Releasing
</details>

<details><summary>TI Auto</summary>

### Style Guide

#### Naming Convention

##### Cookbook

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