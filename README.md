# flexctl
Bash script to control multiple instances of [Flexget](https://github.com/Flexget/Flexget)

## General usage
```
flexctl {dev} {daemon_cmd} {--flexget_flags} {alias_cmd} {flexget_cmd}
```
- ``dev``: Utilize alternate Flexget binary and settings, such as config path. Generally used when you have a development version installed along with a production version. See Configuration section 1 below.
- ``daemon_cmd``: One of ``start``, ``stop``, ``reload``, ``restart``, or ``status``.
    - ``start``: Start Flexget and daemonize it.
    - ``stop``: Shutdown the Flexget daemon.
    - ``reload``: Reload configuration file in a running Flexget daemon.
    - ``restart``: Shutdown the Flexget daemon, wait two seconds and restart it.
    - ``status``: Display whether a Flexget daemon is running, and if so, its process ID.
- ``flexget_flags``: Any flags that can be utilized with the normal Flexget binary can be inserted here, such as ``-h``, ``-V``, or ``--loglevel``.
- ``alias_cmd``: Run a command alias. See [Configuration section 2](https://github.com/tubedogg/flexctl#2-command-aliases) below.
- ``flexget_cmd``: Run normal Flexget commands, or append additional commands to the ``alias_cmd``.

### Examples:
```bash
# starts the Flexget daemon utilizing alternate Flexget binary and settings
# equivelant to `/path/to/dev/flexget -c /path/to/dev/config.yml -l /path/to/dev/fg.log daemon start`
→ flexctl dev start

# executes my_task_name with debug logging
# equivelant to
# `/path/to/flexget -c /path/to/config.yml -l /path/to/fg.log -L debug execute --tasks my_task_name`
→ flexctl -L debug execute --tasks my_task_name

# expands the command alias 'fss' (by default, this is 'series show') and
# passes "My Show" as the show name
# equivelant to `/path/to/flexget -c /path/to/config.yml -l /path/to/fg.log series show "My Show"`
→ flexctl fss "My Show"

# this example presumes a command alias of
# [fdtv]='-L debug execute --tasks download_my_tv'
# is present (see Configuration section 2)
# equivelant to
# `/path/to/flexget -c /path/to/config.yml -l /path/to/fg.log -L debug execute --tasks download_my_tv`
→ flexctl fdtv

# displays the versions of both flexctl and Flexget
# equivelant to `/path/to/flexget -V`
→ flexctl -V

# displays help for both flexctl and Flexget CLI
# equivelant to `/path/to/flexget -h`
→ flexctl -h

# displays help for the 'series show' Flexget CLI command
# equivelant to `/path/to/flexget -c /path/to/config.yml -l /path/to/fg.log series show -h`
→ flexctl series show -h
```

## Configuration
To configure ``flexctl``, open it in a text editor. These instructions can also be found in the script itself.

#### 1. Paths

   You can configure up to two versions of Flexget installed using the 'normal' and 'dev' executable path and binary name, config path, and log path.

   If `normal_config_path` and `normal_log_path` are left empty, Flexget will determine the config and log paths for itself.

   Customize `dev_config_path` and `dev_log_path` for your environment. If you don't use multiple versions of Flexget, these can be left blank.

   - ``normal_command_path``:    Path to (if necessary) and name of Flexget binary. (Usually just `flexget`)
   - ``normal_config_path``:     Path to Flexget config file. (Defaults to `~/.flexget`, or `~/flexget` on Windows)
   - ``normal_config_filename``: Filename of Flexget config file. (Defaults to `config.yml`)
   - ``normal_log_path``:        Path to and filename of log file. (Defaults to `~/.flexget/flexget.log`)
   - ``dev_command_path``:       Path to (if necessary) and name of alternate Flexget binary.
   - ``dev_config_path``:        Path to alternate Flexget config file.
   - ``dev_config_filename``:    Filename of alternate Flexget config file.
   - ``dev_log_path``:           Path to and filename of alternate log file.

#### 2. Command aliases

   These are short aliases to commonly used commands sent to Flexget.

   Define them here by entering a command alias, such as `fsl`, and the actual command to send to Flexget, such as `series list`. See examples below and note that all aliases must follow the format ``[alias]='command'``.
   
   Anything coming after the command alias will be appended to the command defined here. For example, `flexctl fsl all` will run the command `<flexget> series list all`.

   ##### Examples:
```
[fsf]='series forget'
[fsb]='series begin'
[fss]='series show'
[fsl]='series list'
```

#### 3. Options

   Set ``print_flexget_path`` to 1 (e.g. ``print_flexget_path=1``) to always print the path to and name of the Flexget binary as the first output.
