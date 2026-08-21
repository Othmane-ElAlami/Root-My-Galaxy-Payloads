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
