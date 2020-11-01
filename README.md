This repo extract the env and dependence from [coredesign](https://github.com/yuhengy/coredesign.git) to simplify its structure. In summary, this repo includes:
+ `Dockerfile` and `docker-compose.yml` to create the demo docker container
+ dependent repo for demo
+ dependent repo for development
+ demo script `run_from_nothing.sh`

### Demo

To init this repository for demonstration, use
```shell
git clone https://github.com/yuhengy/coredesign-env.git
cd coredesign-env
git submodule update --init --recursive coredesign learn-riscv-note nemu riscv-tests nexus-am RT-Thread
# Add these into `nemu/src/monitor/difftest/ref.c`: 
  add_mmio_map("difftest.serial", 0x40600000, new_space(32), 32, NULL);
  add_mmio_map("difftest.mtime", 0x4070BFF8, new_space(8), 8, NULL);
  add_mmio_map("difftest.mtimecmp", 0x40704000, new_space(8), 8, NULL);

cd coredesign && git checkout master && cd ..
cd learn-riscv-note && git checkout master && cd ..
```

TODO: for nexus-am, you need manually modify `src/nutshell/common/uartlite` by comment all function body except `return 0;`

To demo the latest test, run `bash run_from_nothing.sh coredesign/test-scripts-forcommits/{latest-test}`

### Dev

To init this repository for development, in addition to demonstration, use
```shell
git submodule update --init --depth=1 riscv-gnu-toolchain
cd riscv-gnu-toolchain && git rm -f qemu && cd ..  # repo bug, the qemu commit was deleted
git submodule update --init --recursive --depth=1 riscv-gnu-toolchain
git submodule update --init --recursive verilator
```

Common commands to use docker-compose:
```shell
docker-compose up -d
docker-compose exec my_env bash
docker-compose stop
docker-compose down --rmi all
```

### How is this env repo created

This environment is created by

```shell
git submodule add https://github.com/OSCPU/nemu.git
cd nemu && git checkout 85edc31 && cd ..
git commit -m "NEMU: diff test trace"

git submodule add https://github.com/riscv/riscv-gnu-toolchain.git
cd riscv-gnu-toolchain && git checkout rvv-0.8 && cd ..
git submodule add https://github.com/riscv/riscv-tests.git
cd riscv-tests && git checkout a5d8386 && cd ..
git commit -m "TESTBENCH: use official riscv-tests"

git submodule add https://github.com/OSCPU/nexus-am.git
cd nexus-am && git checkout b2e6303 && cd ..
git commit -m "TESTBENCH: use am tests"

git submodule add https://github.com/OSCPU/RT-Thread.git
cd RT-Thread && git checkout a0ffe63 && cd ..
# comment Line29 in `RT-Thread/bsp/riscv64/Makefile`
# change Line13 in `RT-Thread/bsp/riscv64/rtconfig.py` into `EXEC_PATH   = r'/usr/bin'`
# change `-march=rv64imac` Line43 in `RT-Thread/bsp/riscv64/rtconfig.py` into `-march=rv64im`
git commit -m "RT-THREAD: clone from OSCPU"

git submodule add https://github.com/verilator/verilator.git
cd verilator && git checkout 77553d2 && cd ..
git commit -m "VERILATOR: new version other than apt, no `Floating point exception` bug"
```

This also include my repository:
```shell
git submodule add https://github.com/yuhengy/coredesign.git
git submodule add https://github.com/yuhengy/learn-riscv-note.git
```


