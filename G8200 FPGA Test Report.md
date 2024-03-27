# G8200 FPGA Test Report

[TOC]

<div style="page-break-after: always"></div>

## Spec

- Address = 0X48

## Test Condition

- Clock = 400KHz

<div style="page-break-after: always"></div>

## Test Item

### I2C  Test Pattern

#### Device Address Check

##### Read/Write Test

- Action :Read register 0  from device Address 0~127. 
- Check: If there is any response from the address other than 0X48
- Result: Pass

​	 [Test Log](test_device_address.txt)

#### Write/Read Data to Register Check

##### 確認暫存器寫入功能

- Action: 
  - 對 Register 0x00~0x0xFF 開始執行單筆寫入 1byte 到 256byte
  - 寫入1byte是以一循環是以 bit 位移七次，也就是 byte 值為 1->2->4->8->…->127 為寫入循環
  
- Check: 檢查是否有寫回ACK

- Result: 

  - 寫入`0XF0`後,再寫入其他register沒有發出ACK

    ```
    I2C Write Address= 0X48, reg 0XF0
    Data = 0X01 ACK
    Data = 0X02 ACK
    Data = 0X04 ACK
    Data = 0X08 ACK
    Data = 0X10 ACK
    Data = 0X20 ACK
    Data = 0X40 ACK
    Data = 0X80 ACK
    I2C Write Address= 0X48, reg 0XF1
    Data = 0X01 Failed
    Data = 0X02 Failed
    Data = 0X04 Failed
    Data = 0X08 Failed
    Data = 0X10 Failed
    Data = 0X20 Failed
    Data = 0X40 Failed
    Data = 0X80 Failed
    ```
    

  Write REG 0XF0 = 0X01
	<img src="assets\write_reg0XF0.png" alt="write_reg0xF0" style="zoom:50%;" />

  Write REG 0XF1 = 0X01. No ACK.
	<img src="assets\write_reg0XF1_after_0XF0.png" style="zoom:50%;" />

- Note: 

  - 重複寫G8200會發生錯誤 => add  MCU I2C write timeout