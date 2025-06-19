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
- [x] Q1: Gem5 + NVmain Build-up
- [x] Q2: Enable L3 last level cache in GEM5 + NVmain
- [ ] Q3: Config last level cache to 2-way and full and full-way associative ache and test performance.
- [ ] Q4: Modify last level cache policy based on frequency based replacement policy
- [ ] Q5: Test the performance of write back and write through policy based on 4-wau associatve cache with isscc_pcm  
- Bonus
    - [ ] Design last level cache policy to reduce the energy consumption of pcm_based main memory
    - [ ] Baseline: LRU
## Q1

```
./build/X86/gem5.opt configs/example/se.py   -c tests/test-progs/hello/bin/x86/linux/hello   --cpu-type=TimingSimpleCPU   --caches   --l2cache   --mem-type=NVMainMemory   --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config

```
![image](https://github.com/user-attachments/assets/5611c29e-c3c5-4e44-bf10-20f5ec570314)

## Q2
### 提示
需要修改的檔案 在gem5gem5gem5 資料夾中的
* Options.py 
* Caches.py 
* Xbar.py 
* BaseCPU.py 
* CacheConfig.py 
- 前面四個檔案只是增加 L3 cache 的parameter ，照著 L2 cache的設定去做模仿就可以。
- CacheConfig.py  需要讓 L3 cache 連接整個 Gem5 系統，這邊要注意 L2 跟L3 這兩個  cache的關係， 要讓系統 在已使用 L2 cache的情況下才能使用 L3 cache
  "/n"測試指令
```
./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```

## Q3

## Q4

## Q5

## Bouns
