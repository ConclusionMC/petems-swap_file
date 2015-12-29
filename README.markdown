####Table of Contents

1. [Overview](#overview)
2. [Module Description ](#module-description)
3. [Setup](#setup)
    * [What swap_file affects](#what-swap_file-affects)
4. [Usage](#usage)
5. [Limitations](#limitations)
6. [Development](#development)

##Overview

Manage [swap files](http://en.wikipedia.org/wiki/Paging) for your Linux environments. This is based on the gist by @Yggdrasil, with a few changes and added specs.

##Setup

###What swap_file affects

* Creating a swap-file on disk. This uses `dd` by default, but can use `fallocate` optionally for performance reasons. **Note: Using fallocate to create a ZFS file system will fail: https://bugzilla.redhat.com/show_bug.cgi?id=1129205**
* Swapfiles on the system
* Any mounts of swapfiles

##Usage

The simplest use of the module is this:

```puppet
swap_file::files { 'default':
  ensure   => present,
}
```

By default, the module it will:

* create a file using /bin/dd atr `/mnt/swap.1` with the default size taken from the `$::memorysizeinbytes`
* A `mount` for the swapfile created

For a custom setup, you can do something like this:

```puppet
swap_file::files { 'tmp file swap':
  ensure    => present,
  swapfile  => '/tmp/swapfile',
  add_mount => false,
}
```

To use `fallocate` for swap file creation instead of `dd`:

```puppet
swap_file::files { 'tmp file swap':
  ensure    => present,
  swapfile  => '/tmp/swapfile',
  cmd       => 'fallocate',
}


To remove a prexisting swap, you can use ensure absent:

```puppet
swap_file::files { 'tmp file swap':
  ensure   => absent,
}
```

## Previous to 1.0.1 Release

Previously you would create swapfiles with the `swap_file` class:

```
class { 'swap_file':
   swapfile     => '/mount/swapfile',
   swapfilesize => '100 MB',
}
```

However, this had many problems, such as not being able to declare more than one swap_file because of duplicate class errors.

This is now removed from 2.x.x onwards.

##Limitations

Primary support is for Debian and RedHat, but should work on all Linux flavours.

Right now there is no BSD support, but I'm planning on adding it in the future

##Development

Follow the CONTRIBUTING guidelines! :)
