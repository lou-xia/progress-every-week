## rust-analyzer报错修复

### 报错分析：
<details>
<summary>  
rust-analyzer 报错信息
</summary>

```bash
2026-01-04T16:21:56.416797407+08:00 ERROR FetchBuildDataError: error: failed to run custom build command for `lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc)`

Caused by:
  process didn't exit successfully: `/home/userName/userName/StarryOS/StarryOS/target/debug/build/lwext4_rust-c426d75e6e8d0f9f/build-script-build` (exit status: 101)
  --- stdout
  make: Entering directory '/root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/c/lwext4'
  rm -R -f build_musl-generic
  mkdir build_musl-generic
  cd build_musl-generic && cmake -G"Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DVERSION_MAJOR=1 -DVERSION_MINOR=0 -DVERSION_PATCH=0 -DVERSION=1.0.0-0fd7a3b -DLWEXT4_BUILD_SHARED_LIB=OFF -DLWEXT4_ULIBC=ON -DCMAKE_C_COMPILER_WORKS=1 -DCMAKE_INSTALL_PREFIX=./install   -DCMAKE_TOOLCHAIN_FILE=../toolchain/musl-generic.cmake ..
  -- The C compiler identification is unknown
  -- Configuring incomplete, errors occurred!
  make: Leaving directory '/root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/c/lwext4'

  --- stderr
  CMake Error at CMakeLists.txt:2 (project):
    The CMAKE_C_COMPILER:

      riscv64-linux-musl-cc

    is not a full path and was not found in the PATH.

    Tell CMake where to find the compiler by setting either the environment
    variable "CC" or the CMake cache entry CMAKE_C_COMPILER to the full path to
    the compiler, or to the compiler name if it is in the PATH.


  make: *** [Makefile:39: musl-generic] Error 1

  thread 'main' (30156) panicked at /root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/build.rs:26:9:
  assertion failed: status.success()
  stack backtrace:
     0: __rustc::rust_begin_unwind
               at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:689:5
     1: core::panicking::panic_fmt
               at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/panicking.rs:80:14
     2: core::panicking::panic
               at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/panicking.rs:150:5
     3: build_script_build::main
               at ./build.rs:26:9
     4: <fn() as core::ops::function::FnOnce<()>>::call_once
               at /root/.rustup/toolchains/nightly-2025-12-12-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.


2026-01-04T16:22:02.739668459+08:00 ERROR Flycheck failed to run the following command: CommandHandle { program: "/root/.cargo/bin/cargo", arguments: ["check", "--workspace", "--message-format=json-diagnostic-rendered-ansi", "--manifest-path", "/home/userName/userName/StarryOS/StarryOS/Cargo.toml", "--keep-going", "--target", "riscv64gc-unknown-none-elf", "--features", "qemu"], current_dir: Some("/home/userName/userName/StarryOS/StarryOS") }, error=Cargo watcher failed, the command produced no valid metadata (exit code: ExitStatus(unix_wait_status(25856))):
   0.217621987s  INFO prepare_target{force=false package_id=starry v0.1.0 (/home/userName/userName/StarryOS/StarryOS) target="starry"}: cargo::core::compiler::fingerprint: fingerprint error for starry v0.1.0 (/home/userName/userName/StarryOS/StarryOS)/Check { test: false }/TargetInner { name: "starry", doc: true, ..: with_path("/home/userName/userName/StarryOS/StarryOS/src/main.rs", Edition2024) }
   0.217698288s  INFO prepare_target{force=false package_id=starry v0.1.0 (/home/userName/userName/StarryOS/StarryOS) target="starry"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/starry-df0b03317888d8c3/bin-starry`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: <cargo::core::compiler::build_runner::BuildRunner>::compile
   6: cargo::ops::cargo_compile::compile_ws
   7: cargo::ops::cargo_compile::compile_with_exec
   8: cargo::ops::cargo_compile::compile
   9: cargo::commands::check::exec
  10: <cargo::cli::Exec>::exec
  11: cargo::main
  12: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  13: std::rt::lang_start::<()>::{closure#0}
  14: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  15: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  16: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  17: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  18: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  19: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  20: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  21: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  22: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  23: main
  24: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  25: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  26: <unknown>
   0.359555837s  INFO prepare_target{force=false package_id=axfeat v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/api/axfeat) target="axfeat"}: cargo::core::compiler::fingerprint: fingerprint error for axfeat v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/api/axfeat)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("axfeat", ["lib"], "/home/userName/userName/StarryOS/StarryOS/arceos/api/axfeat/src/lib.rs", Edition2024) }
   0.359600011s  INFO prepare_target{force=false package_id=axfeat v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/api/axfeat) target="axfeat"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/axfeat-ca2c3bea208296ac/lib-axfeat`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: <cargo::core::compiler::build_runner::BuildRunner>::compile
   7: cargo::ops::cargo_compile::compile_ws
   8: cargo::ops::cargo_compile::compile_with_exec
   9: cargo::ops::cargo_compile::compile
  10: cargo::commands::check::exec
  11: <cargo::cli::Exec>::exec
  12: cargo::main
  13: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  14: std::rt::lang_start::<()>::{closure#0}
  15: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  16: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  17: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  18: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  19: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  20: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  21: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  22: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  23: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  24: main
  25: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  26: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  27: <unknown>
   0.365748043s  INFO prepare_target{force=false package_id=axfs v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axfs) target="axfs"}: cargo::core::compiler::fingerprint: fingerprint error for axfs v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axfs)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("axfs", ["lib"], "/home/userName/userName/StarryOS/StarryOS/arceos/modules/axfs/src/lib.rs", Edition2024) }
   0.365794892s  INFO prepare_target{force=false package_id=axfs v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axfs) target="axfs"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/axfs-764014c168aff4c2/lib-axfs`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: cargo::core::compiler::compile
   7: <cargo::core::compiler::build_runner::BuildRunner>::compile
   8: cargo::ops::cargo_compile::compile_ws
   9: cargo::ops::cargo_compile::compile_with_exec
  10: cargo::ops::cargo_compile::compile
  11: cargo::commands::check::exec
  12: <cargo::cli::Exec>::exec
  13: cargo::main
  14: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  15: std::rt::lang_start::<()>::{closure#0}
  16: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  17: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  18: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  19: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  20: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  21: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  22: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  23: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  24: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  25: main
  26: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  27: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  28: <unknown>
   0.373548686s  INFO prepare_target{force=false package_id=lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc) target="lwext4_rust"}: cargo::core::compiler::fingerprint: fingerprint error for lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("lwext4_rust", ["lib"], "/root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/src/lib.rs", Edition2024) }
   0.373613924s  INFO prepare_target{force=false package_id=lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc) target="lwext4_rust"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/lwext4_rust-b3c1dbcf3be6651a/lib-lwext4_rust`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: cargo::core::compiler::compile
   7: cargo::core::compiler::compile
   8: <cargo::core::compiler::build_runner::BuildRunner>::compile
   9: cargo::ops::cargo_compile::compile_ws
  10: cargo::ops::cargo_compile::compile_with_exec
  11: cargo::ops::cargo_compile::compile
  12: cargo::commands::check::exec
  13: <cargo::cli::Exec>::exec
  14: cargo::main
  15: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  16: std::rt::lang_start::<()>::{closure#0}
  17: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  18: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  19: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  20: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  21: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  22: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  23: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  24: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  25: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  26: main
  27: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  28: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  29: <unknown>
   0.374188607s  INFO prepare_target{force=false package_id=lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc) target="build-script-build"}: cargo::core::compiler::fingerprint: fingerprint error for lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc)/RunCustomBuild/TargetInner { ..: custom_build_target("build-script-build", "/root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/build.rs", Edition2024) }
   0.374201092s  INFO prepare_target{force=false package_id=lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc) target="build-script-build"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/lwext4_rust-3d599cdb38123c4c/run-build-script-build-script-build`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::custom_build::build_work
   5: cargo::core::compiler::compile
   6: cargo::core::compiler::compile
   7: cargo::core::compiler::compile
   8: cargo::core::compiler::compile
   9: cargo::core::compiler::compile
  10: <cargo::core::compiler::build_runner::BuildRunner>::compile
  11: cargo::ops::cargo_compile::compile_ws
  12: cargo::ops::cargo_compile::compile_with_exec
  13: cargo::ops::cargo_compile::compile
  14: cargo::commands::check::exec
  15: <cargo::cli::Exec>::exec
  16: cargo::main
  17: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  18: std::rt::lang_start::<()>::{closure#0}
  19: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  20: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  21: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  22: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  23: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  24: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  25: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  26: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  27: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  28: main
  29: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  30: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  31: <unknown>
   0.378152032s  INFO prepare_target{force=false package_id=axnet v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axnet) target="axnet"}: cargo::core::compiler::fingerprint: fingerprint error for axnet v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axnet)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("axnet", ["lib"], "/home/userName/userName/StarryOS/StarryOS/arceos/modules/axnet/src/lib.rs", Edition2024) }
   0.378187719s  INFO prepare_target{force=false package_id=axnet v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axnet) target="axnet"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/axnet-28079d9cfbb7f785/lib-axnet`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: cargo::core::compiler::compile
   7: <cargo::core::compiler::build_runner::BuildRunner>::compile
   8: cargo::ops::cargo_compile::compile_ws
   9: cargo::ops::cargo_compile::compile_with_exec
  10: cargo::ops::cargo_compile::compile
  11: cargo::commands::check::exec
  12: <cargo::cli::Exec>::exec
  13: cargo::main
  14: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  15: std::rt::lang_start::<()>::{closure#0}
  16: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  17: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  18: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  19: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  20: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  21: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  22: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  23: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  24: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  25: main
  26: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  27: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  28: <unknown>
   0.382109932s  INFO prepare_target{force=false package_id=axruntime v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axruntime) target="axruntime"}: cargo::core::compiler::fingerprint: fingerprint error for axruntime v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axruntime)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("axruntime", ["lib"], "/home/userName/userName/StarryOS/StarryOS/arceos/modules/axruntime/src/lib.rs", Edition2024) }
   0.382150224s  INFO prepare_target{force=false package_id=axruntime v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axruntime) target="axruntime"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/axruntime-c2c3f90d71eef176/lib-axruntime`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: cargo::core::compiler::compile
   7: <cargo::core::compiler::build_runner::BuildRunner>::compile
   8: cargo::ops::cargo_compile::compile_ws
   9: cargo::ops::cargo_compile::compile_with_exec
  10: cargo::ops::cargo_compile::compile
  11: cargo::commands::check::exec
  12: <cargo::cli::Exec>::exec
  13: cargo::main
  14: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  15: std::rt::lang_start::<()>::{closure#0}
  16: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  17: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  18: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  19: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  20: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  21: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  22: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  23: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  24: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  25: main
  26: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  27: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  28: <unknown>
   0.383046236s  INFO prepare_target{force=false package_id=axmm v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axmm) target="axmm"}: cargo::core::compiler::fingerprint: fingerprint error for axmm v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axmm)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("axmm", ["lib"], "/home/userName/userName/StarryOS/StarryOS/arceos/modules/axmm/src/lib.rs", Edition2024) }
   0.383079047s  INFO prepare_target{force=false package_id=axmm v0.2.0 (/home/userName/userName/StarryOS/StarryOS/arceos/modules/axmm) target="axmm"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/axmm-643bc0ed68bb4c4b/lib-axmm`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: cargo::core::compiler::compile
   7: cargo::core::compiler::compile
   8: <cargo::core::compiler::build_runner::BuildRunner>::compile
   9: cargo::ops::cargo_compile::compile_ws
  10: cargo::ops::cargo_compile::compile_with_exec
  11: cargo::ops::cargo_compile::compile
  12: cargo::commands::check::exec
  13: <cargo::cli::Exec>::exec
  14: cargo::main
  15: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  16: std::rt::lang_start::<()>::{closure#0}
  17: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  18: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  19: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  20: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  21: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  22: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  23: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  24: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  25: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  26: main
  27: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  28: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  29: <unknown>
   0.384715503s  INFO prepare_target{force=false package_id=starry-api v0.1.0 (/home/userName/userName/StarryOS/StarryOS/api) target="starry_api"}: cargo::core::compiler::fingerprint: fingerprint error for starry-api v0.1.0 (/home/userName/userName/StarryOS/StarryOS/api)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("starry_api", ["lib"], "/home/userName/userName/StarryOS/StarryOS/api/src/lib.rs", Edition2024) }
   0.384751325s  INFO prepare_target{force=false package_id=starry-api v0.1.0 (/home/userName/userName/StarryOS/StarryOS/api) target="starry_api"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/starry-api-0e2b1fb665feb29c/lib-starry_api`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: <cargo::core::compiler::build_runner::BuildRunner>::compile
   7: cargo::ops::cargo_compile::compile_ws
   8: cargo::ops::cargo_compile::compile_with_exec
   9: cargo::ops::cargo_compile::compile
  10: cargo::commands::check::exec
  11: <cargo::cli::Exec>::exec
  12: cargo::main
  13: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  14: std::rt::lang_start::<()>::{closure#0}
  15: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  16: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  17: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  18: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  19: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  20: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  21: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  22: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  23: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  24: main
  25: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  26: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  27: <unknown>
   0.388224319s  INFO prepare_target{force=false package_id=starry-core v0.1.0 (/home/userName/userName/StarryOS/StarryOS/core) target="starry_core"}: cargo::core::compiler::fingerprint: fingerprint error for starry-core v0.1.0 (/home/userName/userName/StarryOS/StarryOS/core)/Check { test: false }/TargetInner { name_inferred: true, ..: lib_target("starry_core", ["lib"], "/home/userName/userName/StarryOS/StarryOS/core/src/lib.rs", Edition2024) }
   0.388261019s  INFO prepare_target{force=false package_id=starry-core v0.1.0 (/home/userName/userName/StarryOS/StarryOS/core) target="starry_core"}: cargo::core::compiler::fingerprint:     err: failed to read `/home/userName/userName/StarryOS/StarryOS/target/riscv64gc-unknown-none-elf/debug/.fingerprint/starry-core-d8e297f350fc4091/lib-starry_core`

Caused by:
    No such file or directory (os error 2)

Stack backtrace:
   0: cargo_util::paths::read_bytes
   1: cargo_util::paths::read
   2: cargo::core::compiler::fingerprint::_compare_old_fingerprint
   3: cargo::core::compiler::fingerprint::prepare_target
   4: cargo::core::compiler::compile
   5: cargo::core::compiler::compile
   6: cargo::core::compiler::compile
   7: <cargo::core::compiler::build_runner::BuildRunner>::compile
   8: cargo::ops::cargo_compile::compile_ws
   9: cargo::ops::cargo_compile::compile_with_exec
  10: cargo::ops::cargo_compile::compile
  11: cargo::commands::check::exec
  12: <cargo::cli::Exec>::exec
  13: cargo::main
  14: std::sys::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  15: std::rt::lang_start::<()>::{closure#0}
  16: <&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/ops/function.rs:287:21
  17: std::panicking::catch_unwind::do_call::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  18: std::panicking::catch_unwind::<i32, &dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  19: std::panic::catch_unwind::<&dyn core::ops::function::Fn<(), Output = i32> + core::panic::unwind_safe::RefUnwindSafe + core::marker::Sync, i32>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  20: std::rt::lang_start_internal::{closure#0}
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:175:24
  21: std::panicking::catch_unwind::do_call::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:581:40
  22: std::panicking::catch_unwind::<isize, std::rt::lang_start_internal::{closure#0}>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:544:19
  23: std::panic::catch_unwind::<std::rt::lang_start_internal::{closure#0}, isize>
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panic.rs:359:14
  24: std::rt::lang_start_internal
             at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/rt.rs:171:5
  25: main
  26: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  27: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  28: <unknown>
   Compiling lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc)
error: failed to run custom build command for `lwext4_rust v0.2.0 (https://github.com/Starry-OS/lwext4_rust.git?rev=033fa2c#033fa2cc)`

Caused by:
  process didn't exit successfully: `/home/userName/userName/StarryOS/StarryOS/target/debug/build/lwext4_rust-c426d75e6e8d0f9f/build-script-build` (exit status: 101)
  --- stdout
  make: Entering directory '/root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/c/lwext4'
  rm -R -f build_musl-generic
  mkdir build_musl-generic
  cd build_musl-generic && cmake -G"Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DVERSION_MAJOR=1 -DVERSION_MINOR=0 -DVERSION_PATCH=0 -DVERSION=1.0.0-0fd7a3b -DLWEXT4_BUILD_SHARED_LIB=OFF -DLWEXT4_ULIBC=ON -DCMAKE_C_COMPILER_WORKS=1 -DCMAKE_INSTALL_PREFIX=./install   -DCMAKE_TOOLCHAIN_FILE=../toolchain/musl-generic.cmake ..
  -- The C compiler identification is unknown
  -- Configuring incomplete, errors occurred!
  make: Leaving directory '/root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/c/lwext4'

  --- stderr
  CMake Error at CMakeLists.txt:2 (project):
    The CMAKE_C_COMPILER:

      riscv64-linux-musl-cc

    is not a full path and was not found in the PATH.

    Tell CMake where to find the compiler by setting either the environment
    variable "CC" or the CMake cache entry CMAKE_C_COMPILER to the full path to
    the compiler, or to the compiler name if it is in the PATH.


  make: *** [Makefile:39: musl-generic] Error 1

  thread 'main' (31243) panicked at /root/.cargo/git/checkouts/lwext4_rust-54dccac8b9756226/033fa2c/build.rs:26:9:
  assertion failed: status.success()
  stack backtrace:
     0: __rustc::rust_begin_unwind
               at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/std/src/panicking.rs:689:5
     1: core::panicking::panic_fmt
               at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/panicking.rs:80:14
     2: core::panicking::panic
               at /rustc/f5209000832c9d3bc29c91f4daef4ca9f28dc797/library/core/src/panicking.rs:150:5
     3: build_script_build::main
               at ./build.rs:26:9
     4: <fn() as core::ops::function::FnOnce<()>>::call_once
               at /root/.rustup/toolchains/nightly-2025-12-12-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

```

</details>

#### 分析原因

通过查询资料，分析是rust-analyzer在编译lwext4_rust时，由于CMakeLists.txt中未指定C编译器，导致CMake无法正常配置。进而导致 fingerprint 和 Flycheck 也出现报错。

### 尝试1：终端指定编译器

在 Starry-OS 的 Readme 中提到了安装工具链，并且导入环境变量：`export PATH=/opt/riscv64-linux-musl-cross/bin:$PATH`。

实操效果是：`make run`能够成功运行，但是 rust-analyzer 仍然报错。

#### 分析原因

rust-analyzer 的编译分析是在 bash 运行前就开始了，而手动在终端输入`export PATH=...`只是对当前终端进程以及其子进程生效，与 rust-analyzer 无关。

### 尝试2：在全局环境变量添加

在`~/.bashrc`中添加`export PATH=/opt/riscv64-linux-musl-cross/bin:$PATH`，重新打开 vs code，发现 rust-analyzer 仍然报错。

#### 分析原因

查找资料发现，rust-analyzer 会读取上次编译的缓存。由于上次编译运行时，`make run`成功运行，cargo 认为上次的缓存是有效的。但实际上上次缓存的数据中的路径是终端临时的，刷新窗口后就失效，因此还是会找不到编译器，再次报错。

### 最终解决方案

1. 清除 cargo 缓存`cargo clean`
2. 在`~/.bashrc`中添加`export PATH=/opt/riscv64-linux-musl-cross/bin:$PATH`
3. 重新打开 vs code，运行`make run`。