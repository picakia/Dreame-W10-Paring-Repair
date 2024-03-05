# Dock communication

Each command can be executed only after `<SA_>`

```
Home HOLD, start cleaning 5x - power off display, UART works - (Beep - beep beep beep beep)
start cleaning HOLD, Home 2x - Sends <SA_><Ccc> to UART (Beep)
```

### Commands with S
```
<SA_> (Resp: <ZA☺>) - Gives access to running commands like listed below
<SB_> (Resp: <ZB☺>) - Turns off all running elements (like pumps, fan)
<SC_> and next letters (Resp: nothing) - Reboots the MCU
```

### Commands with C
```
<CA_> (Resp: <RA☺>) - Turns on the water pump
<CB_> (Resp: <RB>) - Does nothing
<CC_> (Resp: <RC☺>) - Turns on the Vaccum pump
<CD_> (Resp: <RD>) - Does nothing
<CE_> (Resp: <RE☺>) - Turns on the Fan
<CF_> (Resp: <RF⸮>) - Does nothing
<CG_> (Resp: <RG☺>) - Turns on Fan Heater
<CH_> (Resp: <RH>) - Does nothing
<CI_> (Resp: <RI☺>) - Does nothing
<CJ_> (Resp: <RJ>) - Does nothing
<CK_> (Resp: <RK☺>) - Does nothing
<CL_> (Resp: <RL'>) - Does nothing
<CM_> (Resp: <RM☺>) - Turns the screen Blue
<CN_> (Resp: <RN♥♦>) - Does nothing
<CO_> (Resp: <RO>) - Does nothing
<CP_> (Resp: <RP♠>) - Does nothing
<CQ_> (Resp: <RQ\x08>) - Does nothing
<CR_> (Resp: <RR☺>) - Does Beep - Beep
<CS_> (Resp: <RS♦>) - Does nothing
<CT_> (Resp: nothing) - Does nothing
<CU_> (Resp: <RU$↑��E>) - Does nothing
<CV_> (Resp: nothing) - Does nothing
<CW_> (Resp: <RW☺>) - Does nothing
<CX_> (Resp: nothing) - Does nothing
<CY_> (Resp: nothing) - Does nothing
<CZ_> (Resp: nothing) - Does nothing
```



