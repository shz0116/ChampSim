# Immport Flowcharts
## Level Hierachy:
CPU_L1I(level=1), CPU_ITLB(level=1), CPU_DTLB(level=1)
CPU_STLB(level=2)
CPU_PTW(level=3)
CPU_L1D(level=4)
CPU_L2C(level=5)
LLC(level=6)

## direction:
to return (up)        lower_level (down)  
  
# ooo_cpu.cc:
## check_dib()
1. check insts in IFETCH_BUFFER from DIB (decode instruction buffer, cache) and 
if so mark as 
  instr.translation = COMPLETE
  instr.fetched = COMPELTE
  instr.decoded = COMPLETE
## translate_fetch()
1. check IFETCH_BUFFER for translations and send packet to ITLB
## fetch_instruction()
1. fetch those instructions in IFETCH_BUFFER (translated but not fetched),
   send packet to L1I,
   makr instr->fetched=INFLIGHT
## promote_to_decode()
1. move translated&fetched instrs in IFETCH_BUFFER to DECODE_BUFFER
## decode_instruction()
1. update DIB and move instrs from DECODE_BUFFER to DISPATCH_BUFFER
## dispatch_instruction()
1. move instrs from DISPATCH_BUFFER to ROB
## schedule_memory_instruction()
1. put load/store requests of ROB intructions into LD/ST queue
2. mark: instr->scheduled=COMPLETE, execute=INFLIGHT
3. STA ???
### add_load_queue()
1. check RAW relationship, store forward, or add load into RTL0
### add_store_queue()
2. add to RTS0
## operate_lsq()
1. translate RTS0 and send packet to DTLB, execute RTS1
2. translate RTL0 and send packet to DTLB, execute RTL1 and send packet to L1D
## handle_memory_return()
1. process return from ITLB, mark instr->translated=COMPLETE
2. process return from L1I,  mark instr->fetched=COMPELTE
3. process return from DTLB, mark instr->translated=COMPELETE, merge packets into RTS1, RTL1.
4. process return from L1D,  mark instr->fetched=COMPELTE
## schedule_instruction()
1. mark instr->execute=INFLIGHT
## execute_instruction()
1. mark instr->execute=INFLIGHT
## retire_rob()
1. create packets for L1D write queue
2. pop rob entry

#cache.cc:
## add_rq()
1. check forwarded from wq first
2. check duplication. If so, merge request
3. add to RQ
## add_wq()
1. check duplication
2. add to WQ
## prefetch_line()
1. add request packet into VAPQ or add_pq based on whether virtual address prefetch or not
## va_translate_prefetches():
1. translate VAPQ.front() and add to pq
2. 

#ptw.cc:
## handle_read()
1. Handle RQ
2. Check address translation level from PSCL5-2, create packet, insert into MSHR, also to rq in lower_level (L1D)
## handle_fill()
1. Hanle MSHR
2. If translation level reaches 0, add translation into PSCLs
3. Else create new packet and replace in MSHR


