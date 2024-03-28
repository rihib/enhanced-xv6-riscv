# Enhanced xv6-riscv

## Make file system faster

One of the performance challenges of traditional file systems is inefficient path walking: file systems such as Linux require sequential retrieval of metadata from disk for each path component when the cache is cold.

With the recent emergence of storage devices that can be accessed with DRAM latency, traditional file systems can no longer take advantage of that performance.

Therefore, I have newly modified the file system to maintain the relationship between paths and metadata using KVS and to make metadata retrieval super-fast than before.

## Add multiuser function

It works as follows;

```
% make
% make qemu
# cat group
root:x:0:,root
# cat passwd
root:x:0:0
# groupadd group0
# cat group
root:x:0:,root
group0:x:1:
# adduser user0 group0
# cat passwd
root:x:0:0
user0:x:1:1
# cat group
root:x:0:,root
group0:x:1:,user0
# su user0
user0$ cat group
cat: cannot open group
user0$ cat passwd
cat: cannot open passwd
user0$ ls
ls: cannot open .
user0$ su root
# ls
.              1 1 1024
..             1 1 1024
README         2 2 2305
group          2 3 33
passwd         2 4 23
adduser        2 5 48224
cat            2 6 33096
echo           2 7 31960
forktest       2 8 16016
grep           2 9 36536
grind          2 10 47736
groupadd       2 11 36736
init           2 12 32440
kill           2 13 31888
ln             2 14 31712
ls             2 15 35064
mkdir          2 16 31952
rm             2 17 31936
sh             2 18 60368
stressfs       2 19 32816
su             2 20 36640
usertests      2 21 180576
wc             2 22 34016
zombie         2 23 31312
console        3 24 0
# 
```
