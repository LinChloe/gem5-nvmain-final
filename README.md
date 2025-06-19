# gem5-nvmain-final
計組期末專案

更改 Gem5 中的檔案之後要記得編譯
```
scons EXTRAS=../NVmain build/X86/gem5.opt
```
常用指令
```
cd gem5-525ce650e1a5bbe71c39d4b15598d6c003cc9f9e
```
```
git add .
```
```
git commit -m "修改內容"
```
```
git push
```
## TODOs
- [x] Q0: Gem5 + NVmain Build-up
- [ ] Q1: Enable L3 last level cache in GEM5 + NVmain
- [ ] Q2: Config last level cache to 2-way and full and full-way associative ache and test performance.
- [ ] Q3: Modify last level cache policy based on frequency based replacement policy
- [ ] Q4: Test the performance of write back and write through policy based on 4-wau associatve cache with isscc_pcm  
- Bonus
    - [ ] Design last level cache policy to reduce the energy consumption of pcm_based main memory
    - [ ] Baseline: LRU
## Q0

```
./build/X86/gem5.opt configs/example/se.py   -c tests/test-progs/hello/bin/x86/linux/hello   --cpu-type=TimingSimpleCPU   --caches   --l2cache   --mem-type=NVMainMemory   --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config

```
![image](https://github.com/user-attachments/assets/5611c29e-c3c5-4e44-bf10-20f5ec570314)

## Q1

## Q2

## Q3

## Q4

## Q
