---
title: DTV2 Kernal disassembly
permalink: DTV2_Kernal_disassembly
---

The DTV V2 (PAL) ROM disected
=============================

Originally written by Daniel Kahlin, see [his
page](http://www.kahlin.net/daniel/dtv/flash/dtv2_kernal.txt) or
[Dtv2\_kernal\_disassembly\_0\_3.txt](Media:Dtv2_kernal_disassembly_0_3.txt "wikilink").

Note the [Hummer](Hummer "wikilink") uses a slightly different kernal
and flash file format, see [Hummer Kernal
disassembly](Hummer_Kernal_disassembly "wikilink").

Features of the DTV2/3 Kernal
-----------------------------

-   LOAD from flash using device 1, loading files from flash that extend
    into upper memory is supported ($F73C)
-   Video standard (PAL/NTSC) setting based on user port wiring ($F951)
-   If Joy2 button pressed on reset: Map RAM $018000 to $008000 and jump
    there if DTV80 signature present ($F8F6)
-   On reset unless CTRL pressed: Auto-load “INTRO” from flash, start at
    $080D ($F9C4)

Detailed listing of ROM differences
-----------------------------------

The DTV V2 ROM seems to be a patch upon the C64 Kernal version 3
(901227-03).

`;Kernal CINT (Initialize Screen and Keyboard)`  
`E518 20 36 F7   JSR $F736  ;was: JSR $E5A0`  
  
`----------------------------------------------------------------------------`  
  
`E59A 20 36 F7   JSR $F736  ;was: JSR $E5A0`  
  
`----------------------------------------------------------------------------`  
`F72C 60         RTS        ;was: Get Next Tape File Header from Cassette`  
  
`F72D 4C F1 F8   JMP $F8F1  ;Jump #0: Reset Entry`  
`F730 4C 3C F7   JMP $F73C  ;Jump #1: Load`  
`F733 4C FD F9   JMP $F9FD  ;Jump #2: Save`  
`F736 4C 1D FA   JMP $FA1D  ;Jump #3: Reset Video Mode`  
`F739 4C 40 FA   JMP $FA40  ;Jump #4: Reset Palette`  
  
`----------------------------------------------------------------------------`  
`; Jump #1: Load`  
`F73C 85 93      STA $93        ;Set verify flag`  
`F73E A5 BA      LDA $BA        ;Get device number`  
`F740 C9 01      CMP #$01   ;=1`  
`F742 F0 03      BEQ $F747  ;yes, load from flash.`  
`F744 4C A7 F4   JMP $F4A7  ;no, go to normal load.`  
`F747 08         PHP`  
`F748 78         SEI`  
`F749 A9 00      LDA #$00`  
`F74B 85 93      STA $93        ;Clear Verify flag`  
`F74D A2 01      LDX #$01`  
`F74F 8E 3F D0   STX $D03F  ;Enable extended registers.`  
`F752 8D 07 D3   STA $D307  ;DMA source and destination step=1`  
`F755 8D 09 D3   STA $D309`  
`F758 8E 06 D3   STX $D306`  
`F75B 8E 08 D3   STX $D308  ;---`  
`F75E 20 4D F8   JSR $F84D  ;Swap $f7-$012f to $0110f7`  
`F761 A2 00      LDX #$00   ;Set Source Ptr = $010000`  
`F763 86 F8      STX $F8        ;and Destination Ptr = $000100`  
`F765 86 F9      STX $F9`  
`F767 86 FD      STX $FD`  
`F769 E8         INX`  
`F76A 86 FA      STX $FA`  
`F76C A9 00      LDA #$00`  
`F76E 85 FB      STA $FB`  
`F770 A9 01      LDA #$01`  
`F772 85 FC      STA $FC        ;---`  
`F774 20 AF F5   JSR $F5AF  ;Print `“`SEARCHING`` ``FOR`` `<name>”  
`F777 20 B3 F8   JSR $F8B3  ;Set DMA Source from $F8-$FA`  
`F77A 20 C3 F8   JSR $F8C3  ;Set DMA Destination from $FB-$FD`  
`F77D A9 20      LDA #$20   ;One dir entry ($20 bytes)`  
`F77F 20 79 F8   JSR $F879  ;DMA Bytes (Acc=num bytes)`  
`F782 AD 00 01   LDA $0100  ;First byte of entry=00?`  
`F785 D0 07      BNE $F78E  ;no, check filename.`  
`F787 20 4D F8   JSR $F84D  ;Swap $f7-$012f to $0110f7`  
`F78A 28         PLP`  
`F78B 4C 04 F7   JMP $F704  ;Exit with File Not Found`  
`F78E A5 B7      LDA $B7`  
`F790 D0 07      BNE $F799`  
`F792 20 4D F8   JSR $F84D  ;Swap $f7-$012f to $0110f7`  
`F795 28         PLP`  
`F796 4C 10 F7   JMP $F710  ;Exit with Missing Filename`  
`F799 C9 18      CMP #$18   ;Filename longer than 24 chars`  
`F79B 90 02      BCC $F79F  ;No, continue`  
`F79D B0 E8      BCS $F787  ;Yes, File Not Found`  
`F79F A0 00      LDY #$00   ;Match filename`  
`F7A1 B1 BB      LDA ($BB),Y`  
`F7A3 C9 2A      CMP #$2A`  
`F7A5 F0 0F      BEQ $F7B6`  
`F7A7 D9 00 01   CMP $0100,Y`  
`F7AA D0 35      BNE $F7E1  ;mismatch, get next entry`  
`F7AC C8         INY`  
`F7AD C4 B7      CPY $B7`  
`F7AF D0 F0      BNE $F7A1`  
`F7B1 B9 00 01   LDA $0100,Y`  
`F7B4 D0 2B      BNE $F7E1  ;--- differing lengths, get next entry`  
`F7B6 20 D2 F5   JSR $F5D2  ;Print `“`LOADING`”  
`F7B9 A2 05      LDX #$05   ;set source and dest from directory`  
`F7BB BD 18 01   LDA $0118,X    ;entry`  
`F7BE 95 F8      STA $F8,X`  
`F7C0 CA         DEX`  
`F7C1 10 F8      BPL $F7BB  ;---`  
`F7C3 A5 B9      LDA $B9        ;Secondary Address`  
`F7C5 D0 08      BNE $F7CF  ;not 0, start loading `  
`F7C7 A5 C3      LDA $C3        ;set user specified load address `  
`F7C9 85 FB      STA $FB`  
`F7CB A5 C4      LDA $C4`  
`F7CD 85 FC      STA $FC        ;---`  
`F7D0    ; Instruction parameter jumped to (was: Put Ptr to Tape Buffer in X/Y)`  
`F7CF 20 E9 F7   JSR $F7E9  ;Load File Body`  
`F7D2 20 4D F8   JSR $F84D  ;Swap $f7-$012f to $0110f7`  
`F7D5 A9 00      LDA #$00`  
`F7D7 8D 3F D0   STA $D03F  ;Disable extended registers`  
`F7DA 28         PLP`  
`F7DB A9 00      LDA #$00   ;Status ok.`  
`F7DD 85 90      STA $90`  
`F7DF 18         CLC`  
`F7E0 60         RTS`  
  
`F7E1 A9 20      LDA #$20`  
`F7E3 20 D5 F8   JSR $F8D5  ;Update Source Ptr`  
`F7E6 4C 77 F7   JMP $F777`  
`F7EA    ; Instruction parameter jumped to (was: Search Tape for a filename)`  
  
`;Load File Body`  
`F7E9 A9 FE      LDA #$FE   ;Dest=$00FE`  
`F7EB 20 A5 F8   JSR $F8A5  ;Set DMA Destination from Acc`  
`F7EE 20 B3 F8   JSR $F8B3  ;Set DMA Source from $F8-$FA`  
`F7F1 20 88 F8   JSR $F888  ;Get byte + 1 byte look ahead`  
`F7F4 A5 FE      LDA $FE        ;Control byte`  
`F7F6 F0 4C      BEQ $F844  ;=$00, finished...go exit.`  
`F7F8 30 12      BMI $F80C  ;=Neg, go special`  
`F7FA AA         TAX        ;Positive, do copy`  
`F7FB 20 C3 F8   JSR $F8C3  ;Set DMA Destination from $FB-$FD`  
`F7FE 20 B3 F8   JSR $F8B3  ;Set DMA Source from $F8-$FA`  
`F801 8A         TXA        ;length`  
`F802 20 93 F8   JSR $F893  ;Do DMA (Acc=num bytes)`  
`F805 8A         TXA        ;length`  
`F806 20 E3 F8   JSR $F8E3  ;Update Destination Ptr`  
`F809 4C E9 F7   JMP $F7E9  ;Loop and do next.`  
`F80C 49 80      EOR #$80   ;Clear MSB.`  
`F80E 85 F7      STA $F7        ;Preserve length`  
`F810 A9 01      LDA #$01   ;Acc=1`  
`F812 20 D5 F8   JSR $F8D5  ;Update Source Ptr`  
`F815 A6 FD      LDX $FD        ;X=Dest(High) `  
`F817 A4 FC      LDY $FC        ;Y=Dest(Middle)`  
`F819 D0 01      BNE $F81C  ;TmpDest-$0100`  
`F81B CA         DEX`  
`F81C 88         DEY        ;---`  
`F81D A5 FB      LDA $FB        ;TmpDest+second-byte.`  
`F81F 18         CLC`  
`F820 65 FF      ADC $FF`  
`F822 90 04      BCC $F828`  
`F824 C8         INY`  
`F825 D0 01      BNE $F828`  
`F827 E8         INX        ;---`  
`F828 8D 00 D3   STA $D300  ;Set DMA Source=TmpDest`  
`F82B 8C 01 D3   STY $D301`  
`F82E 8A         TXA`  
`F82F 09 40      ORA #$40   ;RAM`  
`F831 8D 02 D3   STA $D302  ;---`  
`F834 20 C3 F8   JSR $F8C3  ;Set DMA Destination from $FB-$FD`  
`F837 A5 F7      LDA $F7        ;length`  
`F839 20 79 F8   JSR $F879  ;DMA Bytes (Acc=num bytes)`  
`F83C A5 F7      LDA $F7        ;length`  
`F83E 20 E3 F8   JSR $F8E3  ;Update Destination Ptr`  
`F841 4C E9 F7   JMP $F7E9  ;Loop and do next.`  
`F844 A6 FB      LDX $FB        ;Update load end addr, and exit`  
`F846 86 AE      STX $AE`  
`F848 A4 FC      LDY $FC`  
`F84A 84 AF      STY $AF        ;---`  
`F84C 60         RTS`  
  
`;Swap $f7-$012f to $0110f7 (always leaves with Y=$41)`  
`F84D A9 00      LDA #$00`  
`F84F A0 40      LDY #$40`  
`F851 8C 05 D3   STY $D305  ;Destination (High)=$00, RAM`  
`F854 C8         INY`  
`F855 8C 02 D3   STY $D302  ;Source (High)=$01, RAM`  
`F858 8D 0B D3   STA $D30B  ;DMA Length (High)=0`  
`F85B 8D 04 D3   STA $D304  ;Destination (Middle)=$00`  
`F85E A9 10      LDA #$10`  
`F860 8D 01 D3   STA $D301  ;Source (Middle)=$10`  
`F863 A9 F7      LDA #$F7`  
`F865 8D 00 D3   STA $D300  ;Source (Low)=$F7`  
`F868 8D 03 D3   STA $D303  ;Destination (Low)=$F7`  
`F86B A9 39      LDA #$39`  
`F86D 8D 0A D3   STA $D30A  ;DMA Length (Low)=$39`  
`F870 A9 0F      LDA #$0F   ;Source Dir=pos, Dest Dir=pos,`  
`F872 8D 1F D3   STA $D31F  ;Swap source with dest=1, Force Start=1`  
`F875 4C 81 F8   JMP $F881  ;Check for DMA finished`  
`F878 60         RTS`  
  
`;DMA Bytes (Acc=num bytes)`  
`F879 20 9C F8   JSR $F89C  ;Set DMA Length`  
`F87C A9 0D      LDA #$0D   ;Source Dir=pos, Dest Dir=pos`  
`F87E 8D 1F D3   STA $D31F  ;Force Start=1`  
`F881 AD 1F D3   LDA $D31F  ;Check for DMA finished`  
`F884 4A         LSR A`  
`F885 B0 FA      BCS $F881`  
`F887 60         RTS`  
  
`;Get byte + 1 byte look ahead`  
`F888 A9 02      LDA #$02`  
`F88A 20 79 F8   JSR $F879`  
`F88D A9 01      LDA #$01`  
`F88F 20 D5 F8   JSR $F8D5  ;Update Source Ptr`  
`F892 60         RTS`  
  
`;Do DMA (Acc=num bytes)`  
`F893 48         PHA`  
`F894 20 79 F8   JSR $F879`  
`F897 68         PLA`  
`F898 20 D5 F8   JSR $F8D5  ;Update Source Ptr`  
`F89B 60         RTS`  
  
`;Set DMA Length (Acc=length)`  
`F89C 8D 0A D3   STA $D30A`  
`F89F A9 00      LDA #$00`  
`F8A1 8D 0B D3   STA $D30B`  
`F8A4 60         RTS`  
  
`; Set DMA Destination from Acc`  
`F8A5 8D 03 D3   STA $D303  ;Destination (Low)`  
`F8A8 A9 00      LDA #$00`  
`F8AA 8D 04 D3   STA $D304  ;Destination (Middle) = $00`  
`F8AD A9 40      LDA #$40`  
`F8AF 8D 05 D3   STA $D305  ;Destination (High) = $40 (RAM)`  
`F8B2 60         RTS`  
  
`;Set DMA Source from $F8-$FA`  
`F8B3 A5 F8      LDA $F8`  
`F8B5 8D 00 D3   STA $D300  ;Source (Low)`  
`F8B8 A5 F9      LDA $F9`  
`F8BA 8D 01 D3   STA $D301  ;Source (Middle)`  
`F8BD A5 FA      LDA $FA`  
`F8BF 8D 02 D3   STA $D302  ;Source (High)`  
`F8C2 60         RTS`  
  
`; Set DMA Destination from $FB-$FD`  
`F8C3 A5 FB      LDA $FB`  
`F8C5 8D 03 D3   STA $D303  ;Destination (Low)`  
`F8C8 A5 FC      LDA $FC`  
`F8CA 8D 04 D3   STA $D304  ;Destination (Middle)`  
`F8CD A5 FD      LDA $FD`  
`F8CF 09 40      ORA #$40   ;RAM`  
`F8D1 8D 05 D3   STA $D305  ;Destination (High)`  
`F8D4 60         RTS`  
  
`;Update Source Ptr`  
`F8D5 18         CLC`  
`F8D6 65 F8      ADC $F8`  
`F8D8 85 F8      STA $F8`  
`F8DA 90 06      BCC $F8E2`  
`F8DC E6 F9      INC $F9`  
`F8DE D0 02      BNE $F8E2`  
`F8E0 E6 FA      INC $FA`  
`F8E2 60         RTS`  
  
`;Update Destination Ptr`  
`F8E3 18         CLC`  
`F8E4 65 FB      ADC $FB`  
`F8E6 85 FB      STA $FB`  
`F8E8 90 06      BCC $F8F0`  
`F8EA E6 FC      INC $FC`  
`F8EC D0 02      BNE $F8F0`  
`F8EE E6 FD      INC $FD`  
`F8F0 60         RTS`  
  
`----------------------------------------------------------------------------`  
`; Jump #0: Reset Entry`  
`F8F1 A2 FF      LDX #$FF   ;Reset Hook (from Jump #0)`  
`F8F3 78         SEI        ;Kill interrupts`  
`F8F4 9A         TXS        ;Reset SP`  
`F8F5 D8         CLD        ;Clear decimal mode`  
`F8F6 A9 FF      LDA #$FF   ;Set all joy-port pins high`  
`F8F8 8D 00 DC   STA $DC00  ;to enable reading them.`  
`F8FB AD 00 DC   LDA $DC00`  
`F8FE 29 10      AND #$10   ;Check Left Button`  
`F900 D0 1C      BNE $F91E  ;No, skip resident check`  
`F902 32 EE      SAC #$EE`  
`F904 A9 06      LDA #$06   ;Map $018000 at $8000`  
`F906 32 00      SAC #$00`  
`F908 A2 04      LDX #$04   ;Check for DTV80 at $018004`  
`F90A BD 18 FA   LDA $FA18,X`  
`F90D DD 04 80   CMP $8004,X`  
`F910 D0 06      BNE $F918  ;No, restore mapping and skip`  
`F912 CA         DEX`  
`F913 10 F5      BPL $F90A`  
`F915 6C 00 80   JMP ($8000)    ;Jump indir to $8000 ($018000)`  
`F918 32 EE      SAC #$EE`  
`F91A A9 02      LDA #$02   ;Map $008000 at $8000`  
`F91C 32 00      SAC #$00`  
`F91E A2 00      LDX #$00   ;Initialize`  
`F920 8E 20 D0   STX $D020  ;Black border`  
`F923 8E 11 D0   STX $D011  ;Blank screen`  
`F926 20 A3 FD   JSR $FDA3`  
`F929 20 50 FD   JSR $FD50`  
`F92C 20 15 FD   JSR $FD15`  
`F92F 20 53 E4   JSR $E453`  
`F932 20 BF E3   JSR $E3BF`  
`F935 AD 11 D0   LDA $D011  ;Wait for vertical blanking`  
`F938 10 FB      BPL $F935`  
`F93A AD 11 D0   LDA $D011`  
`F93D 30 FB      BMI $F93A  ;---`  
`F93F 20 A0 E5   JSR $E5A0`  
`F942 A9 00      LDA #$00`  
`F944 8D 20 D0   STA $D020  ;Black border`  
`F947 8D 11 D0   STA $D011  ;Blank Screen`  
`F94A 20 1B E5   JSR $E51B`  
`F94D 20 5E FF   JSR $FF5E`  
`F950 58         CLI`  
`F951 A2 01      LDX #$01   ;Set video standard from User Port and $01 bits`  
`F953 8E 3F D0   STX $D03F       `  
`F956 A9 00      LDA #$00`  
`F958 8D 03 DD   STA $DD03`  
`F95B AD 01 DD   LDA $DD01`  
`F95E 85 AA      STA $AA`  
`F960 29 03      AND #$03`  
`F962 8D 40 D0   STA $D040`  
`F965 A5 AA      LDA $AA`  
`F967 4A         LSR A`  
`F968 4A         LSR A`  
`F969 29 0F      AND #$0F`  
`F96B 8D 4F D0   STA $D04F`  
`F96E 24 AA      BIT $AA`  
`F970 70 03      BVS $F975`  
`F972 A9 07      LDA #$07`  
`F974 2C        .BYTE $2C`  
`F975 A9 06 LDA #$06`  
`F977 8D 4E D0   STA $D04E`  
`F97A 24 AA      BIT $AA`  
`F97C 30 03      BMI $F981`  
`F97E A0 02      LDY #$02`  
`F980 2C            .BYTE $2C`  
`F981 A0 05      LDY #$05`  
`F983 A5 01      LDA $01`  
`F985 29 10      AND #$10`  
`F987 D0 0A      BNE $F993`  
`F989 98         TYA`  
`F98A 18         CLC`  
`F98B 69 06      ADC #$06`  
`F98D A8         TAY`  
`F98E A9 00      LDA #$00`  
`F990 8D 4E D0   STA $D04E`  
`F993 A2 02      LDX #$02`  
`F995 B9 0C FA   LDA $FA0C,Y`  
`F998 9D 41 D0   STA $D041,X`  
`F99B 88         DEY`  
`F99C CA         DEX`  
`F99D 10 F6      BPL $F995`  
`F99F A9 00      LDA #$00`  
`F9A1 8D 3F D0   STA $D03F  ;---`  
`F9A4 78         SEI`  
`F9A5 A9 7F      LDA #$7F`  
`F9A7 8D 00 DC   STA $DC00  ;Check `<CTRL>  
`F9AA AD 01 DC   LDA $DC01`  
`F9AD 29 04      AND #$04`  
`F9AF D0 13      BNE $F9C4  ;no, auto boot`  
`F9B1 58         CLI        ;yes, jump into basic`  
`F9B2 A9 0E      LDA #$0E`  
`F9B4 8D 20 D0   STA $D020`  
`F9B7 A9 06      LDA #$06`  
`F9B9 8D 21 D0   STA $D021`  
`F9BC A9 1B      LDA #$1B`  
`F9BE 8D 11 D0   STA $D011`  
`F9C1 6C 00 A0   JMP ($A000)`  
`F9C4 A2 FB      LDX #$FB   ;Auto boot `“`INTRO`”` from flash and go to $080D`  
`F9C6 9A         TXS`  
`F9C7 A9 08      LDA #$08`  
`F9C9 A8         TAY`  
`F9CA A2 01      LDX #$01`  
`F9CC 20 BA FF   JSR $FFBA`  
`F9CF AD FA F9   LDA $F9FA`  
`F9D2 A2 F5      LDX #$F5`  
`F9D4 A0 F9      LDY #$F9`  
`F9D6 20 BD FF   JSR $FFBD`  
`F9D9 A9 00      LDA #$00`  
`F9DB 20 D5 FF   JSR $FFD5`  
`F9DE 86 2D      STX $2D`  
`F9E0 84 2E      STY $2E`  
`F9E2 A9 93      LDA #$93`  
`F9E4 8D 00 DD   STA $DD00`  
`F9E7 A9 15      LDA #$15`  
`F9E9 8D 18 D0   STA $D018`  
`F9EC 58         CLI`  
`F9ED A9 0B      LDA #$0B`  
`F9EF 8D 11 D0   STA $D011`  
`F9F2 6C FB F9   JMP ($F9FB)`  
  
`F9F5 49 4E 54 52 4F    ; `“`INTRO`”  
`F9FA 05         .BYTE $05  ;length of name`  
`F9FB 0D 08      .WORD $080D    ;start address`  
  
`----------------------------------------------------------------------------`  
`; Jump #2: Save`  
`F9FD A5 BA      LDA $BA        ;Get device number`  
`F9FF C9 01      CMP #$01   ;Is it 1?`  
`FA01 F0 03      BEQ $FA06  ;yes, skip out.`  
`FA03 4C ED F5   JMP $F5ED  ;no, go to normal save`  
`FA06 A9 00      LDA #$00   ;Save to flash`  
`FA08 85 90      STA $90        ;Set status ok.`  
`FA0A 18         CLC`  
`FA0B 60         RTS        ;Exit`  
  
`;Burst Rate Modulus table`  
`FA0C 1C 13 2A   .BYTE $1C,$13,$2A  ;NTSC, 32.64 MHz`  
`FA0F 24 31 5B   .BYTE $24,$31,$5B  ;PAL,  31.36 MHz`  
`FA12 1C 00 00   .BYTE $1C,$00,$00  ;NTSC, 32.7272 MHz`  
`FA15 24 00 00   .BYTE $24,$00,$00  ;PAL,  31.5279MHz`  
  
`FA18 C4 D4 D6 38 30   ; `“`DTV80`”  
  
`----------------------------------------------------------------------------`  
`; Jump #3: Reset Video Mode`  
`FA1D A9 01      LDA #$01`  
`FA1F 8D 3F D0   STA $D03F`  
`FA22 A2 07      LDX #$07`  
`FA24 A9 00      LDA #$00`  
`FA26 9D 36 D0   STA $D036,X`  
`FA29 9D 45 D0   STA $D045,X`  
`FA2C CA         DEX`  
`FA2D 10 F7      BPL $FA26`  
`FA2F 8D 4D D0   STA $D04D`  
`FA32 A9 76      LDA #$76`  
`FA34 8D 36 D0   STA $D036`  
`FA37 8D 3A D0   STA $D03A`  
`FA3A 20 40 FA   JSR $FA40`  
`FA3D 4C A0 E5   JMP $E5A0  ;Continue to load the VIC-II defaults`  
  
`----------------------------------------------------------------------------`  
`; Jump #4: Reset Palette`  
`FA40 A9 01      LDA #$01   ;Reset Palette`  
`FA42 8D 3F D0   STA $D03F`  
`FA45 A2 0F      LDX #$0F`  
`FA47 BD 56 FA   LDA $FA56,X`  
`FA4A 9D 00 D2   STA $D200,X`  
`FA4D CA         DEX`  
`FA4E 10 F7      BPL $FA47`  
`FA50 A9 00      LDA #$00`  
`FA52 8D 3F D0   STA $D03F`  
`FA55 60         RTS`  
  
`FA56 00 0F 36 BE 58 DB 86 FF   ;Default Palette`  
`FA5E 29 26 3B 05 07 DF 9A 0A   ;`  
`----------------------------------------------------------------------------`  
  
`----------------------------------------------------------------------------`  
`FCE2 4C 2D F7   JMP $F72D  ;Reset Hook, was: LDX #$FF, SEI`  
`----------------------------------------------------------------------------`  
  
`----------------------------------------------------------------------------`  
`FD4C 30 F7      .WORD $F730    ;Load Vector, was: $F4A5`  
`FD4E 33 F7      .WORD $F733    ;Save Vector, was: $F5ED`  
`----------------------------------------------------------------------------`

The User Port Configuration bits
--------------------------------

`        PAL  NTSC  `  
`USR[0]   1    0    Pal-Ntsc`  
`USR[1]   1    0    PhaseAlt`  
`USR[2]   0    0    Sat0`  
`USR[3]   1    1    Sat1`  
`USR[4]   0    0    BurstLock`  
`USR[5]   0    0    Lockmode`  
`USR[6]   0    0    Tune`  
`USR[7]   1    0    PalBurst`  
`ATNin    0    0    ($01 bit 4) (XTal Select)`

Explanation of what the config code does:

`$D040 bits 1-0 = USR[1-0] (Burst phase alternate, PAL line timing)`  
`$D040 bits 7-2 = 0000000 (V1 Palette compatibility off)`  
`$D04F bits 3-0 = USR [5-2] (Burst lock with neg phase `“`walk`”`, Burst lock, SAT1, SAT0)`  
`$D04E = 0 if ATNin=0`  
`$D04E = 7 if USR[6]=0 and ATNin=1`  
`$D04E = 6 if USR[6]=1 and ATNin=1`  
`$D041=$1C, $D042=00, $D043=00 if USR[7]=0 and ATNin=0.`  
`$D041=$24, $D042=00, $D043=00 if USR[7]=1 and ATNin=0.`  
`$D041=$1C, $D042=13, $D043=2A if USR[7]=0 and ATNin=1.`  
`$D041=$24, $D042=31, $D043=5B if USR[7]=1 and ATNin=1.`  
`($D04E is the Scan line timing adjust. $D041,$D042,$D042 sets the Burst rate modulus)`

ROM Bugs
--------

**Tape I/O crashes:** The patch replaces several tape routines, but all
entry points are not patched, meaning that attempted tape operation may
simply crash in a strange way.

**Kernal load from flash returns faulty address in X/Y:** The kernal
load routine is supposed to return the end address in X/Y. (lsb/msb) It
also shall set $ae/$af to the end address. The routine at $f7e9
correctly sets the address in X/Y and $ae/$af, but unfortunately the
swap routine at $f84d then overwrites Y with $41 every time. X/Y are
used by basic to set up the end pointers $2d/$2e and others, so there
are problems loading basic programs (and other programs that rely on
$2d/$2e) from flash. Also note that the load routine will never return
with an error.

**Kernal verify from flash will result in load:** A kernal verify
operation is not supported for flash, but will not be ignored. Instead a
load operation will be performed.

**Palette reset:** Not called on normal FCE2 reset but on several other
events (RESTORE...). As the standard DTV kernal sets the same values as
are ASIC default, this does not show.

**Overwriting memory:** During LOAD from flash, $f7-$12f is saved to
$0110f7-$01112f and restored on load finish. This means that data at
$0110f7 gets overwritten on LOAD. Also, if a big file (extending to
$0110f7 and higher) gets loaded, after load $f7-$12f will be corrupted,
as is the data of the loaded program at $0110f7.

Note the standard C64 kernal overwrites several other memory locations
on reset: BASIC area, $FD30 (by $FD15 reset routine), first byte
encountered under ROM (usually $A000, by the memtest at $FD50). See [All
About Your 64](http://unusedino.de/ec64/technical/aay/c64/krnromma.htm)
documentation for details.

Most of these problems are fixed by tlr's
[DTVMON](DTVMON "wikilink")/[Kernalpatcher](Kernalpatcher "wikilink")
programs.

Directory Format
----------------

The directory is located at $010000-$013fff.

Each entry is 32 bytes long:

  
$00-$17 Filename max 24 bytes, padded with $00

$18-$1a Location of the file in flash.

$1b-$1d Load address of file in RAM.

$1e-$1f unused, always $00.

Directory scanning stops when the first byte of the entry is $00.

Note that the directory presented when doing LOAD“$” is just a file
named “$”, not the actual directory.

File Format
-----------

The files are compressed using a simple equal-sequence compression
algorithm.

Note the [Hummer](Hummer "wikilink") uses a slightly different format,
see [Hummer Kernal disassembly\#File
Format](Hummer_Kernal_disassembly#File_Format "wikilink").

-   Every chunk of data starts with a code byte.
-   If the code is $01-$7f, that number of bytes follow.
-   If the code is $80-$ff, then len=code & $7f, and a second byte
    follows
    -   this copies len bytes of data from the current
        destination-$0100+secondbyte.
-   If the code is $00, then we are done.

To just store the data without any compression, put an $7f byte before
every 127 bytes, and terminate with a $00 byte.

See also [DTVFSEdit](DTVFSEdit "wikilink").

### Example

“HELLO” loaded to $0801:

`$010018  00 00 10 01 08 00`  
`         ^^^^^^^^           location of file in flash ($100000)`  
`                  ^^^^^^^^  load address ($000801)`  
`$100000  05 08 05 0C 0C 0F 00 `  
`         ^^                    read five raw bytes`  
`            ^^^^^^^^^^^^^^     five raw bytes`  
`                           ^^  end marker`
