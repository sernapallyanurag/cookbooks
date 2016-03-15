# yum-centos Cookbook
[![Build Status](https://travis-ci.org/chef-cookbooks/yum-centos.svg?branch=master)](http://travis-ci.org/chef-cookbooks/yum-centos) [![Cookbook Version](https://img.shields.io/cookbook/v/yum-centos.svg)](https://supermarket.chef.io/cookbooks/yum-centos)

The yum-centos cookbook takes over management of the default repositoryids that ship with CentOS systems. It allows attribute manipulation of `base`, `updates`, `extras`, `centosplus`, `contrib`, and `fasttrack`.

## Requirements
### Platforms
- CentOS

### Chef
- Chef 11+

### Cookbooks
- yum version 3.2.0 or higher

## Attributes
The following attributes are set by default

```ruby
default['yum']['base']['repositoryid'] = 'base'
default['yum']['base']['mirrorlist'] = 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os'
default['yum']['base']['description'] = 'CentOS-$releasever - Base'
default['yum']['base']['enabled'] = true
default['yum']['base']['managed'] = true
default['yum']['base']['gpgcheck'] = true
default['yum']['base']['gpgkey'] = 'file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-$releasever'
```

```ruby
default['yum']['contrib']['repositoryid'] = 'contrib'
default['yum']['contrib']['description'] = 'CentOS-$releasever - Contrib'
default['yum']['contrib']['mirrorlist'] = 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib'
default['yum']['contrib']['enabled'] = false
default['yum']['contrib']['managed'] = false
default['yum']['contrib']['gpgcheck'] = true
default['yum']['contrib']['gpgkey'] = 'file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-$releasever'
```

```ruby
default['yum']['extras']['repositoryid'] = 'extras'
default['yum']['extras']['description'] = 'CentOS-$releasever - Extras'
default['yum']['extras']['mirrorlist'] = 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras'
default['yum']['extras']['enabled'] = true
default['yum']['extras']['managed'] = true
default['yum']['extras']['gpgcheck'] = true
default['yum']['extras']['gpgkey'] = 'file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-$releasever'
```

```ruby
default['yum']['centosplus']['repositoryid'] = 'centosplus'
default['yum']['centosplus']['description'] = 'CentOS-$releasever - Centosplus'
default['yum']['centosplus']['mirrorlist'] = 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus'
default['yum']['centosplus']['enabled'] = false
default['yum']['centosplus']['managed'] = false
default['yum']['centosplus']['gpgcheck'] = true
default['yum']['centosplus']['gpgkey'] = 'file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-$releasever'
```

```ruby
default['yum']['updates']['repositoryid'] = 'updates'
default['yum']['updates']['description'] = 'CentOS-$releasever - Updates'
default['yum']['updates']['mirrorlist'] = 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates'
default['yum']['updates']['enabled'] = true
default['yum']['updates']['managed'] = true
default['yum']['updates']['gpgcheck'] = true
default['yum']['updates']['gpgkey'] = 'file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-$releasever'
```

```ruby
default['yum']['fasttrack']['repositoryid'] = 'fasttrack'
default['yum']['fasttrack']['description'] = 'CentOS-$releasever - fasttrack'
default['yum']['fasttrack']['mirrorlist'] = 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=fasttrack&infra=$infra'
default['yum']['fasttrack']['enabled'] = false
default['yum']['fasttrack']['managed'] = false
default['yum']['fasttrack']['gpgcheck'] = true
default['yum']['fasttrack']['gpgkey'] = 'file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-$releasever'
```

## Recipes
- default - Walks through node attributes and feeds a yum_resource
- parameters. The following is an example a resource generated by the
- recipe during compilation.

```ruby
  yum_repository 'base' do
    mirrorlist 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os'
    description 'CentOS-$releasever - Base'
    enabled true
    gpgcheck true
    gpgkey 'file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-$releasever'
  end
```

## Usage Example
To disable the CentOS Extras repository through a Role or Environment definition

```ruby
default_attributes(
  :yum => {
    :extras => {
      :enabled => {
        false
       }
     }
   }
 )
```

Uncommonly used repositoryids are not managed by default. This is speeds up integration testing pipelines by avoiding yum-cache builds that nobody cares about. To enable the CentOS Plus repository with a wrapper cookbook, place the following in a recipe:

```ruby
node.default['yum']['centosplus']['managed'] = true
node.default['yum']['centosplus']['enabled'] = true
include_recipe 'yum-centos'
```

## More Examples
Point the base and updates repositories at an internally hosted server.

```ruby
node.default['yum']['base']['enabled'] = true
node.default['yum']['base']['mirrorlist'] = nil
node.default['yum']['base']['baseurl'] = 'https://internal.example.com/centos/6/os/x86_64'
node.default['yum']['base']['sslverify'] = false
node.default['yum']['updates']['enabled'] = true
node.default['yum']['updates']['mirrorlist'] = nil
node.default['yum']['updates']['baseurl'] = 'https://internal.example.com/centos/6/updates/x86_64'
node.default['yum']['updates']['sslverify'] = false

include_recipe 'yum-centos'
```

## License & Authors
**Author:** Cookbook Engineering Team ([cookbooks@chef.io](mailto:cookbooks@chef.io))

**Copyright:** 2011-2015, Chef Software, Inc.

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