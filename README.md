nginx
=========

An ansible role to install the latest version of Nginx on Centos, Ubuntu or Debian, configure the main nginx.conf configuration file, create a 'security.conf' file to be included with any server definitions added at a later stage, enable the software in systemd and start it.

Tested using Ansible 2.1.2 on:

CentOS 7
Debian Jessie
Ubuntu Trusty

Requirements
------------

Ansible 2+

Configuration Variables
--------------

All variables are in 'defaults/main.yml'

```yml

# RedHat/Centos version to configure Nginx repo base URL
rh_os: 'centos'
rh_release_ver: '7'
rh_base_arch: 'x86_64'

# Nginx user/group to run the processes and for file permissions
nginx_user: 'nginx'
nginx_group: "{{ nginx_user }}"

# nginx.conf:


# The number of processes to run.
# 'auto' sets one nginx process per core (recommended ratio).
# Set this too high and you will could idle processes lying around.
nginx_worker_processes: 'auto'
# Sets the maximum number of connections that can be processed at one time by
# each worker process. 512 is the default and is quite low.  The appropriate
# setting depends on the size of the server and the nature of the traffic, and
# can be discovered through testing.
# Use 'ulimit -n' to see the systems core limitations. Exceeding this number
# would cause serious problems. Consider changing the default system setting.
nginx_worker_connections: '512'
nginx_pid: '/run/nginx.pid'
nginx_log_dir: '/var/log/nginx/'
nginx_error_log: 'error.log'
nginx_access_log: 'access.log'
# set the format of the logfiles
# http://nginx.org/en/docs/http/ngx_http_log_module.html
nginx_log_format: '$remote_addr - $remote_user [$time_local] \
                  "$request" \
                  $status \
                  $body_bytes_sent \
                  "$http_referer" \
                  "$http_user_agent" \
                  "$http_x_forwarded_for"'
# Set the verbosity / information that Nginx logs to the error log
nginx_error_log_level: 'notice'
# keep alive allows for fewer client browser reconnects
nginx_keepalive_timeout: '65'
nginx_keepalive_requests: '100000'
# tcp_sendfile optimizes serving static files from the file system
nginx_sendfile: 'on'
# tcp_nopush optimizes the amount of data sent down the wire at once by
# activating the TCP_CORK option within the TCP stack. TCP_CORK blocks the data
# until the packet reaches the MSS, which is equal to the MTU minus the 40 or
# 60 bytes of the IP header
nginx_tcp_nopush: 'on'
# tcp_nodelay allows Nginx to make TCP send multiple buffers as individual packets
nginx_tcp_nodelay: 'on'
# The client_body_timeout and client_header_timeout directives are responsible
# for the time a server will wait for a client body or client header to be
# sent after request. If neither a body or header is sent, the server will issue
# a 408 error or Request time out.
nginx_client_header_timeout: '12'
nginx_client_body_timeout: '12'
# send_timeout specifies the response timeout to the client. This timeout does
# not apply to the entire transfer but, rather, only between two subsequent
# client-read operations
nginx_send_timeout: '10'
# Gzip can help reduce the amount of network transfer Nginx deals with
# Setting 'comp_level' too high could waste CPU.
# http://bit.ly/2fVfhmk
nginx_gzip: 'on'
nginx_gzip_disable: '"MSIE [1-6]\."'
nginx_gzip_vary: 'on'
nginx_gzip_proxied: 'any'
nginx_gzip_comp_level: '2'
nginx_gzip_buffers: '16 8k'
# What to compress
nginx_gzip_types:
  - text/plain
  - text/css application/json
  - application/x-javascript
  - text/xml application/xml
  - application/xml+rss
  - text/javascript

# client_body_buffer_size handles the client buffer size, mostly POST forms
nginx_client_body_buffer_size: '128k'
# client_max_body_size sets the max body buffer size. If the size in a request
# exceeds the configured value, a 413 error is returned to the client. Setting
# this to 0 disables the check
nginx_client_max_body_size: '10m'
# client_header_buffer_size handles the client header size. 1k is usually a
# sane choice for this by default.
nginx_client_header_buffer_size: '1k'
# large_client_header_buffers shows the maximum number and size of buffers for
# large client headers
nginx_large_client_header_buffers: '4 4k'
# output_buffers sets the number and size of the buffers used for reading a
# response from a disk.
nginx_output_buffers: '1 32k'
# If possible, the transmission of client data will be postponed until Nginx
# has at least the set size of bytes of data to send.
# Setting this to 0 disables postponing data transmission.
nginx_postpone_output: '1460'

# security.conf:

# Flag to deny access to any requests not specifically for our server.
nginx_strict_hostname_access: 'true'
# Flag to limit request methods to those in the list below.
nginx_limit_request_method_access: 'true'
nginx_allowed_request_methods:
  - GET
  - HEAD
  - POST
# Flag to deny requests linked in from dodgy referers
nginx_filter_referers: 'true'
nginx_deny_referers:
  - babes
  - forsale
  - girl
  - jewelry
  - love
  - organic
  - poker
  - porn
  - sex
  - teen
# Cache static files for a short period of time
# open_file_cache max sets the number of files cached
nginx_open_file_cache_max: '1000'
# open_file_cache inactive excludes files not accessed for a period of time
nginx_open_file_cache_inactive: '20s'
# open_file_cache_min_uses sets the minimum number of file accesses during the
# period configured by the inactive parameter, required for a file descriptor
# to remain open in the cache
nginx_open_file_cache_min_uses: '5'
# open_file_cache_valid sets the length of time files are cached
nginx_open_file_cache_valid: '30s'
# Enables/disables cache file lookup errors
nginx_open_file_cache_errors: 'off'

```

Useful resources on Nginx configuration tuning
--------------------------------------------

[ Linode: Configure Nginx for optimum performance ](https://www.linode.com/docs/websites/nginx/configure-nginx-for-optimized-performance#buffer-size)

[ Digital Ocean: How to optimise nginx configuration ](https://www.digitalocean.com/community/tutorials/how-to-optimize-nginx-configuration)

[ Nginx: Tuning nginx ](https://www.nginx.com/blog/tuning-nginx/)

[ Github Gist: Best nginx configuration for improved security](https://gist.github.com/plentz/6737338)


Example Playbook
----------------

    - hosts: servers
      become: true
      roles:
         - nginx

License
-------

MIT
