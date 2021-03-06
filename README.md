# yum-mysql-community Cookbook

[![Build Status](https://travis-ci.org/chef-cookbooks/yum-mysql-community.svg?branch=master)](http://travis-ci.org/chef-cookbooks/yum-mysql-community) [![Cookbook Version](https://img.shields.io/cookbook/v/yum-mysql-community.svg)](https://supermarket.chef.io/cookbooks/yum-mysql-community)

The yum-mysql-community cookbook takes over management of the default repository ids shipped with mysql*-community-release. It allows attribute manipulation of `mysql-connectors-community`, `mysql56-community`, and `mysql57-community-dmr`.

## Requirements

### Platforms

- RHEL/CentOS and derivatives
- Fedora

### Chef

- Chef 13.0+

### Cookbooks

- none

## Attributes

The following attributes are set by default

```ruby
default['yum']['mysql-connectors-community']['repositoryid'] = 'mysql-connectors-community'
default['yum']['mysql-connectors-community']['description'] = 'MySQL Connectors Community'
default['yum']['mysql-connectors-community']['baseurl'] = 'http://repo.mysql.com/yum/mysql-connectors-community/el/$releasever/$basearch/'
default['yum']['mysql-connectors-community']['gpgkey'] = 'https://raw.githubusercontent.com/rs-services/equinix-public/master/cookbooks/db_mysql/files/centos/mysql_pubkey.asc'
default['yum']['mysql-connectors-community']['failovermethod'] = 'priority'
default['yum']['mysql-connectors-community']['gpgcheck'] = true
default['yum']['mysql-connectors-community']['enabled'] = true
```

```ruby
default['yum']['mysql56-community']['repositoryid'] = 'mysql56-community'
default['yum']['mysql56-community']['description'] = 'MySQL 5.6 Community Server'
default['yum']['mysql56-community']['baseurl'] = 'http://repo.mysql.com/yum/mysql56-community/el/$releasever/$basearch/'
default['yum']['mysql56-community']['gpgkey'] = 'https://raw.githubusercontent.com/rs-services/equinix-public/master/cookbooks/db_mysql/files/centos/mysql_pubkey.asc'
default['yum']['mysql56-community']['failovermethod'] = 'priority'
default['yum']['mysql56-community']['gpgcheck'] = true
default['yum']['mysql56-community']['enabled'] = true
```

```ruby
default['yum']['mysql57-community-dmr']['repositoryid'] = 'mysql57-community-dmr'
default['yum']['mysql57-community-dmr']['description'] = 'MySQL 5.7 Community Server Development Milestone Release'
default['yum']['mysql57-community-dmr']['baseurl'] = 'http://repo.mysql.com/yum/mysql56-community/el/$releasever/$basearch/'
default['yum']['mysql57-community-dmr']['gpgkey'] = 'https://raw.githubusercontent.com/rs-services/equinix-public/master/cookbooks/db_mysql/files/centos/mysql_pubkey.asc'
default['yum']['mysql57-community-dmr']['failovermethod'] = 'priority'
default['yum']['mysql57-community-dmr']['gpgcheck'] = true
default['yum']['mysql57-community-dmr']['enabled'] = true
```

## Recipes

- mysql55 - Sets up the mysql55-community repository on supported platforms

```ruby
  yum_repository 'mysql55-community' do
    mirrorlist 'https://repo.mysql.com/yum/mysql-5.5-community/el/$releasever/$basearch/'
    description ''
    enabled true
    gpgcheck true
  end
```

- mysql56 - Sets up the mysql56-community repository on supported platforms

```ruby
  yum_repository 'mysql56-community' do
    mirrorlist 'https://repo.mysql.com/yum/mysql-5.6-community/el/$releasever/$basearch/'
    description ''
    enabled true
    gpgcheck true
  end
```

- connectors - Sets up the mysql-connectors-community repository on supported platforms

## Resources

- yum_mysql_community_repo - Creates /etc/yum.repos.d/mysql-community repo file with enabled repos on supported platforms

```ruby
  yum_mysql_community_repo 'default' do
    version '8.0'
    gpgcheck true
    mysql_community_server true
    mysql_connectors_community true
    mysql_tools_community true
    mysql_tools_preview false
    mysql_cluster_community false
  end
```

## Usage Example

To disable the mysql-community-dmr repository through a Role or Environment definition

```ruby
default_attributes(
  :yum => {
    :mysql57-community-dmr => {
      :enabled => {
        false
       }
     }
   }
 )
```

Uncommonly used repositoryids are not managed by default. This is speeds up integration testing pipelines by avoiding yum-cache builds that nobody cares about. To enable the mysql-community-dmr repository with a wrapper cookbook, place the following in a recipe:

```ruby
node.default['yum']['mysql57-community-dmr']['enabled'] = true
node.default['yum']['mysql57-community-dmr']['managed'] = true
include_recipe 'mysql57-community-dmr'
```

## More Examples

Point the mysql56-community repositories at an internally hosted server.

```
node.default['yum']['mysql56-community']['enabled'] = true
node.default['yum']['mysql56-community']['mirrorlist'] = nil
node.default['yum']['mysql56-community']['baseurl'] = 'https://internal.example.com/mysql/mysql56-community/'
node.default['yum']['mysql56-community']['sslverify'] = false

include_recipe 'mysql56-community'
```

## License & Authors

**Author:** Cookbook Engineering Team ([cookbooks@chef.io](mailto:cookbooks@chef.io))

**Copyright:** 2011-2016, Chef Software, Inc.

```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
