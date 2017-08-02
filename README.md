# Chef Development Style and Standards
The purpose of this guide is to outline the standards and best practices for developing Chef cookbooks and related items.

The guide outlines the common attributes shared between TI Auto, ETS, and Malvern, as well as governance specific to the individual organizations.

The original wiki document for this document can be found here: https://wiki.ucern.com/x/jzUnY

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
|ETS | https://github.cerner.com/ets/ets_chef-repo |
|CWXTI Auto | https://github.cerner.com/CWXAutomation |
|Malvern | https://github.cerner.com/cernerhs-cwx-ti |

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
|CernerWorks Tech Stack Role Cookbook | cwx_<tech_stack>_role | cwx_linux_role |
|CernerWorks Tech Improvement Automation Developed Cookbook | cwxtiauto_<cookbook_name> | cwxtiauto_logdir |
|Client Ops Developed Cookbook | clientops_<cookbook_name> | clientops_zabbix |

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

##### README.md
:white_check_mark: **Example README.md**

https://github.cerner.com/CWxAutomation/cwxtiauto_java_patching/blob/master/README.md

:white_check_mark: **Example README.md with Custom Resources**

https://github.cerner.com/CWxAutomation/cwxtiauto_lvm/blob/dev/README.md

##### Headers
:white_check_mark: **Example**
```
# This file is managed by the cwxtiauto_example Chef cookbook and manual changes will be erased on the next chef-client run
# More information on this cookbook can be found at https://github.cerner.com/CWxAutomation/cwxtiauto_example
```

### Behavior

#### Attribute Handling

##### Standard Attributes
- Standard attributes are defined by roles in the Chef environment and assigned to nodes. More information on currently developed standard attributes can be found at the cwx_chef_config repository (https://github.cerner.com/CWxAutomation/cwx_chef_config).
    - **node['cwx']['tech_stack']**
        - Used when flexing based on the technology stack of the given node.
        - Set via a role cookbook.
        - Valid values:

        | Valid Value | Details |
        | ----------- | ------- |
        |724midtier | 724 DTV (Level 2) midtier node|
        |724roapp | 724 Read Only (Level 1) Millennium application node|
        |724rodb | 724 Read Only (Level 1) Millennium database node|
        |camm | CAMM node|
        |gluster | gluster node|
        |ibus | iBus node|
        |millapp | Millennium application node|
        |millcombo | Millennium application/database node (full stack)|
        |milldb | Millennium database node|
        |nfs | NFS node|
        |p2 | P2Sentinel node|
        |was | WAS on Linux node|

    - **node['cwx']['region']**
        - Used when flexing based on the region of a given node.
        - Set via role cookbook.
        - Valid values:

        | Valid Value | Details |
        | ----------- | ------- |
        |clientops_australia | CernerWorks Client Ops Australia Region|
        |clientops_canada | CernerWorks Client Ops Canada Region|
        |clientops_central | CernerWorks Client Ops Central US Region|
        |clientops_federal | CernerWorks Client Ops Federal Region|
        |clientops_france | CernerWorks Client Ops France Region|
        |clientops_global | CernerWorks Client Ops Global Region (Generic)|
        |clientops_midwest | CernerWorks Client Ops Midwest US Region|
        |clientops_northatlantic | CernerWorks Client Ops Northatlantic US Region|
        |clientops_southeast | CernerWorks Client Ops Southeast US Region|
        |clientops_uk | CernerWorks Client Ops UK Region|
        |clientops_west | CernerWorks Client Ops West US Region|
        |ehosting | CernerWorks eHosting Region|
        |internal | CernerWorks Internal Region|

    - **node['server']['data_center']**
        - Used when flexing based on the data center of a given node.
        - Set via role cookbook.
        - Valid values:

        | Valid Value | Details |
        | ----------- | ------- |
        |AUS_IR | Australia IR Data Center|
        |AUS_LDR | Australia LDR Data Center|
        |CD_Q9 | Canada Q9 Data Center|
        |CD_SUNGARD | Canada Sungard Data Center|
        |FRA_PA4 | France PA4 Data Center|
        |INJAZAT | Injazat (UAE) Data Center|
        |KC1 | KC1 Data Center|
        |KC2 | KC2 Data Center|
        |KC3 | KC3 Data Center|
        |KC4 | KC4 Data Center|
        |KC5 | KC5 Data Center|
        |KC6 | KC6 Data Center|
        |KC7 | KC7 Data Center|
        |LS1 | LS1 Data Center|
        |LS2 | LS2 Data Center|
        |LS3 | LS3 Data Center|
        |LS4 | LS4 Data Center|
        |LS5 | LS5 Data Center|
        |LS6 | LS6 Data Center|
        |LS7 | LS7 Data Center|
        |UK_CL3 | UK CL3 Data Center|
        |UK_LD5 | UK LD5 Data Center|

    - **node['server']['onsite']**
        - Used when flexing based on whether a node is located at a client site.
        - Set via role cookbook.
        - Valid values:

        | Valid Value | Details |
        | ----------- | ------- |
        |true | Node is located at a client site|
        |nil | Node is not located at a client site|

##### Feature Attributes
- If a cookbook will perform an action that is client-impacting or would normally require a scheduled event with the client, use a feature attribute to prevent that action from running without explicit specification.

#### Logging
Standard logging configurations for all executions of 'chef-client' are managed by the cwx_linux_role cookbook (https://github.cerner.com/CWxAutomation/cwx_linux_role).

:warning: Modifying the logging configuration manually may results in incomplete logging and failure to properly execute 'chef-client' on the server.

##### Location
Current log for 'chef-client' executions is **/var/log/chef/chef-client.log** on servers within the **cernerworks_rho** Chef organization.
```
view /var/log/chef/chef-client.log
```

##### Configuration
Chef reads logging configurations from the **client.rb** file located in the **/etc/chef** directory.

**Items**

The following configuration items are found within the **client.rb** file.

|Item|
|----|
|chef_server_url|
|validation_client_name|
|log_location|
|log_level|
|node_name|
|trusted_certs_dir|

For detailed information on the configuration items, please see the official Chef [documentation](https://docs.chef.io/config_rb_client.html).

**Example**

The following **client.rb** is a correct example of what to expect on a server managed by Chef.
```
chef_server_url "https://cwxtiauto01.cernerasp.com/organizations/cernerworks"
validation_client_name "chef-validator"
log_location "/var/log/chef/chef-client.log"
log_level :info
node_name "cwimmo72402.cernerasp.com"
trusted_certs_dir "/etc/chef/trusted_certs"
```

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
|weekly | Logs are rotated weekly. |
|rotate 4 | Logs are kept four weeks. |
|compress | Logs are compressed when located. |

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

### Style Guide

#### Naming Convention

##### Cookbook
- Preface cookbook names with **cerner_** to remove conflicts with open source or community names.

#### Process

##### Source Control
- Code and Chef Objects (data bags, roles, environments, etc.) must be source controlled in Cerner GitHub.

#### Releasing
- Follow the ETS Operation Engineering Low Risk Release, Documentation and Testing Strategy development process.
    - [Operational Engineering Low Risk Release, Documentation and Testing Strategy](https://wiki.ucern.com/display/ETSrvcs/Operational+Engineering+Low+Risk+Release%2C+Documentation+and+Testing+Strategy)
- Anyone can release, but only certain architects can perform merge and tag operations in the release.
    - Architect certification TBD.

</details>

<details><summary>Malvern</summary>

### Style Guide

#### Naming Convention

##### ALT Allowable Values
- Refer to this link to review the list of allowable values in regards to naming conventions and mnemonics
    - https://wiki.ucern.com/pages/viewpage.action?spaceKey=HSALT&title=Allowable+Values

##### Attributes
- Attributes must be created using the following format (use underscores for spaces):

:white_check_mark: **Examples:**

```
default['cookbook_name']['attribute_1'] = 'some_value'
default['cookbook_name']['attribute_2'] = 'another_value'
```

##### Roles
- Roles must be created using the following format:

```
[app_Id]_role_[server_role]
```

:white_check_mark: **Examples:**

```
sf_role_primary_server
s2_role_application_server
hep_role_ems_primary_server
hosting_role_windows_2012_base_os
```

##### Environment
- Environment names should using the following format:

```
{app_Id}_{release_version}
```

:white_check_mark: **Examples:**

```
Soarian Financials 4.1: sf_4_1
Soarian Financials 4.2.100: sf_4_2_100
HEP 1.2 SP2: hep_1_2_sp2
S2 4.2: s2_4_2
```
##### Methods
- Method names should not be prefixed with verbs (get, set, is).
- Boolean methods should end with **?**.

#### Documentation

##### Metadata.rb
- Metadata.rb should contain the following sections.
    - **Version**
        - The cookbook version must be incremented appropriately when updates have been committed and the cookbook is ready to be released.
    - **Contact Information**
        - Developer or development team.
    - **Issue Tracking**
        - Link to the JIRA queue used to log issues.
    - **Source_URL**
        - Link to the cookbook's source control repository.

#### Environments

##### Environment Files
- Environment files will contain information and data releate to a major release cycle of an application.
- Environment files created for hot fix and smaller service pack releases will not be supported.

##### Versioning
- Cookbook version will be defined within these environment files for that specific release.

#### Data Bags
- All dynamic properties must be stored in the application's specific data bag.
- Data bag names for applications should follow the ALT standard of allowable values.

:white_check_mark: **Examples:**

```
C:\chef-repo\data_bags\hep\
C:\chef-repo\data_bags\sf\
C:\chef-repo\data_bags\s2\
C:\chef-repo\data_bags\slpa\
```

#### Encrypted Values
- All passwords stored within data bags, environment files, or source code of cookbooks must be encrypted.
- Third party encryption applications or libraries should not be used. Encrypted data bag items within Chef must be used for encrypting secure items.
- The keys used to encrypt the encrypted data bags must be stored in the following locations:
    - Windows: **C:\chef\keys**
    - Linux/Unix: **/etc/chef/keys**

### Behavior

#### Attribute Handling

##### Environment Attributes
- Environment files must include the following base set of default attributes.

    |Attribute |Description |
    |--------- |----------- | 
    |['common'] | Top level key for common environment level attributes.|
    |['ad_domain_name'] | NetBIOS name for Active Directory (ex. RESDM50)|
    |['ad_domain_dns_suffix'] | DNS suffix of Active Directory domain (ex. resdm50.siemensmedasp.com)|
    |['dns_suffix_search_order'] | DNS suffix search list for DNS configuration|
    |['enable_media_authentication'] | Boolean variable which is set when media server requires authentication|
    |['media_url'] | Artifactory Media URL with username and encrypted password|
    |['media_api_key'] | Artifactory REST API Key|
    |['smtp_host'] | SMTP Mail Server Hostname|
    |['smtp_port'] | SMTP Port for Mail Server|



#### Ruby Gems and External Utilities
- New development using Chef should always attempt to be written using resources from the Chef DSL or native Ruby libraries. 
- Third party Ruby gems usually provide support for multiple platforms and should be considered and reviewed first if the Chef environment does not provide a specific library. 
- Use of platform specific utilities or binaries should be used if no Ruby libraries are available.

#### Roles and Role Cookbooks
- Role cookbooks will be used in place of using server roles as role cookbooks provide greater flexibility compared to Chef roles. 
- Role cookbooks contain attributes, a run list or configuration for a specific role using recipes, and allows for cookbook versioning specific to that run list within the metadata.rb file.
- Default recipes should include run list of all recipes to manage the configuration policy of that server's role.

#### Internal Mode vs. On Demand
- Organize recipes that can be executed on-demand during an event or for configuration management.

:white_check_mark: **Example structure:**

- wsus_client
    - Recipes:
        - default (contains configuration elements that will be executed during every Chef client run)
            - Avoid use of this recipe in cookbooks as the default recipe name does not clearly define the purpose of the recipe.
        - configure_wsus_client (contains configuration elements that will configure the WSUS client on a server)
            - This can be added using include_recipe in the default recipe to ensure the WSUS client is configured properly on each interval run
        - patch_system (contains the process and configuration elements required to patch and reboot a system)
            - This recipe would be executed on demand during an event.  If this is not defined in the default recipe, it will not execute during a Chef client run until it is explicitly defined.


- The Chef client can be called by passing in the **--override-runlist** (or **-o**) argument to execute a specific cookbook or recipe.
- For example, based on the example cookbook, the Chef client service could be stopped first, then the chef-client -o recipe[wsus_client::patch_system] command could be executed on a server on demand during a scheduled event to patch the system.
- When executing a on demand job during an event, interval mode can be turned off by stopping the Chef client service.  Restart the Chef client service to enable interval mode.

### Process

#### Source Control
- For common, shared cookbooks, one repository per cookbook must be used.
- Solutions can store all cookbooks in one repository.

#### Code Review

##### RHO Deployments
- RHO Chef administrators must be included in the code review before they are uploaded to the production environment.
- Refer to the following guide for submission acceptance criteria and process for the RHO Chef Enterprise system.
    - https://wiki.ucern.com/display/HSCernerWorks/RHO+Chef+Automation+Submission+Process+and+Requirements


#### Releasing
- Releases must be tagged if using GitHub for source control.

</details>