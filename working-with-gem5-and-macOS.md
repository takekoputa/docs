## Compilation

- Compiling gem5 might result in opening a lot of files and on macOS, the default number of files can be opened
at the same time on the whole system is `30720`, and that number for an individual process is `10240`.
To increase these limits,
```sh
sudo sysctl kern.maxfiles=70000
sudo sysctl kern.maxfilesperproc=70000
```
