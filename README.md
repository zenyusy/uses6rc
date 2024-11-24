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

# stage1

fire

# note

- prepend `redirfd -w 2 logger.err` in logger/run, for logger err
    + `cat fifo`
