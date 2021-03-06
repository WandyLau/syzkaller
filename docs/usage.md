# How to use syzkaller

## Running

Start the `syz-manager` process as:
```
./bin/syz-manager -config my.cfg
```

The `syz-manager` process will wind up VMs and start fuzzing in them.
The `-config` command line option gives the location of the configuration file, which is [described here](configuration.md).
Found crashes, statistics and other information is exposed on the HTTP address specified in the manager config.

At this point it's important to ensure that syzkaller is able to collect code coverage of the executed programs (unless you specified `"cover": false` in the config).
The `cover` counter on the web page should be non zero.

## Crashes

Once syzkaller detected a kernel crash in one of the VMs, it will automatically start the process of reproducing this crash (unless you specified `"reproduce": false` in the config).
By default it will use 4 VMs to reproduce the crash and then minimize the program that caused it.
This may stop the fuzzing, since all of the VMs might be busy reproducing detected crashes.

The process of reproducing one crash may take from a few minutes up to an hour depending on whether the crash is easily reproducible or reproducible at all.
Since this process is not perfect, there's a way to try to manually reproduce the crash, as described [here](reproducing_crashes.md).

If a reproducer is successfully found, it can be generated in one of the two forms: syzkaller program or C program.
Syzkaller always tries to generate a more user-friendly C reproducer, but sometimes fails for various reasons (for example slightly different timings).
In case syzkaller only generated a syzkaller program, there's [a way to execute them](reproducing_crashes.md) to reproduce and debug the crash manually.

## Reporting bugs

Check [here](linux_kernel_reporting_bugs.md) for the instructions on how to report Linux kernel bugs.

## Other

[How to connect several managers via Hub](connecting_several_managers.md)
