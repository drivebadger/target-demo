This is an extension for Drive Badger. It provides a pair of sample `target.uuid` and `ignore.uuid` files, containing the lists of partition UUIDs, that should be:

- recognized by Mobile Badger as target drives
- ignored by Mobile Badger

after plugging in the USB Mass Storage device containing these partitions.

### Installing

Do NOT clone this repository directly. Instead, create similar repository from stratch, reading the below warning first
(empty repository with just `target.uuid` and `ignore.uuid` files - or one of them - is enough).

Next, deploy it to `/opt/drivebadger/config/target-yourchosenname` local directory on your Drive Badger drive.

This way, you are able to update it in the future by a single command `/opt/drivebadger/update.sh` (it automatically runs
`/opt/drivebadger/internal/generic/rebuild-uuid-lists.sh` after update - if you want to update this repository alone,
remember to run this script manually).

Multiple `target.uuid` and `ignore.uuid` files are allowed - each in separate repository.

### Public vs private repository?

`target.uuid` and `ignore.uuid` files mostly contain **confidential information about your particular devices** used or planned for the attack.

This particular repository also contains information about real devices - the ones, that are used by Drive Badger authors for demo/training
purposes. So, ones not really confidential. Therefore, at our risk, we decided to make this repository public to simplify its distribution
into many Raspberry Pi devices (to avoid configuring Github deploy keys for each training device, and to show you the real example).

However in your case, **you should most probably keep these files in private repository.**

#### Deploying a private repository

1. Create a private repository (the below example assumes `yourgithublogin` as your Github login, and `target-yourchosenname` as your private repository name).

2. Create a [deploy key](https://docs.github.com/en/developers/overview/managing-deploy-keys), on each device separately:

```
/opt/drivebadger/internal/git/key.sh targets
```

Generated key will be displayed on console, and it will end with the hostname of local device (setting proper hostnames helps in later management of lost/broken/retired devices).

3. Go to this address and add generated deploy key to your private repository:

https://github.com/yourgithublogin/target-yourchosenname/settings/keys

4. Actually deploy the repository to the device:

```
GIT_SSH=/opt/drivebadger/internal/git/helper.sh \
	GIT_KEY=~/.ssh/id_github_targets \
	git clone \
	git@github.com:yourgithublogin/target-yourchosenname.git \
	/opt/drivebadger/config/target-yourchosenname
```

Once you've done it properly, `/opt/drivebadger/update.sh` script will automatically discover all deploy keys and will pull updates also from private repositories.


### More information

- [Drive Badger main repository](https://github.com/drivebadger/drivebadger)
- [Drive Badger wiki](https://github.com/drivebadger/drivebadger/wiki)
- [description, how configuration repositories work](https://github.com/drivebadger/drivebadger/wiki/Configuration-repositories)
