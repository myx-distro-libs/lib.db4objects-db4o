# ae3.pkg.lib.db4o

The AE3 wrapper around DB4o, the embedded object database. It vendors one jar and writes no Java of its
own: `/Volumes/workspace/myx/ae3.pkg.lib.db4o/java` and its `bin` both hold 0 entries.

It is not a storage backend. The backend is `ae3.sys.pkg.s4.lcl.db4o`, which implements `S4TreeDriver`
over the vendored `com.db4o` API. `ae3-packages/ae3.pkg.lib.db4o/package.json` provides the name `db4o`
and requires `java`.

## The vendor jar and where it ships

- The jar is `ae3-packages/ae3.pkg.lib.db4o/jars/db4o-8.0.249.jar`, 1,471,522 bytes, md5
  `0077c4f8ed37e5eee66fd29f0b2bd932`. It holds 1,441 entries, 1,439 of them under `com/db4o/`.
- It is committed beside `db4o-8.0.249-LICENSE.txt`, 22,019 bytes of GNU GPL version 2 in HTML. The same
  text sits at `incoming/db4o-8.0/db4o.license.html`, md5 `a9607898532a52b101a7e2f812059f29` in both.
- The same bytes sit at `/Volumes/workspace/myx/acm-cvs/sys-current/axiom/db4o-8.0.249.jar`, with its
  licence file beside it. Two builders fill that folder by scanning for `jars` directories:
  `/Volumes/workspace/myx/ae3-devel-tools/make-ae3-distro.sh:40` and
  `/Users/myx/Workspaces/ws-myx.ae3-devel/source/ae3/util.repository-ae3/builders/source-process/2901-make-ae3-share.sh:52`.
  Neither names this project, so the jar travels on the folder scan alone.
- `/Volumes/workspace/myx/ae3-devel-tools/MAKE-AE3-DISTRO.xml` names this project 0 times, against 2
  lines for its consumer at `:63` and `:134`.
- The package also reaches a runtime as a package.
  `/Volumes/workspace/myx/ae3-share/ae3-distro/common/ae3-packages/ae3.pkg.lib.db4o.tar.gz` is 1,285,320
  bytes and holds `jars/`, the two jar files and `package.json`. `make-ae3-distro.sh:22` collects every
  `ae3-packages` folder into `packages-all/` first, so the wrapper needs its `package.json` to be
  packaged at all.
- In the axiom, this is the only one of the 51 jars carrying a `com/db4o/` entry.

## What depends on it

- `ae3.sys.pkg.s4.lcl.db4o` is the one project that requires it, at
  `/Volumes/workspace/myx/ae3.sys.pkg.s4.lcl.db4o/project.inf:4`. Its `.classpath` adds the jar as a
  `kind="lib"` entry with `sourcepath="/ae3.pkg.lib.db4o/incoming/db4o-8.0/src"`.
- That consumer is 9 Java files and 881 lines, compiles to 10 classes, and ships as
  `acm-cvs/sys-current/axiom/ru.myx.ae3sys.s4.lcl.db4o-5.0.jar`, 16,103 bytes, 11 entries under `db4o/`.
- `acm-cvs/sys-current/settings/system/s4/driver/db4o.json` names
  `java.class/ru.myx.ae3.vfs.s4.lcl.db4o.Db4oLocalS4`, so the driver is selectable by type name `db4o`.
- `ae3.test` reaches this wrapper through the consumer and never directly. `VfsVariants.java:62` and
  `S4Variants.java:54` both name `Db4oLocalS4`.

## Gotchas

- **The wrapper's own `.classpath` does not name its jar.** It holds 0 `kind="lib"` entries, where the
  `bdbje` sibling holds 1. Only the consumer's classpath puts the jar on a compile path.
- **`incoming/` is a tracked vendor drop, not a scratch folder.** This project's copy holds 5,149 files,
  3,456 of them `.java` under `src/`, and none of it is this project's own source. The committed jar is
  what a build uses.
- **The devel checkout has not got the licence file.** The place copy is 3 commits and stops at
  `8cf6db6 + minor: explicit license file with lib name`. Its
  `/Users/myx/Workspaces/ws-myx.ae3-devel/source/lib/lib.db4objects-db4o` twin stops at `f5f8d73`, 2
  commits, a 0-byte `README.md`, and a `jars/` folder holding the jar alone.
- **The two checkouts differ in shape, not only in history.** The devel twin has no `java/` and no `bin/`
  directory at all, while the place copy keeps both as empty folders.
