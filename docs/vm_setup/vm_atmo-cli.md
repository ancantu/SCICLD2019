### Atmosphere Command Line Utilities

Jetstream has a command-line alternative to the Atmosphere web-interface, called `atmo`. `atmo` provides the same control of Instances and Volumes that the web-interface does.

#### Installation:
`atmo` can be installed into any Linux/MacOS environment using:

```
pip install --user atmosphere-cli
```
Two specific environment variables allow one to point `atmo` at your account on Jetstream. We recommend you add these to your `.bashrc` file in your `$HOME` directory:
```
export ATMO_BASE_URL="https://use.jetstream-cloud.org"
export ATMO_AUTH_TOKEN="fff28djdFAKETOKENfff28djdFAKETOKENfff28djd"
```

#### Token:
To get a Personal Access Token:

* Browse to your web `Dashboard`
* Access your `Login> Settings` in the top right <br><img src="https://iujetstream.atlassian.net/wiki/download/thumbnails/641794049/image2018-11-29_16-33-38.png?version=1&modificationDate=1543530820006&cacheVersion=1&api=v2&width=100&height=100">
* Click on `Show More` under `Advance`.
* Scroll down to `Persona Access Tokens` and Click on the `+` button
* Give your token a name and click on "Create Token"<br><img src="https://iujetstream.atlassian.net/wiki/download/thumbnails/641794049/image2018-11-29_16-37-56.png?version=1&modificationDate=1543531078190&cacheVersion=1&api=v2&width=255&height=150">
* Save this Token by clicking on the "Copy" button and pasting it somewhere you can recover it.<br>
___THIS IS YOUR ONLY CHANCE TO SAVE THIS TOKEN.
YOU WILL HAVE TO DELETE THE TOKEN AND START OVER IF YOU DO NOT SAVE IT.___
* Replace this token in the ATMO_AUTH_TOKEN line in your `.bashrc` as as previously mentioned.

#### `atmo` Usage:
In your terminal type:
```
prompt> atmo --help
usage: atmo [--version] [-v | -q] [--log-file LOG_FILE] [-h] [--debug]
            [--atmo-base-url <atmosphere-base-url>]
            [--atmo-auth-token <atmosphere-auth-token>]
            [--atmo-api-server-timeout <atmosphere-api-server-timeout>]

           Atmosphere CLI

           optional arguments:
             --version             show program's version number and exit
             -v, --verbose         Increase verbosity of output. Can be repeated.
             -q, --quiet           Suppress output except warnings and errors.
             --log-file LOG_FILE   Specify a file to log output. Disabled by default.
             -h, --help            Show help message and exit.
             --debug               Show tracebacks on errors.
             --atmo-base-url <atmosphere-base-url>
                                   Base URL for the Atmosphere API (Env: ATMO_BASE_URL)
             --atmo-auth-token <atmosphere-auth-token>
                                   Token used to authenticate with the Atmosphere API
                                   (Env: ATMO_AUTH_TOKEN)
             --atmo-api-server-timeout <atmosphere-api-server-timeout>
                                   Server timeout (in seconds) when accessing Atmosphere
                                   API (Env: ATMO_API_SERVER_TIMEOUT)

           Commands:
             allocation source list  List allocation sources for a user.
             allocation source show  Show details for an allocation source.
             complete       print bash completion command (cliff)
             group list     List groups for a user.
             group show     Show details for a group.
             help           print detailed help for another command (cliff)
             identity list  List user identities managed by Atmosphere.
             identity show  Show details for a user identity.
             image list     List images for user.
             image search   Search images for user.
             image show     Show details for an image.
             image version list  List image versions for an image.
             image version show  Show details for an image version.
             instance actions  Show available actions for an instance.
             instance attach  Attach a volume to an instance.
             instance create  Create an instance.
             instance delete  Delete an instance.
             instance detach  Detach a volume from an instance.
             instance history  List history for instance.
             instance list  List instances for user.
             instance reboot  Reboot an instance.
             instance redeploy  Redeploy to an instance.
             instance resume  Resume an instance.
             instance shelve  Shelve an instance.
             instance show  Show details for an instance.
             instance start  Start an instance.
             instance stop  Stop an instance.
             instance suspend  Suspend an instance.
             instance unshelve  Unshelve an instance.
             maintenance record list  List maintenance records for Atmosphere.
             maintenance record show  Show details for a maintenance record.
             project create  Create a project.
             project list   List projects for a user.
             project show   Show details for a project.
             provider list  List cloud providers managed by Atmosphere.
             provider show  Show details for a cloud provider.
             size list      List sizes (instance configurations) for cloud provider.
             size show      Show details for a size (instance configuration).
             version        Show Atmosphere API version.
             volume create  Create a volume.
             volume delete  Delete a volume.
             volume list    List volumes for a user.
             volume show    Show details for a volume.
```

Try:

* Get a list of providers:

```
prompt> atmo provider list
+----+--------------------------------------+--------------------------------+-----------+----------------+--------+--------+------------+
| id | uuid                                 | name                           | type      | virtualization | public | active | start_date |
+----+--------------------------------------+--------------------------------+-----------+----------------+--------+--------+------------+
|  4 | f906a5ee-34a8-499a-9185-a35feb3d6f01 | Jetstream - Indiana University | OpenStack | KVM            | True   | True   | 2016-01-28 |
|  5 | 3ff65aa8-505b-48c3-aef1-aa0ada14c756 | Jetstream - TACC               | OpenStack | KVM            | True   | True   | 2016-02-16 |
+----+--------------------------------------+--------------------------------+-----------+----------------+--------+--------+------------+
```

* Get a list of Instances:

```
prompt> atmo provider list
+--------------------+--------------------+---------+----------+----------------+-----------+----------------------+---------------+------------+
| uuid               | name               | status  | activity | ip_address     | size      | provider             | project       | launched   |
+--------------------+--------------------+---------+----------+----------------+-----------+----------------------+---------------+------------+
| 690c98d0-674e-     | Train199-U18-Docke | active  |          | 149.165.156.92 | m1.tiny   | Jetstream - Indiana  | First_Project | 2019-07-22 |
| 4c7b-b6aa-         | r                  |         |          |                |           | University           |               |            |
| 20318568e90c       |                    |         |          |                |           |                      |               |            |
+--------------------+--------------------+---------+----------+----------------+-----------+----------------------+---------------+------------+
```

* Get details about an Instance:

```
prompt> atmo instance show 690c98d0-674e-4c7b-b6aa-20318568e90c
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| id                | 33652                                |
| uuid              | 690c98d0-674e-4c7b-b6aa-20318568e90c |
| name              | Train199-U18-Docker                  |
| username          | train199                             |
| identity          | Username: train199, Project:train199 |
| project           | First_Project                        |
| allocation_source | TG-CDA170005                         |
| compute_allowed   | 150000                               |
| compute_used      | 104243.322                           |
| global_burn_rate  | 4.5                                  |
| user_compute_used | 801.51                               |
| user_burn_rate    | 3.5                                  |
| image_id          | 717                                  |
| image_version     | 1.22                                 |
| image_usage       | 104243.322                           |
| launched          | 2019-07-22                           |
| image_size        | m1.tiny                              |
| image_cpu         | 1                                    |
| image_mem         | 2048                                 |
| image_disk        | 8                                    |
| status            | active                               |
| activity          |                                      |
| ip_address        | 149.165.156.92                       |
| provider          | Jetstream - Indiana University       |
| web_desktop       | True                                 |
| shell             | False                                |
| vnc               | True                                 |
+-------------------+--------------------------------------+
```

<br>

---

<br>

Previous: [Volumes](vm_volumes.md) | Next: **BREAK** followed by [Building Containers](../docker_containers/docker_containers.md) | Top: [Course Overview](../../index.md)
