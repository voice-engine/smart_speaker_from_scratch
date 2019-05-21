GPIO
====

## IO Used By VOICEN Kit

| name       | number  | usage        |
|------------|---------|--------------|
| PA2        | 2       | AMP_PWR_EN   |
| PA3        | 3       | AMP_MUTE     |
| PC0        | 64      | BLUE LED     |
| PC1        | 65      | GREEN LED    |
| PC2        | 66      | YELLOW LED   |
| PC3        | 67      | RED LED      |
| PG11       | 203     | TOUCH BUTTON |

from name to number: 

`n(PC3) = ('C' - 'A') * 32 + 3 = 2 * 32 + 3 = 67`

## 3 ways to access IO
1. sysfs


2. gpiod

   ```
   sudo apt install -y gpiod
   sudo gpioset 0 65=0  # turn on green LED
   sudo gpiomon 0 203   # detect touch event 
   ```

3. direct memory (IO registers)

   ```
   sudo apt install -y sunxi-tools
   sudo sunxi-pio -m PC1=1  # turn on green LED
   ```

   ## Reference
   1. https://github.com/brgl/libgpiod
   2. https://github.com/linux-sunxi/sunxi-tools
   