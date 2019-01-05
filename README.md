## Interrupt Generator

This is a simple interrupt generator module used for OpenRISC interrupt testing
in the [or1k-tests](../or1k-tests) project.

See example usage in [mor1kx-generic](../mor1kx-generic).

The `intgen` module provides 2 8-bit write only registers on a wishbone bus.
The registers are an 8-bit counter and an interrupt clear register.  The counter
register at address `0x0` will decrement on every clock tick whenever the value
is not zero.  The interrupt line `irq_o` will go high when the counter reaches
`1`.  The interrupt line is cleared when there are any writes to the interrupt
clear register at address `0x1`.


### Diagram

```
            |---------------------------------------------|
            |                   INTGEN                    |
   -------->| clk_i                                       |
   -------->| rst_i                                       |
   ========>| wb_* ==\\                                   |
            |         V                                   |
            | |----REGMAP-------|                         |
            | 0x0 [ counter   ]<----(decrement if != 0)   |
            | 0x1 [ irq clear ] |                         |
            | |-----------------|                         |
            |                                             |
            |                                             |
            |           (set when counter == 1) -\        |
            |           (clear on write to irq) --- irq_o |------>
            |                                             |
            |                                             |
            |---------------------------------------------|
```
