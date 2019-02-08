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
                              |   Open Network Linux OS      |
                        |------------------------------------------|
                        | white-box hardware (with ONIE installed) |
                        |------------------------------------------|

## How to Access
```
ssh user_name@aslan.wings.cs.wisc.edu # enter the lab machine with public address
ssh root@10.0.1.232 # password is onl
```

## To do
* The switch's IP is obtained via DHCP. Use hostname to access the switch.
* Change the default password for safety consideration.
* Currently only 1 out of 2 switches works. The other switch installed with Ubuntu 16.4.4 has driver issues and can not start P4 functions. Will contact Stordis technique support soon.

## P4 function cases
* See `basic_switching.md`
