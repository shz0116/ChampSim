# ooo_cpu.cc:
## check_dib:
1. check insts in IFETCH_BUFFER from DIB (decode instruction buffer, cache) and 
if so mark as 
  instr.translation = COMPLETE
  instr.fetched = COMPELTE
  instr.decoded = COMPLETE
## translate_fetch:
1. check IFETCH_BUFFER for translations and send packet to ITLB
## fetch_instruction
1. fetch those instructions in IFETCH_BUFFER (translated but not fetched),
   send packet to L1I,
   makr instr->fetched=INFLIGHT
   