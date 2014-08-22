# ansible-td-agent

This ansible role installs the td-agent, the distribution of [fluentd](http://fluentd.org/).

This role is written based on [the official installation page](http://docs.fluentd.org/categories/installation)

## Requirements

This role requires Ansible 1.4 higher and platforms listed in the metadata file.

## Role Variables

The variables that can be passed to this role **when the OS is Debian** and a brief description about them are as follows

    # The version of td-agent (optional)
    td_agent_version: 1.1.18-1

    # Architecture of the machine (required, i386 or amd64)
    td_agent_architecture: amd64

    # Additional configuration for the td-agent.conf file
    td_agent_conf: |
        <match myproject.*>
            type stdout
        </match>

    # Automatically include files from /etc/td-agent/conf.d/*
    td_agent_include: true

    # Adds a td-agent target with your api key to the conf file
    td_agent_api_key: <your-td-api-key>

    # Adds a http source
    td_agent_http: true

    # Changes the port of the http client. (default is 8888)
    td_agent_http_port: 8080

    # Sets up debugging for the td-agent.
    td_agent_debug: true

    # Changes the default debug port (default is 24230)
    td_agent_debug_port: 43424

    # Adds s3 output
    td_agent_s3:

        # match pattern (default is 's3.*')
        pattern: *.*

        # aws key to access the s3 bucket (needs to have read/write privileges) (required!)
        aws_key_id: "{{ lookup('env', 'TD_AGENT_S3_AWS_KEY_ID') }}"

        # aws security key to the "aws_key_id" (required!)
        aws_sec_key: "{{ lookup('env', 'TD_AGENT_S3_AWS_SEC_KEY') }}"

        # aws bucket to be used to store the log files (required!)
        bucket: my-bucket-id

        # region of the s3 bucket (default 'us-east-1')
        region: ap-northeast-1

        # full key format definition (default '{{prefix}}%{hostname}-%{time_slice}_%{index}.%{file_extension}' thus it overrides the 'prefix' definition)
        object_key_format: %{hostname}-%{time_slice}_%{index}.%{file_extension}

        # prefix that is prepended to the file path for the log files (only works when object_key_format is not set)
        prefix: some/folder/

        # Path to keep files before pushing them to s3 (default /var/tmp/fluent/s3)
        buffer_path: /tmp_fluent_folder

        # Format used to slice the log files (default '%Y%m%d-%H')
        time_slice_format '%Y%m%d'

        # Delay to wait before writing the timeslice to s3 (default '10m')
        time_slice_wait '2m'

Detail for [the official intallation page](http://docs.fluentd.org/articles/install-by-deb)

## Examples

1) Ubuntu or EL

    ---
    - hosts: all
      roles:
        - td-agent

2) Debian (define only architecture)

    ---
    - hosts: all
      roles:
        - role: td-agent
          td_agent_architecture: i386

3) Debian (define architecture and td_agent_version)

    ---
    - hosts: all
      roles:
        - role: td-agent
          td_agent_architecture: amd64
          td_agent_version: 1.1.17-1


## Dependencies

None

## Licence

BSD

## Contribution

Fork and make pull request!

[kawasakitoshiya / ansible-td-agent](https://github.com/kawasakitoshiya/ansible-td-agent)

## Author Information

Toshiya Kawasaki
