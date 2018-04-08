+++
archive = true
date = "2014-12-15T21:21:45Z"
title = "Mounting a Windows directory in the Docker VM (boot2docker)"

+++
If you’re building a Docker image on Windows you’ll want to make your application files available to the Docker VM for inclusion in the container.

Assuming you have installed [boot2docker](https://github.com/boot2docker/boot2docker "boot2docker") and enabled [hardware virtualization](http://www.sysprobs.com/disable-enable-virtualization-technology-bios "hardware virtualization"), your Windows `Users` directory will automatically be available inside the Docker VM under the root directory.

For example, `C:\Users` on your Windows host will be accessible under `/c/Users` in the Docker VM.

To make ***any*** directory available to the VM, you simply need to add it as a shared directory in VirtualBox, and then mount it on the VM:

1. Open the VM VirtualBox Manager that was installed with boot2docker and open the settings dialog.
2. Select the ‘Shared Folders’ item from the menu and add a new shared folder definition.
3. Set the folder path to the directory you want available to the VM (eg, your app root directory) and give it a sensible name, eg ‘my-application’.
4. Create a new directory on the Docker VM that will contain your application contents, eg: `mkdir  ~/my-application-contents`.
5. Mount your shared application directory to the directory you just created, eg: `sudo mount -t vboxsf my-application ~/my-application-contents`

That’s it. You can now treat `~/my-application-contents` as your application working directory and target any of its contents in your Dockerfile.


