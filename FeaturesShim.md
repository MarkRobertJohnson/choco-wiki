# Shimming

Shimming is like symlinking, but it works much better. It's a form of
redirection, where you create a "shim" that redirects input to the
actual binary process and shares the output. It can also work to simply
call the actual binary when it shims GUI applications.

We like to call this "batch redirection that works".

This also allows applications and tools to be on the "PATH" without
cluttering up the PATH environment variable.

These are the benefits of creating a shim:

 * Provides an exe file that calls a target executable.
 * The exe can be called from powershell, bash, cmd.exe, or other shells just like you would call the target.
 * Blocks and waits for command line apps to finish running, exits immediately when running a GUI app.
 * Uses the icon of the target if the target exists on creation.
 * Works better than symlinks. Symlinks on Windows fall down at file dependencies. So if your file depends on other files and DLLs, all of those need to also be linked.
 * Does not require special privileges like creating symlinks (symbolic links) do. So you can create shims without administrative rights.

## Usage
Chocolatey automatically shims executables in package folders that are
not explicitly ignored, putting them into the
`$($env:ChocolateyInstall)\bin` folder (and subsequently onto the PATH).

These executables can come as part of the package or downloaded to the
package folder during the install script.

Chocolatey ensures the folder `$($env:ChocolateyInstall)\bin` in the
PATH environment variable, allowing you to put tools on the PATH without
cluttering up the PATH.

## Options and Switches

You pass these arguments to an executable that is a shim (e.g. executables in the bin directory of your Chocolatey install, not choco.exe):

 * `--shimgen-help` - shows this help menu and exits without running the target
 * `--shimgen-log` - logging is shown on command line
 * `--shimgen-waitforexit` - explicitly tell the shim to wait for target to exit - useful when something is calling a gui and wanting to block - command line programs explicitly have waitforexit already set.
 * `--shimgen-exit` - explicitly tell the shim to exit immediately.
 * `--shimgen-gui` - explicitly behave as if the target is a GUI application. This is helpful in situations where the package did not have a proper .gui file.
 * `--shimgen-usetargetworkingdirectory` - set the working directory to the target path. Useful when programs need to be running from where they are located (usually indicates programs that have issues being run globally).
 * `--shimgen-noop` - Do not actually call the target. Useful to see what would happen if you ran the command.

## FAQ

### How do I take advantage of this feature?
This works with all versions of Chocolatey. Just use packages and when those packages have exe files, those are automatically shimmed so they are on the PATH.

### How does it work?
Chocolatey uses a tool called ShimGen that inspects an executable and creates a small binary, known as a "shim", that simply calls the executable. Then it places that shim in the `$($env:ChocolateyInstall)\bin`. It creates the shim by generating it at runtime based on the actual binary's information.

### How is this better than symlinks?
When you symlink a file on Windows, you must symlink all of its dependencies like dlls and config files. If you put that all into the `$($env:ChocolateyInstall)\bin` folder to take advantage of being on the PATHyou can see that there

### Does it require admin rights?
No, and this is another thing that sets it apart from symlinks. To create symlinks on Windows, you need to have `SeCreateSymbolicLinkPrivilege`, which is one of the privileges granted to Administrators.

### Does the shim work with UAC?
Yes! When a shim detects that elevation is required, it will automatically request elevation.

### Why not simple batch redirection?
We tried using batch ("*.bat") files, and it mostly works, but when applications calling other applications expect the file name to be ".exe", a file named "*.bat" doesn't work. Batch files also don't work in all shells, and shims do.

### Have you thought about shimming in more places?
Yes, but we have not decided whether shimming Program Files is a good idea yet or not. Packages can explicitly enforce shims with [[Install-BinFile|HelpersInstallBinFile]].

### I need to shim a non-exe file.
If you are creating a package and you need to shim a file that doesn't end in .exe (like a .bat file), you should look at [[Install-BinFile|HelpersInstallBinFile]].

### I need to exclude a file from shimming.
If you are creating a package and you want to skip creation of a shim for a particular file, you can create a "*.ignore" file.

Set an empty file next to the executable (or where it will be downloaded/unpacked to), sharing the same name with the executable and appending ".ignore". For example, if your file is named "`bob.exe`", you need a file named "`bob.exe.ignore`" (pay attention to case - "`BOB.exe.ignore`" may not work with all versions of Chocolatey).

[[Read more...|CreatePackages#how-do-i-exclude-executables-from-getting-shims]]

### How can I ensure a GUI shim?
Chocolatey 0.9.10+ will automatically detect GUI applications and adjust the shim accordingly. The detection may not always be accurate, and older versions of Chocolatey don't handle this, so it's best to create a "*.gui" file to direct the shim creation to be for a GUI application.

If you are creating a package and want the shim to exit immediately after calling the application, create an empty "*.gui" file next to where the exe file is (or where it will be downloaded/unpacked to), sharing the same name with the executable and appending ".gui". For example, if your file is named "`bob.exe`", you need a file named "`bob.exe.gui`" (pay attention to case - "`BOB.exe.gui`" may not work with all versions of Chocolatey).

[[Read more...|CreatePackages#how-do-i-set-up-shims-for-applications-that-have-a-gui]]

### A package messed up and should have set up a shim as a GUI.
Call the shim with `--shimgen-gui` to target the correct behavior.

### An executable requires being run from the folder where it actually is.
Call the shim with `--shimgen-usetargetworkingdirectory`. There are badly behaved applications that don't run well from anywhere, and they require some extra help so they will run correctly.