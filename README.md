Description
===========

Cookbook to manage configuration settings of NexentaStor ZFS based storage systems

Requirements
============

Chef 0.10+

Platform
--------

* NexentaStor 3.x

Tested On:

* NexentaStor 3.1.3
* NexentaStor 3.1.3.5
* NexentaStor 3.1.4
* NexentaStor 3.1.4.1

Attributes
==========

See the `attributes/default.rb` for suggested values. Some attributes are used based on the node's version.

* `default["nexenta"]["nmv_log_rotate_default"]`        - Must be in format "XXm".
  Log rotation for nmv.log is not implemented in 3.x. The suggested value adds the same log rotation to nmv.log
  as the default for other log files which do have log rotation.
* `default["nexenta"]["nms_reporter_default"]`          - Must be either "enable" or "disable".
  The NMS reporter aggregates information once a week and mails this. The aggregation proces can hang the NMS 
  for a few minutes, impeding monitoring. The suggested value disables the NMS reporter.
* `default["nexenta"]["ses_check_flapping_default"]`    - Must be a value between 0 and 9.
  Only for NexentaStor 3.1.4 and higher. The ses-check runner occasionally gives false positives, for which
  this setting has been added. The suggested value ensures no false positives occur.

Templates
=========

* `System.erb`          - Dynamically add /etc/system settings based on NexentaStor version. In effect after reboot.
                        - Extra/changed settings from NexentaStor default:
                            - `zfs:l2arc_write_boost`           - Ensure the L2ARC is populated fast after a failover.
                            - `zfs:zfs_arc_shrink_shift`        - Adjusted to memory size. Not needed after 3.1.4.
* `Authorized_keys.erb` - Adds the partner key (if there is a ssh-bind) and any other key you specify.

Usage
=====

* Install chef-client on your NexentaStor ZFS based storage system (after the initial setup has completed).
* Configure the templates, attributes and files for your environment.
* Create a role for NexentaStor systems which uses the cookbooks default recipe.
* Add the newly created role to your NexentaStor systems.

License and Author
==================

- Author:: Brenn Oosterbaan (<boosterbaan@schubergphilis.com>)

Copyright:: 2013 Schuberg Philis
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
 
    http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.