Follow the `git log` to see what encountering and resolving this issue looks like:

```
$ git log --oneline
6719385 (HEAD -> main) Fix conda-lock file by `rm conda-lock.yml && conda-lock`
ca3a030 `bump-my-version` is now on conda-forge! Remove `pip` section and re-lock
fa676c7 Initial commit with `pip` dependency
```

* Create an `environment.yml` file with a `pip` dependency, e.g. `bump-my-version`.
* Lock. Everything looks OK.
* Migrate that dependency out of the `pip` section, e.g. because that dependency became
  available on `conda-forge`, and change the version.
* Lock. There are now two copies of `bump-my-version` in `conda-lock.yml`
* Install from lockfile. The "old" version, which is no longer present in
  `environment.yml`, is installed. For me, this was very unexpected and took me a long
  time to understand why it was happening.
* Remove the `conda-lock.yml` file and lock again. Everything looks OK again.


> :memo: There is a corresponding bug in `micromamba` in which it will install the `pip`
> version but `micromamba list` will fib and tell me the version from `conda-forge` is
> installed.
