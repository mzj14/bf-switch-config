# bf-switch-config
Documentation for Barefoot Switch Configuration

## Introduction

2 Stordis BF6064X white-box switches with Barefoot Tofino Chips in Room \#3370.

Software stack is as follows:

                                      |--------------|
                                      | Barefoot SDE |
                                |-------------------------|
                                |   Board Support Package |
                              |------------------------------|
                          | Open Network Linux OS / Ubuntu 16.4.4 |
                        |------------------------------------------|
                        | white-box hardware (with ONIE installed) |
                        |------------------------------------------|

## How to Access
Access and use the switch with Open Network Linux installed as follows:

```
ssh user_name@aslan.wings.cs.wisc.edu # enter the lab machine with public address
ssh root@10.0.1.232 # password is onl
cd /root/bf-sde-8.7.0
```

Access and use the switch with Ubuntu installed as follows:

```
ssh user_name@aslan.wings.cs.wisc.edu # enter the lab machine with public address
ssh p4@10.0.1.241 # password is hello2tofino
cd /data/bf-sde-8.7.0
```

## P4 function cases
* See `basic_switching.md`
