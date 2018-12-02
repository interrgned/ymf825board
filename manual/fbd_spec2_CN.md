## 系统设置
### CLKE

|I_ADR|W/R|D7|D6|D5|D4|D3|D2|D1|D0|Reset Value|
|-|-|-|-|-|-|-|-|-|-|-|
|#0|W/R|"0"|"0"|"0"|"0"|"0"|"0"|"0"|CLKE|00H|

#### 描述
CLKE是用于控制内部主时钟门控（ENABLE / DISABLE）的寄存器位。
要激活时钟，请将ALKE设置为“1”，将CLKE设置为“1”。
有关控制时钟的详细信息，请参见“初始化步骤”。
+ "0": 时钟禁用 (复位初值)
+ "1": 时钟使能

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。


### ALRST

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#1|W/R|ALRST|"0"|"0"|"0"|"0"|"0"|"0"|"0"|80H|

#### 描述
ALRST是用于复位所有接口寄存器的寄存器。  
+ "0": 退出重置状态。
+ "1": 重置接口寄存器。 （重置值）

以下接口寄存器被重置：  
+ I_ADR#3，#5到#28，从#32到#79

包含控制寄存器的以下接口寄存器不会通过该寄存器位复位：
+ I_ADR#0: CLKE
+ I_ADR#1: ALRST (this register bit)
+ I_ADR#2: AP0-3
+ I_ADR#29: DRV_SEL
+ I_ADR#80: COMM


#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。


### AP0, AP1, AP2, AP3

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#2|W/R|"0"|"0"|"0"|"0"|AP3|AP2|AP1|AP0|0FH|

#### 描述
AP0-3是模拟模块中的掉电控制寄存器位。
将寄存器位设置为“1”（复位值）时，其将进入断电状态，从而降低功耗。 每个寄存器位与其可控制之间的对应关系

模块如下：
+ AP0: VREF, IREF
+ AP1: SPAMP, SPOUT1
+ AP2: SPAMP, SPOUT2
+ AP3: DAC

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。


### GAIN

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#3|W/R|"0"|"0"|"0"|"0"|"0"|"0"|GAIN1|GAIN0|01H|


#### 描述

GAIN是用于选择扬声器放大器增益的寄存器，如下所示：  

|GAIN1|GAIN0|SPAmplifier|Gain|
|-|-|-|-|
|"0"|"0"|SPOUT|gain =5.0dB|
|"0"|"1"|SPOUT|gain =6.5dB (复位初值)|
|"1"|"0"|SPOUT|gain =7.0dB|
|"1"|"1"|SPOUT|gain =7.5dB|

注意) 假设空载条件下面的增益值。 
负载为8欧姆，增益降低约0.2 dB（典型值）。

#### 重置条件

1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。
3. 当ALRST设置为“1”时。


### HW_ID

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#4|R|"0"|"0"|"0"|"0"|"0"|"0"|"0"|"1"|01H|

#### 描述
HW_ID是用于保存硬件版本的只读寄存器。  
值为“01H”（固定）。  
即使ALRST位设置为“1”，也可以读取该值。


### CONTENTS_DATA_REG

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#7|W|DT7|DT6|DT5|DT4|DT3|DT2|DT1|DT0|-|

#### 描述
该寄存器用于写入内容数据。

### 音序器设置

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#8|W/R|AllKeyOff|AllMute|AllEGRst|R_FIFOR|REP_SQ|R_SEQ| R_FIFO|START|00H|

### AllKeyOff
#### 描述
AllKeyOff是用于将所有声音的KeyOn寄存器设置为“0”的寄存器位。

+ "0": 没有处理 (复位初值)
+ "1": 将KeyOn寄存器设置为“0”。

将寄存器位设置为“1”后，等待超过6us后，然后将其返回到“0”。

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。
3. 当ALRST设置为“1”时。

### AllMute
#### 描述
AllMute是用于将所有声音的静音寄存器设置为“1”的寄存器位。
+ "0": 没有处理 (复位初值)
+ "1": 将静音寄存器设置为“1”。

将寄存器位设置为“1”后，等待超过6us后，然后将其返回到“0”。

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。
3. 当ALRST设置为“1”时。

### AllEGRst
#### 描述
AllEGRst用于将所有声音的EG_RST寄存器设置为“1”。
+ "0": 没有处理 (复位初值)
+ "1": 将EG_RST寄存器设置为“1”。

将寄存器位设置为“1”后，等待超过6us，然后将其返回到“0”。

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。
3. 当ALRST设置为“1”时。


### SEQ_Vol

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#9|W/R|SEQ_Vol4|SEQ_Vol3|SEQ_Vol2|SEQ_Vol1|SEQ_Vol0|DIR_SV|"0"|SIZE8|00H|
|#10|W/R|SIZE7|SIZE6|SIZE5|SIZE4|SIZE3|SIZE2|SIZE1|SIZE0|00H|


#### 描述
SEQ_Vol是音序器音量的音量设置寄存器。

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。
3. 当ALRST设置为“1”时。

|Value(HEX)|Volume[dB]|
|-|-|
|00H|mute|
|01H|-47.9|
|02H|-42.6|
|03H|-37.2|
|04H|-33.1|
|05H|-29.8|
|06H|-27.0|
|07H|-24.6|
|08H|-22.4|
|09H|-20.6|
|0AH|-18.9|
|0BH|-17.3|
|0CH|-15.9|
|0DH|-14.6|
|0EH|-13.4|
|0FH|-12.2|
|10H|-11.1|
|11H|-10.1|
|12H|-9.2|
|13H|-8.3|
|14H|-7.4|
|15H|-6.6|
|16H|-5.8|
|17H|-5.1|
|18H|-4.4|
|19H|-3.6|
|1AH|-3.0|
|1BH|-2.3|
|1CH|-1.7|
|1DH|-1.1|
|1EH|-0.6|
|1FH|0.0|

### DIR_SV

#### 描述

DIR_SV寄存器位控制是否将插值应用于SEQ_Vol和ChVol0-15。
当寄存器位设置为“1”时，无论DIR_CV0-15和CHVOL_ITIME的设置如何，插值都不会应用于SEQ_Vol和ChVol0-15。
当它设置为“0”（复位值）时，插值取决于DIR_CV0-15和CHVOL_ITIME设置。

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。
3. 当ALRST设置为“1”时。

### SIZE

#### 描述
SIZE是用于设置音序器数据大小的寄存器，以字节为单位。


## 合成器设置

### CRGD_VNO

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#11|W/R|"0"|"0"|"0"|"0"|CRGD_VNO3|CRGD_VNO2|CRGD_VNO1|CRGD_VNO0|00H|

#### 描述
CRGD_VNO用于指定音调编号。

#### 重置条件
1. 电源打开时（电源复位）。
2. 应用硬件复位时（RST_N =“L”）。
3. 当ALRST设置为“1”时。


## 控制寄存器写入寄存器

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 |
|-|-|-|-|-|-|-|-|-|-|
|#12|W|"0"|VoVol4|VoVol3|VoVol2|VoVol1|VoVol0|"0"|"0"|
|#13|W|"0"|"0"|FNUM9|FNUM8|FNUM7|BLOCK2|BLOCK1|BLOCK0|
|#14|W|"0"|FNUM6|FNUM5|FNUM4|FNUM3|FNUM2|FNUM1|FNUM0|
|#15|W|"0"|KeyOn|Mute|EG_RST|ToneNum3|ToneNum2|ToneNum1|ToneNum0|
|#16|W|"0"|ChVol4|ChVol3|ChVol2|ChVol1|ChVol0|"0"|DIR_CV|
|#17|W|"0"|"0"|"0"|"0"|"0"|XVB2|XVB1|XVB0|
|#18|W|"0"|"0"|"0"|INT1|INT0|FRAC8|FRAC7|FRAC6|
|#19|W|"0"|FRAC5|FRAC4|FRAC3|FRAC2|FRAC1|FRAC0|"0"|
|#20|W|"0"|"0"|"0"|"0"|"0"|"0"|"0"|DIR_MT|

### VoVol

#### Description
The VoVol is the volume setting registers for each voice number.  
The relationship between setting values and volume gain values is the same as that of ChVol and SEQ_Vol.
The interpolation function is not provided for these volume setting registers.

#### Reset Value
+ 00H (Mute)

### FNUM, BLOCK

#### Description
+ BLOCK: Specifies an octave.
+ FNUM: Sets the frequency information for one octave.

They are set for each voice.

#### Reset Value
+ FNUM : 000H
+ BLOCK: 00H

#### Pitch Table

|Note|Frequency|BLOCK|FNUM|
|-|-|-|-|
|C2		|130.8	|3	|357|
|C#2	|138.6	|3	|378|
|D2		|146.8	|3	|401|
|D#2	|155.6	|3	|425|
|E2		|164.8	|3	|450|
|F2		|174.6	|3	|477|
|F#2	|185	|3	|505|
|G2		|196	|3	|535|
|G#2	|207.7	|3	|567|
|A2		|220	|3	|601|
|A#2	|233.1	|3	|637|
|B2		|246.9	|3	|674|
|C3		|261.6	|4	|357|
|C#3	|277.2	|4	|378|
|D3		|293.7	|4	|401|
|D#3	|311.1	|4	|425|
|E3		|329.6	|4	|450|
|F3		|349.2	|4	|477|
|F#3	|370	|4	|505|
|G3		|392	|4	|535|
|G#3	|415.3	|4	|567|
|A3		|440	|4	|601|
|A#3	|466.2	|4	|637|
|B3		|493.9	|4	|674|
|C4		|523.3	|5	|357|

### ToneNum

#### Description
The ToneNum is used to select a tone parameter to use.  
This register is provided for each voice.  

#### Reset Value
+ 00H

### KeyOn

#### Description
The KeyOn is used to control the sound generation.  
+ "0": KeyOff (reset value)
+ "1": KeyOn

This register is provided for each voice.

### Mute

#### Description
The Mute is the mute control register.  
This register is provided for each voice.   
+ "0: Cancels the mute (reset value)
+ "1": Shifts to the mute state.

The volume of a voice with the Mute set to "1" shifts to a mute state according to the DIR_MT(I_ADR#20) and the MUTE_ITIME(I_ADR#27) settings; however, when the mute is cancelled, the interpolation is not performed regardless of these settings.

### EG_RST

#### Description
A voice with the EG_RST set to "1" shifts to a mute state immediately regardless of the DIR_MT and MUTE_ITIME settings.  
This register is provided for each voice.   
Reset Value is "0".

### ChVol

#### Description
This volume setting register is provided for each voice.  
The interpolation function is provided for this volume setting register.  
The relationship between setting values and volume gain values is the same as that of VoVol and SEQ_Vol.
Reset Value is "18H" (-4.4 dB)

### DIR_CV

#### Description
The DIR_CV controls the interpolation of the SEQ_Vol and ChVol.  
This register is provided for each voice.   

DIR_CV="1":  
No interpolation in the SEQ_Vol and the ChVol# regardless of the DIR_SV and CHVOL_ITIME settings.

DIR_CV# = "0" (reset value):  
The interpolation depends on the DIR_SV and CHVOL_ITIME settings.

### XVB

#### Description
The XVB is used to set a vibrato modulation.  
This register is provided for each voice.   
A setting value relatively acts on a DVB setting value of the voice parameter, as shown below.  
When the calculation (add) result exceeds "3", "3"is used for the processing.

+ "0": OFF (reset value)
+ "1": 1 x (DVB value is used as is.)
+ "2": 2 x (DVB += 1)
+ "3": 2 x (DVB += 1)
+ "4": 4 x (DVB += 2)
+ "5": 4 x (DVB += 2)
+ "6": 8 x (DVB += 3)
+ "7": 8 x (DVB += 3)

### INT, FRAC

#### Description
These registers specify a multiplier to the generated audio frequency. This number and frequency are proportional.  
The INT is an integer part and FRAC is a fraction part.  
These registers are provided for each voice.  

#### Reset Value
+ INT : "01H"
+ FRAC: "000H"


### DIR_MT

The DIR_MT is used to control the interpolation in a mute state. This register bit works for all the 16 voices.
+ "0": Enables the interpolation. (reset value)
+ "1": Disables the interpolation.

When this register bit is set to "0", the MUTE_ITIME (I_ADR#27) setting becomes valid; however, the interpolation is not performed when the mute is cancelled regardless of this register and MUTE_ITIME setting.


## Volume Settings

### MASTER_VOL

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#25|W/R|MASTER_VOL5|MASTER_VOL4|MASTER_VOL3|MASTER_VOL2|MASTER_VOL1|MASTER_VOL0|"0"|"0"|00H|

#### Description
The MASTER_VOL is used to control the master volume level.  
The interpolation function is available.  

|DEC|HEX|Volume Level[dB]|
|-|-|-|
|0  |00H |muted  |
|1  |01H |-50  |
|2  |02H |-49  |
|3  |03H |-48  |
|4  |04H |-47  |
|5  |05H |-46  |
|6  |06H |-45  |
|7  |07H |-44  |
|8  |08H |-43  |
|9  |09H |-42  |
|10 |0AH |-41  |
|11 |0BH |-40  |
|12 |0CH |-39  |
|13 |0DH |-38  |
|14 |0EH |-37  |
|15 |0FH |-36  |
|16 |10H |-35  |
|17 |11H |-34  |
|18 |12H |-33  |
|19 |13H |-32  |
|20 |14H |-31  |
|21 |15H |-30  |
|22 |16H |-29  |
|23 |17H |-28  |
|24 |18H |-27  |
|25 |19H |-26  |
|26 |1AH |-25  |
|27 |1BH |-24  |
|28 |1CH |-23  |
|29 |1DH |-22  |
|30 |1EH |-21  |
|31 |1FH |-20  |
|32 |20H |-19  |
|33 |21H |-18|
|34 |22H |-17|
|35 |23H |-16|
|36 |24H |-15|
|37 |25H |-14|
|38 |26H |-13|
|39 |27H |-12|
|40 |28H |-11|
|41 |29H |-10|
|42 |2AH |-9|
|43 |2BH |-8|
|44 |2CH |-7|
|45 |2DH |-6|
|46 |2EH |-5|
|47 |2FH |-4|
|48 |30H |-3|
|49 |31H |-2|
|40 |32H |-1|
|51 |33H |0|
|52 |34H |+1|
|53 |35H |+2|
|54 |36H |+3|
|55 |37H |+4|
|56 |38H |+5|
|57 |39H |+6|
|58 |3AH |+7|
|59 |3BH |+8|
|60 |3CH |+9|
|61 |3DH |+10|
|62 |3EH |+11|
|63 |3FH |+12|


### MUTE_ITIME

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#27|W/R|"0"|DADJT|MUTE_ITIME1|MUTE_ITIME0|CHVOL_ITIME1|CHVOL_ITIME0|MVOL_ITIME1|MVOL_ITIME0|00H|

#### Description
The MUTE_ITIME is used to specify the volume level variation under the muted condition when the DIR_MT is "0".
+ "00b": No interpolation (reset value)
+ "01b": Setting prohibited
+ "10b": Enables the interpolation. (in 0.3750dB steps, 128/fs (0dB <-> Mute (approx. 2.7 ms))
+ "11b": Enables the interpolation. (in 0.1875dB steps, 256/fs (0dB <-> Mute (approx. 5.3 ms))

When the DIR_MT is "1", no interpolation is selected regardless of this register setting.

### CHVOL_ITIME

#### Description
The CHVOL_ITIME is used to specify the volume level variation time of the SEQ_Vol and the ChVol0-15.  
This variation time becomes valid only for voices with the DIR_SV set to "0"and the DIR_CV0-15 set to "1".
+ "00b": No interpolation (reset value)
+ "01b": Setting prohibited
+ "10b": Enables the interpolation. (approx. 0.2dB steps, 256/fs (0dB <-> Mute: approx. 5.3ms))
+ "11b": Enables the interpolation. (approx. 0.05 dB steps, 1024/fs (0dB <-> Mute: approx. 21.3ms))

### MVOL_ITIME

#### Description
The MVOL_ITIME is used to specify the master volume level variation time.
+ "00b": No interpolation (The setting value in the MASTER_VOL (I_ADR#25) is immediately reflected.) (reset value)
+ "01b": Enables the interpolation. (approx. 0.2 dB steps, 512/fs (+12dB <-> Mute: approx. 10.6ms))
+ "10b": Enables the interpolation. (approx. 0.1 dB steps, 1024/fs (+12dB <-> Mute: approx. 21.3 ms))
+ "11b": Enables the interpolation. (approx. 0.05 dB steps, 2048/fs (+12dB <-> Mute: approx. 42.6 ms))

### LFO_RST

| I_ADR | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | Reset Value |
|-|-|-|-|-|-|-|-|-|-|-|
|#28|W/R|"0"|"0"|"0"|"0"|"0"|"0"|"0"|LFO_RST|00H|

#### Description
The LFO_RST is used to reset the phase of the LFO (Low Frequency Oscillator).
+ "0": No processing (reset value)
* "1": Reset

Write "0" into the register bit after writing "1" to the bit.

### W_CEQ0/1/2

| I_ADR | W/R | D7-D0 | Reset Value |
|-|-|-|-|
|#32|W|W_CEQ0|-|
|#33|W|W_CEQ1|-|
|#34|W|W_CEQ2|-|

#### Description
The W_CEQ registers are used to set equalizer coefficients.  
There are 3 bytes x 5 coefficients (CEQ#0[23:0] to CEQ#4[23:0]) for each band and they correspond to the coefficient setting registers as follows:
+ W_CEQ0: Band 0 coefficients (CEQ00[23:0] to CEQ04[23:0])
+ W_CEQ1: Band 1 coefficients (CEQ10[23:0] to CEQ14[23:0])
+ W_CEQ2: Band 2 coefficients (CEQ20[23:0] to CEQ24[23:0])

The setting is independent one another. Write the coefficients to the registers in this order, as
shown below:

| No. | W_CEQ0 | W_CEQ1 | W_CEQ2 |
|-|-|-|-|
|1|CEQ00[23:16]|CEQ10[23:16]|CEQ20[23:16]|
|2|CEQ00[15:8]|CEQ10[15:8]|CEQ20[15:8]|
|3|CEQ00[7:0]|CEQ10[7:0]|CEQ20[7:0]|
|4|CEQ01[23:16]|CEQ11[23:16]|CEQ21[23:16]|
|5|CEQ01[15:8]|CEQ11[15:8]|CEQ21[15:8]|
|6|CEQ01[7:0]|CEQ11[7:0]|CEQ21[7:0]|
|7|CEQ02[23:16]|CEQ12[23:16]|CEQ22[23:16]|
|8|CEQ02[15:8]|CEQ12[15:8]|CEQ22[15:8]|
|9|CEQ02[7:0]|CEQ12[7:0]|CEQ22[7:0]|
|10|CEQ03[23:16]|CEQ13[23:16]|CEQ23[23:16]|
|11|CEQ03[15:8]|CEQ13[15:8]|CEQ23[15:8]|
|12|CEQ03[7:0]|CEQ13[7:0]|CEQ23[7:0]|
|13|CEQ04[23:16]|CEQ14[23:16]|CEQ24[23:16]|
|14|CEQ04[15:8]|CEQ14[15:8]|CEQ24[15:8]|
|15|CEQ04[7:0]|CEQ14[7:0]|CEQ24[7:0]|

The 15-byte coefficient values are reflected at the moment when the 15th coefficient (CEQ#4[7:0]) is written.  
Write 15 bytes of coefficients in burst mode for each band.  
If the write operation is stopped in the middle (less than 15 bytes), values are not reflected.  
And on the contrary, if the write operation continues even after the 15th bytes, the 15-byte  
coefficients are overwritten when all the next 15 coefficients are written.  

The coefficient data format is as follows:  
Sign bit: 1 bit (CEQ##[23]), integer part: 3 bits (CEQ##[22:20]) and fraction part: 20 bits
(CEQ##[19:0])
in 2'complement.  
The figure bellows shows the relationship between the coefficients and the circuit configuration.

![EQ](eq.png)
