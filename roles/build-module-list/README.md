# build-module-list

Ansible role for building the `folio_modules` list of modules (used in `okapi-register-modules` and `okapi-tenant-deploy` roles).

Any settings already defined in the `folio_modules` variable (e.g. from a variables file) will be merged into the list generated by this role, based on module name. See the `okapi-register-modules` role for the format of the `folio_modules` list.

The list can be generated from one of two sources:

* Install files located on the control host or at a URL
* A set of module descriptors generated by a Stripes build (_not yet implemented_)

_Note: this feature is not yet implemented_. The role will optionally use Okapi's dependency resolution to calculate the full list of modules that need to be deployed and enabled. It does not do the module deployment and enablement, however -- for that functionality, see the `okapi-tenant-deploy` role.

Sample install files are included in the `files` directory.

## Prerequisites (if using Okapi dependency resolution _not yet implemented_)

* A running Okapi system with:
  * Registered module descriptors for all modules required to meet dependencies. Module descriptors for modules to be deployed must contain a launch descriptor telling Okapi how to deploy the module (you can use the `okapi-pull` role to populate the module registry).

## Usage

Invoke the role in a playbook, e.g.:

```yaml
- hosts: my-folio-test
  roles:
    - role: build-module-list
      deploy_url: https://rawgit.com/folio-org/platform-complete/q3-2018/okapi-install.json
      enable_url: https://rawgit.com/folio-org/platform-complete/q3-2018/stripes-install.json
```

## Variables

```yaml
---
# defaults
okapi_port: 9130
okapi_url: "http://{{ ansible_default_ipv4.address }}:{{ okapi_port }}"

# Define or override one or more of these variables to post install JSON to Okapi.
# Note that if the x_url variables are defined, they will override the x_file variables

# File with modules for Okapi to enable and deploy
deploy_file: okapi-install.json

# URL for install file with modules for Okapi to enable and deploy
# deploy_url:

# File with modules for Okapi to enable only (no deployment)
enable_file: stripes-install.json

# URL for install file with modules for Okapi to enable only (no deployment)
# enable_url:
```
