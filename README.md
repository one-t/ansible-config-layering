This example attempts to recreate the configuration layering capability of Satellite 5. 
For this, role dependencies provide a method for layering configuration files.

The idea is that a playbook should call the most specific role needed, and this role will call the role it relies upon, which will in turn call its own dependencies. For example, each role contains a list of the files it will be copying down, as well as an empty list of files to skip:
common_config/defaults/main.yml

```yaml
---
# defaults file for common_config
common_config_skip: []
common_config_files:
  - etc/motd
  - etc/arbitrary.conf
```

The files are located within the role’s files directory. The task that actually handles copying the files is shown here:
`common_config/tasks/main.yml`
```yaml
---
# tasks file for common_config
- name: Copy Configuration Files
  copy:
    src: 'config_files/{{ item }}'
    dest: '/{{ item }}'
    force: True
  when: item not in common_config_skip
  with_items: '{{ common_config_files }}'
```

This variable and task structure will be maintained for each role than handles configuration. If you needed to apply a second layer of configuration files on top of the base role, the `meta/main.yml` file would need to pass down the list of files to skip. In this example, an “apache” role is depending on the common_config role:
`apache/meta/main.yml`
```yaml
dependencies:
  - role: common_config
    common_config_skip: '{{ apache_config_files }} + {{ apache_config_skip }}'
```

Note that the role passes down the list of files that it will be acting upon, in addition to the list of files to skip. This is empty by default, but it can be populated by passing in a list from another role that calls this one. This is the `meta/main.yml` for a “webmin” role that depends on the apache role:
dependencies:
```yaml
  - role: apache
    apache_config_skip: '{{ webmin_config_files }} + {{ webmin_config_skip }}'
```

In this way, a form of inheritance is possible between roles. The list is concatenated with each dependency, and with this mechanism only the highest precedence file is copied down. The repo contains playbooks that demonstrate how the roles can be used at all three levels.
