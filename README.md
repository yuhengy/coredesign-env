This env is created by

```shell
git submodule add https://github.com/OSCPU/nemu.git
cd nemu && git checkout 85edc31 && cd ..
git commit -m "NEMU: diff test trace"
git submodule add https://github.com/riscv/riscv-gnu-toolchain.git
cd riscv-gnu-toolchain && git checkout rvv-0.8 && cd ..
git submodule add https://github.com/riscv/riscv-tests.git
cd riscv-tests && git checkout a5d8386 && cd ..
git commit -m "TESTBENCH: use official riscv-tests"
```

To init this repo, use
```shell
git clone https://github.com/yuhengy/coredesign-env.git
cd coredesign-env
git submodule update --init --depth=1
cd riscv-gnu-toolchain && git rm -f qemu && cd ..  # repo bug, the qemu commit was deleted
git submodule update --init --recursive --depth=1
```

