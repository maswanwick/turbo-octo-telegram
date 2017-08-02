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

##### Attribute

##### Role

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

##### Artifacts

#### Formatting

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