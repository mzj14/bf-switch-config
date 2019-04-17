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
Please access and use the switch with Open Network Linux installed as follows (The one with Ubuntu is under configuration for advanced purposes).

```
ssh user_name@aslan.wings.cs.wisc.edu # enter the lab machine with public address
ssh root@10.0.1.232 # password is onl
```

## To do
* The switch's IP is obtained via DHCP. Use hostname to access the switch.
* Change the default password for safety consideration.

## P4 function cases
* See `basic_switching.md`
