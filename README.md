# [rc](https://skarnet.org/software/s6-rc/)

## [service definition](https://skarnet.org/software/s6-rc/s6-rc-compile.html)

```
sd
|- init: oneshot
|- toil: longrun, depending on `init`
|- default: bundle of the above
```

## compile

```sh
s6-rc-compile sdb sd
```
(no need for `-h fdhuser`)

# run-image, [scandir](https://skarnet.org/software/s6/scandir.html)

## for user process

```
run-image
|- scandir
   |- .s6-svscan
      |- crash [1]
      |- finish
   |- logger
      |- run  [2]
      |- fifo [3]
```

- **[1]** [control dir](https://www.skarnet.org/software/s6/s6-svscan.html)
- **[2]** logdir
- **[3]** mkfifo

## for Linux pid1

```
run
|- service
   |- .s6-svscan
      |- SIGTERM # shut down rc and svscan by kill -15 1
      |- finish  # after SIGTERM, shut down everything
      |- crash
   |- log        # mkfifo -m 600
   |- tty
|- uncaught-logs # mkdir -m 750
```

# stage1

## user process

`stage1`, controlled by *fire*

## Linux pid1

`init`, and `poff`

rc live dir will be at [the default location: /run/s6-rc](https://www.skarnet.org/software/s6-rc/s6-rc-init.html)

# note

- prepend `redirfd -w 2 logger.err` in logger/run, for logger err
    + `cat fifo`
