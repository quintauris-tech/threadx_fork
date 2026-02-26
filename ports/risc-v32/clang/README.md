# RISCV32 clang port

This is basically a copy of the RISC64 gnu port.
The only major modification was changing the load double word (ld) 
and store double double (sd) word with load word (ld) and store word (sd).

I also added support for semihosting so the example can be executed on QEMU.

## How to build

cd to the folder where this repo is cloned and run the following commands:

```
cd /threadx/ports/risc-v32/clang/example_build/qemu_virt
./build_libthreadx.sh
./build_threadx_sample.sh
```

The first script will build the ThreadX libraries.
You can find the library in <threadx repo>/build/libthreadx.a.

The second script will build the demo application.
You can find the demo application in <threadx repo>/ports/risc-v32/clang/example_build/qemu_virt/build/demo_threadx.elf

## How to run using QEMU

cd to the folder where this repo is cloned and run the following command:

```
docker run --rm -it -p 1234:1234 -v $(pwd):/threadx -w /threadx ghcr.io/quintauris-tech/qemu-system-riscv32-v10:latest bash
```

The commands assumes that this repo is clone into a folder named "threadx"

```
cd /threadx/ports/risc-v32/clang/example_build/qemu_virt

qemu-system-riscv32 -machine virt -m 16M -bios ./build/demo_threadx.elf -display none -chardev stdio,id=stdio0 -semihosting-config enable=on,userspace=on,chardev=stdio0  -gdb tcp::1234
```

This should print output from different threads. In the QEMU output you should see output like the following:
 
```
[Thread] : thread_xxxx_entry is here!
```
 
You can use option -S with qemu-system-riscv32 to debug.
 
In this case run debugger as /opt/riscv_rv32ima_zicsr/bin/riscv32-unknown-elf-gdb <repo>/ports/risc-v32/gnu/example_build/qemu_virt/build/demo_threadx.elf
 
```
target remote :1234
```

to connect to the target. Enter 'c' to continue execution.

 
 