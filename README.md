# vagrant-vbguest: Test CentOS Kernel Guest Additions

This repo contains a `Vagrantfile` that makes it possible to reproduce an issue where the Virtualbox Guest Additions are not installed correctly when using CentOS.

The reason, [as described in the original issue][original-issue], is because CentOS will regularly remove old kernel releases from their repositories, but not update the Vagrant base box when this happens.  This means it is possible to end up in a state where the latest base box has an out of date kernel, and thanks to the old kernels no longer existing in the repos, it is impossible to install a version of the kernel headers, needed to compile the Guest Additions, that matches the running kernel.

The theoretical fix is to install the latest kernel in addition to the kernel headers, thereby working around the lack of old kernel headers in the repositories.  Adding this as an option would both allow CentOS users and other Linux OS users to upgrade their kernel if they desire.

[original-issue]: https://github.com/dotless-de/vagrant-vbguest/issues/297

## Usage

1.  Clone this repo and the [plugin repo][vbguest] in to the same root directory:

    ```
    git clone git@github.com:dotless-de/vagrant-vbguest.git
    git clone git@github.com:xanderxaj/vagrant-vbguest-test-centos-kernel-guest-additions.git
    ```

2.  Navigate to the plugin repo:

    ```
    cd vagrant-vbguest
    ```

3.  Install the required dependencies:

    ```
    bundle install --path .bundle
    ```

    A local path is used to avoid the need for root/administrator permissions.  `bundle` keeps a note of this path in `.bundle/config`.

4.  Check the development version of Vagrant is being run:

    ```
    bundle exec vagrant
    ```

    If the correct version is running, you will get a message beginning with:

    ```
    You appear to be running Vagrant outside of the official installers.
    ```

5.  Set the `VAGRANT_CWD` environment variable to point to the test repo; this tells Vagrant will `Vagrantfile` to use:

    ```
    export VAGRANT_CWD=../vagrant-vbguest-test-centos-kernel-guest-additions
    ```

6.  [Follow the instructions in the `vagrant-vbguest` README][vbguest] to test and develop a fix.

    In this case, simply starting the box will show whether the Guest Additions have installed correctly:

    ```
    bundle exec vagrant up
    ```

    If you see the following message (towards the end of the output), the Guest Additions *are broken*:

    ```
    ==> default: Checking for guest additions in VM...
        default: No guest additions were detected on the base box for this VM! Guest
        default: additions are required for forwarded ports, shared folders, host only
        default: networking, and more. If SSH fails on this machine, please install
        default: the guest additions and repackage the box to continue.
        default:
        default: This is not an error message; everything may continue to work properly,
        default: in which case you may ignore this message.
    ```

    If you do not see this message, they are working. :)


[vbguest]: https://github.com/dotless-de/vagrant-vbguest
