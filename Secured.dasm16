; Try some basic stuff
   SET A, 0x30
   SET [0xB000], 0x20
   SUB A, [0xB000]
   SET [0x04], crash
   SET [0x05], PC
   IFN A, 0x0
    SET PC, jmp_patcher

; Do a loopy thing
   SET I, 10
   SET A, 0x2000
:loop
   SET [0x02], [0xC000+I]
   AND [0x02], 0x3FFF
   SET [0x03], [0xA000+A]
   SET [0x02], [0x03]
   SUB I, 1
   SET [0x04], loop
   SET [0x05], PC
   IFN I, 0
    SET PC, jmp_patcher

; Call a subroutine
   SET X, 0x4
   SET [0x04], testsub
   SET [0x05], PC
   SET PC, jsr_patcher
   SET PC, crash

:testsub
   SHL X, 4
   SET PC, ret_patcher

; Hang forever. X should now be 0x40 if everything went right.
:crash
   SET [0x04], crash
   SET [0x05], PC
   SET PC, jmp_patcher
   
:jmp_patcher
   SUB [0x05], [0x06]
   ; call scheduler with time-delta = [0x05 + 2]
   SET [0x06], [0x04]
   SET PC, [0x04]
:jsr_patcher
   SET PUSH, [0x05]
   ADD PEEK, 1
   SUB [0x05], [0x06]
   ; call scheduler with time-delta = [0x05 + 2]
   SET [0x06], [0x04]
   SET PC, [0x04]
:ret_patcher
   SET PC, POP