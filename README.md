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
- [x] Q3: Config last level cache to 2-way and full-way associative ache and test performance.
- [x] Q4: Modify last level cache policy based on frequency based replacement policy
- [ ] Q5: Test the performance of write back and write through policy based on 4-way associatve cache with isscc_pcm  
- Bonus
    - [ ] Design last level cache policy to reduce the energy consumption of pcm_based main memory
    - [ ] Baseline: LRU
## Q1
按照教學講義

指令
```
./build/X86/gem5.opt configs/example/se.py   -c tests/test-progs/hello/bin/x86/linux/hello   --cpu-type=TimingSimpleCPU   --caches   --l2cache   --mem-type=NVMainMemory   --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```
## Q2
修改的檔案 在gem5 資料夾中的
* Options.py 
* Caches.py 
* Xbar.py 
* BaseCPU.py 
* CacheConfig.py 
- 前面四個檔案只是增加 L3 cache 的parameter ，照著 L2 cache的設定去做模仿就可以。
- CacheConfig.py  需要讓 L3 cache 連接整個 Gem5 系統，這邊要注意 L2 跟L3 這兩個  cache的關係， 要讓系統 在已使用 L2 cache的情況下才能使用 L3 cache

指令
```
./build/X86/gem5.opt configs/example/se.py -c tests/test-progs/hello/bin/x86/linux/hello --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```
觀察指令
```
grep -i l3 m5out/stats.txt
```
## Q3 Config last level cache to 2-way and full-way associative ache and test performance.
2-way : --l3_assoc=2
```
./build/X86/gem5.opt configs/example/se.py -c ./quicksort --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --l3_assoc=2 --l1i_size=32kB --l1d_size=32kB --l2_size=128kB --l3_size=1MB --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```
full-way : l3_assoc = 16384 (l3_size_bytes // block_size)
```
./build/X86/gem5.opt configs/example/se.py -c ./quicksort --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --l3_assoc=16384 --l1i_size=32kB --l1d_size=32kB --l2_size=128kB --l3_size=1MB --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```

查看 miss rate
```
grep 'system.l3.*miss_rate' m5out/stats.txt
```
查看 assoc
```
grep -A 10 "\[system.l3\]" m5out/config.ini
```
- two
![image](https://github.com/user-attachments/assets/96a8d2db-c7ac-4b96-b824-6e357e9ac1a0)

- full -16384
![image](https://github.com/user-attachments/assets/b02689d1-8191-47ee-b01d-d0a14aa476ea)

觀察 system.l3.overall_miss_rate

## Q4 Modify last level cache policy based on frequency based replacement policy

在~/gem5-525ce650e1a5bbe71c39d4b15598d6c003cc9f9e/configs/common$ nano Caches.py 加在L3
```
replacement_policy = Param.BaseReplacementPolicy(LFURP(), "Replacement policy")
```
觀看指令
```
grep -A 5 "\[system.l3.replacement_policy\]" m5out/config.ini
```
![image](https://github.com/user-attachments/assets/38b2e040-42f1-4f03-9451-d9ed873d4f34)

觀察 : system.l3.replacements會變低

## Q5 Test the performance of write back and write through policy based on 4-way associatve cache with isscc_pcm  
4-way : --l3_assoc=4

改base.cc

我也搞不懂 ?

```
./build/X86/gem5.opt configs/example/se.py -c ./multiply --cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --l3_assoc=4 --l1i_size=32kB --l1d_size=32kB --l2_size=128kB --l3_size=1MB --mem-type=NVMainMemory --nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```
觀察 i0.defaultMemory.totalWriteRequests
## Bouns
