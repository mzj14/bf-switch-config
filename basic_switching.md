# basic_switching test

## Topo

    |-----------------------|           |-----------------|             |-----------------------|
    |  pinter 192.168.1.10  |---------- | basic_switching |-------------|  jarry 192.168.1.11  |
    |-----------------------|           |-----------------|             |-----------------------|

* Physically connect host `pinter` and `jarry` with switch via fibers.
* Manually add arp entry for host `pinter` and `jarry` so that they know each other's MAC address.

## ICMP ping test
SSH to switch, run
```
sde # enter the directory of barefoot SDE
./p4_build.sh pkgsrc/p4-examples/programs/basic_switching/basic_switching.p4  # build basic_switch program
./run_switchd.sh -p basic_switching # start basic_switching program

### sde cmd from now on ####
bfshell> ucli
bf-sde> pm                          # enter port manager module
bf-sde.pm> port-add 30/- 10G NONE   # split into 4 10Gbps port to fit in host NIC rate
bf-sde.pm> port-add 31/- 10G NONE
bf-sde.pm> an-set 30/- 2            # turn off auto-negotiate mechanism of the switch port
bf-sde.pm> an-set 31/- 2
bf-sde.pm> port-enb 30/-            # enable switch port
bf-sde.pm> port-enb 31/-
bf-sde.pm> show
-----+----+---+----+------+----+---+---+---+----------------+----------------+-
PORT |MAC |D_P|P/PT|SPEED |FEC |RDY|ADM|OPR|FRAMES RX       |FRAMES TX       |E
-----+----+---+----+------+----+---+---+---+----------------+----------------+-
30/0 |27/0|156|3/28| 10G  |NONE|YES|ENB|UP |               5|               0|  
31/0 |24/0|132|3/ 4| 10G  |NONE|YES|ENB|UP |               4|               0|
```

Note that there should be 2 ports with `RDY=YES`, `ADM=ENB` and `OPR=UP` like the above.

You can refer to the section `Start up the switching functions` in `barefoot-switch-instruction.pdf`.

Then add entries to the table, run
```
bf-sde.pm> ..
bf-sde> exit
bfshell> pd-basic-switching device 0
pd-basic-switching:0> pd init
pd-basic-switching:0> pd forward add_entry set_egr ethernet_dstAddr 0x90e2ba5e72c9 action_egress_spec 156
pd-basic-switching:0> pd forward add_entry set_egr ethernet_dstAddr 0x001b2173e84d action_egress_spec 132
```

Tip: Always remember to type "?" or tab for auto completion.

SSH to `pinter`, run
```
ping 192.168.1.11
```
