# tna_basic_switch test

## Introduction

This tutorial shows how to compile a p4-16 program (tna_basic_switch), install it, configure it and test it during run time.

## Topo

    |----------------------------------|            |---------------------------|             |-----------------------------------|
    |  ionesco 192.168.100.1(enp8s0f1) | ---------- | (24)tna_basic_switch(20)  |-------------|  tzara 192.168.100.2(enp8s0f1)    |
    |----------------------------------|            |---------------------------|             |-----------------------------------|

* Please verify the above physical connection every time you use this tutorial.
* Please manually add arp entry for host `ionesco` and `tzara` so that they know each other's MAC address.

## Compilation and Installation

You should add your program to `$SDE/pkgsrc/p4-examples/p4_16_programs/`. Every time you add / modify a program (i.e., tna_basic_switch). Do the following:

```
cd $SDE/build/p4-examples
$SDE/pkgsrc/p4-build/configure --with-tofino --with-p4c=p4c --prefix=$SDE_INSTALL --bindir=$SDE_INSTALL/bin P4_NAME=tna_basic_switch P4_PATH=$SDE/pkgsrc/p4-examples/p4_16_programs/tna_basic_switch/tna_basic_switch.p4 P4_VERSION=p4-16 P4_ARCHITECTURE=tna LDFLAGS="-L$SDE_INSTALL/lib"
make
make install
```

## Configuration

```
cd $SDE
./run_switchd.sh --arch tofino -p tna_basic_switch
### sde cmd from now on ####
bfshell> ucli
bf-sde> pm
bf-sde.pm> port-add 24/- 100G NONE
bf-sde.pm> port-add 20/- 100G NONE
bf-sde.pm> an-set 24/- 2
bf-sde.pm> an-set 20/- 2
bf-sde.pm> port-enb 24/-
bf-sde.pm> port-enb 20/-
bf-sde.pm> show
-----+----+---+----+------+----+---+---+---+--------+----------------+----------------+-
PORT |MAC |D_P|P/PT|SPEED |FEC |RDY|ADM|OPR|LPBK    |FRAMES RX       |FRAMES TX       |E
-----+----+---+----+------+----+---+---+---+--------+----------------+----------------+-
20/0 | 4/0| 24|1/24| 100G |NONE|YES|ENB|UP |  NONE  |               5|               0|
24/0 | 0/0| 56|1/56| 100G |NONE|YES|ENB|UP |  NONE  |               3|               0|
```

Note that there should be 2 ports with RDY=YES, ADM=ENB and OPR=UP like the above.
Then add entries to the table, run:

```
bf-sde.pm> ..
bf-sde> exit
bfshell> bfrt_python
bfrt_root>bfrt
bfrt>tna_basic_switch
tna_basic_switch>pipe.SwitchIngress.dmac
dmac>add_with_dmac_hit(dst_addr=0x98039b9b55fb, port=56)
dmac>add_with_dmac_hit(dst_addr=0x98039b9b55ab, port=24)
dmac>quit
bfshell>quit
```

## Test with ICMP ping

SSH to `ionesco`, run

```
ping 192.168.100.2
```

## Test with RDMA packet

You could use 4 commands to send RDMA packets: `ib_read_lat`, `ib_read_bw`, `ib_write_lat` and `ib_write_bw`.

We provide 2 test examples.

#### ib_read_lat

On `ionesco`, run
```
ib_read_lat -n 10000 -F -s 2048 -m 256 -c RC -d mlx5_1 -i 1 -p 9001 192.168.100.2
```
On `tzara`, run
```
ib_read_lat -d mlx5_1 -i 1 -p 9001
```

#### ib_write_bw

On `ionesco`, run
```
ib_write_bw -n 10000 -F -s 2048 -m 256 -c RC -d mlx5_1 -i 1 -p 9001 192.168.100.2
```
On `tzara`, run
```
ib_write_bw -d mlx5_1 -i 1 -p 9001
```
