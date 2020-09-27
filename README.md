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
git submodule update --init --recursive coredesign learn-riscv-note nemu riscv-tests

cd coredesign && git checkout master && cd ..
cd learn-riscv-note && git checkout master && cd ..
```

To demo the latest test, run `bash run_from_nothing.sh coredesign/test-scripts-forcommits/{latest-test}`

### Dev

To init this repository for development, in addition to demonstration, use
```shell
git submodule update --init --depth=1 riscv-gnu-toolchain
cd riscv-gnu-toolchain && git rm -f qemu && cd ..  # repo bug, the qemu commit was deleted
git submodule update --init --recursive --depth=1 riscv-gnu-toolchain
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
```

This also include my repository:
```shell
git submodule add https://github.com/yuhengy/coredesign.git
git submodule add https://github.com/yuhengy/learn-riscv-note.git
```


