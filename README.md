# ebpf-playground

## Setting up environment

### Prerequisites

- go 1.19+

- make
- llvm
- clang
- bpftool
- libelf-dev

If you are running on Ubuntu, you can install those tools with:

```bash
apt update
apt install -y build-essential linux-tools-common linux-tools-generic libelf-dev llvm clang
```

### Clone submodules

```bash
git submodule update --init --recursive
```

### Build

```bash
make build
```

On success, the built program is located in `./bin/exec`

### Run

```bash
sudo ./bin/exec
```

It should capture exec syscall and print the information to stdout.

### Troubleshooting build

#### Build error with message unknown type

```
In file included from cmd/exec/c/capture_exec.bpf.c:3:
In file included from /root/ebpf-playground/output/bpf/bpf_helpers.h:11:
/root/ebpf-playground/output/bpf/bpf_helper_defs.h:77:83: error: unknown type name '__u64'
static long (*bpf_map_update_elem)(void *map, const void *key, const void *value, __u64 flags) = (void *) 2;
                                                                                  ^
/root/ebpf-playground/output/bpf/bpf_helper_defs.h:101:42: error: unknown type name '__u32'
static long (*bpf_probe_read)(void *dst, __u32 size, const void *unsafe_ptr) = (void *) 4;
                                         ^
/root/ebpf-playground/output/bpf/bpf_helper_defs.h:113:16: warning: type specifier missing, defaults to 'int' [-Wimplicit-int]
static __u64 (*bpf_ktime_get_ns)(void) = (void *) 5;
               ^
```

Check ./output/vmlinux.h

If that file is empty, you need to install the correct kernel header. It could look like this, depending on your kernel version:

```
sudo apt install -y linux-tools-5.15.0-58-generic
```

