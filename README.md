Download Link: https://assignmentchef.com/product/solved-eth263-2210-hw-4-memory-interference-and-qos
<br>
<h1>. Memory System</h1>

A machine with a 4 GB DRAM main memory system has 4 channels, 1 rank per channel and 4 banks per rank. The cache block size is 64 bytes.

(a) You are given the following byte addresses and the channel and bank to which they are mapped:

Byte: 0<em>x</em>0000 ⇒ Channel                         0,      Bank      0

Byte: 0<em>x</em>0100 ⇒ Channel                         0,      Bank      0

Byte: 0<em>x</em>0200 ⇒ Channel                         0,      Bank      0

Byte: 0<em>x</em>0400 ⇒ Channel                         1,      Bank      0

Byte: 0<em>x</em>0800 ⇒ Channel                         2,      Bank      0

Byte: 0<em>x</em>0<em>C</em>00 ⇒ Channel                         3,      Bank      0

Byte: 0<em>x</em>1000 ⇒ Channel                         0,      Bank      1

Byte: 0<em>x</em>2000 ⇒ Channel                         0,      Bank      2

Byte: 0<em>x</em>3000 ⇒ Channel                         0,      Bank      3

Determine which bits of the address are used for each of the following address components. Assume row bits are higher order than column bits:

<ul>

 <li>Byte on bus Addr [ 2 : 0 ]</li>

 <li>Channel bits (channel bits are contiguous)</li>

</ul>

Addr [           :           ]

<ul>

 <li>Bank bits (bank bits are contiguous)</li>

</ul>

Addr [           :           ]

<ul>

 <li>Column bits (column bits are contiguous)</li>

</ul>

Addr [           :           ]

<ul>

 <li>Row bits (row bits are contiguous)</li>

</ul>

Addr [           :           ]

(b) Two applications App 1 and App 2 share this memory system (using the address mapping scheme you determined in part (a)). The memory scheduling policy employed is FR-FCFS. The following requests are queued at the memory controller request buffer at time t. Assume the first request (A) is the oldest and the last one (A + 15) is the youngest.

A B             A + 1      A + 2      A + 3      B + 10    A + 4      B + 12    A + 5      A + 6      A + 7 A + 8          A + 9      A + 10   A + 11   A + 12   A + 13   A + 14   A + 15

These are cache block addresses, not byte addresses. Note that requests to A + x are from App 1, while requests to B + x are from App 2. Addresses A and B are row-aligned (i.e., they are at the start of a row) and are at the same bank but are in different rows.

Assuming row-buffer hits take T time units to service and row-buffer conflicts/misses take 2T time units to service, what is the slowdown (compared to when run alone on the same system) of

<ul>

 <li>App 1?</li>

 <li>App 2?</li>

</ul>

(d) In class, we discussed memory channel partitioning and memory request scheduling as two solutions to mitigate interference and application slowdowns in multicore systems. Propose another solution to reduce the slowdown of the more-slowed-down application, without increasing the slowdown of the other application? Be concrete.

<h1>3. DRAM Scheduling and Latency</h1>

You would like to understand the configuration of the DRAM subsystem of a computer using reverse engineering techniques. Your current knowledge of the particular DRAM subsystem is limited to the following information:

<ul>

 <li>The physical memory address is 16 bits.</li>

 <li>The DRAM subsystem consists of a single channel, 2 banks, and 64 rows per bank.</li>

 <li>The DRAM is byte-addressable.</li>

 <li>The most-significant bit of the physical memory address determines the bank. The following 6 bits of the physical address determine the row.</li>

 <li>The DRAM command bus operates at 1 GHz frequency.</li>

 <li>The memory controller issues commands to the DRAM in such a way that <em>no command </em>for servicing a <em>later </em>request is issued before issuing a READ command for the current request, which is the oldest request in the request buffer. For example, if there are requests A and B in the request buffer, where A is the older request and the two requests are to different banks, the memory controller does <em>not </em>issue an ACTIVATE command to the bank that B is going to access <em>before </em>issuing a READ command to the bank that A is accessing.</li>

 <li>The memory controller services requests in order with respect to each bank. In other words, for a given bank, the memory controller first services the oldest request in the <em>request buffer </em>that targets the same bank. If all banks are ready to service a request, the memory controller first services the oldest request in the request buffer.</li>

</ul>

You realize that you can observe the memory requests that are waiting to be serviced in the request buffer. At a particular point in time, you take the snapshot of the request buffer and you observe the following requests in the request buffer (in descending order of request age, where the oldest request is on the top):

y Read 0xD780

Read 0x280C

Read 0xE4D0

Read 0x2838

At the same time you take the snapshot of the request buffer, you start probing the DRAM command bus. You observe the DRAM command type and the cycle (relative to the first command) at which the command is seen on the DRAM command bus. The following are the DRAM commands you observe on the DRAM bus while the requests above are serviced.

Cycle 0 — READ

Cycle 1 — PRECHARGE

Cycle 8 — PRECHARGE

Cycle 13 — ACTIVATE Cycle 18 — READ

Cycle 20 — ACTIVATE Cycle 22 — READ

Cycle 25 — READ

Answer the following questions using the information provided above.

<ul>

 <li>What are the following DRAM timing parameters used by the memory controller, in terms of nanoseconds? If there is not enough information to infer the value of a timing parameter, write <em>unknown</em>.</li>

</ul>

<ol>

 <li>i) ACTIVATE-to-READ latency:</li>

</ol>

<ul>

 <li>What is the status of the banks <em>prior </em>to the execution of any of the above requests? In other words, which rows from which banks were open immediately prior to issuing the DRAM commands listed above? Fill in the table below indicating whether a bank has an open row, and if there is an open row, specify its address. If there is not enough information to infer the open row address, write <em>unknown</em>.</li>

</ul>

<table width="332">

 <tbody>

  <tr>

   <td width="58"> </td>

   <td width="128"><strong>Open or Closed?</strong></td>

   <td width="146"><strong>Open Row Address</strong></td>

  </tr>

  <tr>

   <td width="58">Bank 0</td>

   <td width="128"> </td>

   <td width="146"> </td>

  </tr>

  <tr>

   <td width="58">Bank 1</td>

   <td width="128"> </td>

   <td width="146"> </td>

  </tr>

 </tbody>

</table>

<ul>

 <li>To improve performance, you decide to implement the idea of Tiered-Latency DRAM (TL-DRAM) in the DRAM chip. Assume that a bank consists of a single subarray. With TL-DRAM, an entire bank is divided into a near segment and far segment. When accessing a row in the near segment, the ACTIVATE-to-READ latency <em>reduces </em>by 1 cycle and the ACTIVATE-to-PRECHARGE latency reduces by 3 cycles. When precharging a row in the near segment, the PRECHARGE-to-ACTIVATE latency reduces by 3 cycles. When accessing a row in the far segment, the ACTIVATE-to-READ latency <em>increases </em>by 1 cycle and the ACTIVATE-to-PRECHARGE latency increases by 2 cycles. When precharging a row in the far segment, the PRECHARGE-to-ACTIVATE latency increases by 2 cycles. The following table summarizes the changes in the affected latency parameters.</li>

</ul>

<table width="521">

 <tbody>

  <tr>

   <td width="193">Timing Parameter</td>

   <td width="169"><strong>Near Segment Latency</strong></td>

   <td width="158"><strong>Far Segment Latency</strong></td>

  </tr>

 </tbody>

</table>

<table width="521">

 <tbody>

  <tr>

   <td width="193">ACTIVATE-to-READ</td>

   <td width="169">−1</td>

   <td width="158">+1</td>

  </tr>

  <tr>

   <td width="193">ACTIVATE-to-PRECHARGE</td>

   <td width="169">−3</td>

   <td width="158">+2</td>

  </tr>

  <tr>

   <td width="193">PRECHARGE-to-ACTIVATE</td>

   <td width="169">3</td>

   <td width="158">+2</td>

  </tr>

 </tbody>

</table>

−

Assume that the rows in the near segment have smaller row ids compared to the rows in the far segment. In other words, physical memory row addresses 0 through <em>N </em>−1 are the near-segment rows, and physical memory row addresses <em>N </em>through 63 are the far-segment rows.

If the above DRAM commands are issued 2 cycles faster with TL-DRAM compared to the baseline (the last command is issued in cycle 23), how many rows are in the near segment, i.e., what is <em>N</em>? Show your work.

<h1>4. Memory Scheduling [200 points]</h1>

To serve a memory request, the memory controller issues one or multiple DRAM commands to access data from a bank. There are four different DRAM commands.

<ul>

 <li>ACTIVATE: Loads the row (that needs to be accessed) into the bank’s row-buffer. This is called <em>opening </em>a row. <strong>(Latency: 15ns)</strong></li>

 <li>PRECHARGE: Restores the contents of the bank’s row-buffer back into the row. This is called <em>closing </em>a row. <strong>(Latency: 15ns)</strong></li>

 <li>READ/WRITE: Accesses data from the row-buffer. <strong>(Latency: 15ns)</strong></li>

</ul>

The following figure shows the snapshot of the memory request buffers (in the memory controller) at <em>t</em><sub>0</sub>. Each request is color-coded to denote the application to which it belongs (assume that all applications are running on separate cores). Additionally, each request is annotated with the address (or index) of the row that the request needs to access (e.g., <em>R3 </em>means that the request is to the 3<em><sup>rd </sup></em>row). Furthermore, assume that all requests are read requests.

A memory request is considered to be <em>served </em>when the READ command is complete (i.e., 15ns after the request’s READ command has been issued). In addition, each application (A, B, or C) is considered to be <strong>stalled </strong>until <em>all </em>of its memory requests (across all the request buffers) have been served.

Assume that, initially (at <em>t</em><sub>0</sub>) each bank has the 3<em><sup>rd </sup></em>and the 12<em><sup>th </sup></em>row loaded in the row-buffer, respectively.

Furthermore, no additional requests from any of the applications arrive at the memory controller.

<h2>4.1. Application-Unaware Scheduling Policies</h2>

(a) Using the <strong>FCFS </strong>scheduling policy, what is the <strong>stall time </strong>of each application?

(d) Briefly describe the scheduling policy that would <strong>maximize </strong>the <em>request throughput </em>at any given bank, where request throughput is defined as the number of requests served per unit amount of time. (Less than 10 words.)

<h2>4.2. Application-Aware Scheduling Policies</h2>

Of the three applications, application C is the least memory-intensive (i.e., has the lowest number of outstanding requests). However, it experiences the largest stall time since its requests are served only after the numerous requests from other applications are first served. To ensure the shortest stall time for application C, one can assign its requests with the highest priority, while assigning the same low priority to the other two applications (A and B).

<ul>

 <li><strong>Scheduling Policy X: </strong>When application C is assigned a high priority and the other two applications are assigned the same low priority, what is the stall time of each application? (Among requests with the same priority, assume that <em>FR-FCFS </em>is used to break ties.)</li>

</ul>

Can you design an even better scheduling policy? While application C now experiences low stall time, you notice that the other two applications (A and B) are still delaying each other.

<ul>

 <li>Assign priorities to the other two applications such that you minimize the average stall time across all applications. Specifically, list all <strong>three </strong>applications in the order of highest to lowest priority. (Among requests with the same priority, assume that <em>FR-FCFS </em>is used to break ties.)</li>

</ul>

<h1>5. Memory Request Scheduling</h1>

A machine has a DRAM main memory organized as 2 channels, 1 rank and 2 banks/channel. An open row policy is used, i.e., a row is retained in the row-buffer after an access until an access to another row is made. The following commands (defined as we discussed in class) can be issued to DRAM with the given latencies:

<ul>

 <li>ACTIVATE: 15 ns</li>

 <li>PRECHARGE: 15 ns</li>

 <li>READ/WRITE: 15 ns</li>

</ul>

Assume the bus latency is 0 cycles.

(a) We studied Parallelism Aware Batch Scheduling (PAR-BS) in class. We will use a PAR-BS-like scheduler that we will call X. This scheduler operates as follows:

<ul>

 <li>The scheduler forms request batches consisting of the 4 oldest requests from each application at each bank.</li>

 <li>At each bank, the scheduler ranks applications based on the number of requests they have outstanding at that bank. Applications with a smaller number of requests outstanding are assigned a higher rank.</li>

 <li>The scheduler always ranks the application with the oldest request higher in the event of a tie between applications.</li>

 <li>The scheduler prioritizes the requests of applications based on this ranking (higher ranked applications requests are prioritized over lower ranked applications requests).</li>

 <li>The scheduler repeats the above steps once all requests in a batch are serviced at all banks.</li>

</ul>

Channel 0

Youngest                                                                         Oldest

<table width="66">

 <tbody>

  <tr>

   <td width="66">

    <table width="54">

     <tbody>

      <tr>

       <td width="54">Bank 0</td>

      </tr>

     </tbody>

    </table>Row 4

    <table width="54">

     <tbody>

      <tr>

       <td width="54">Bank 1</td>

      </tr>

     </tbody>

    </table>Row 1024</td>

  </tr>

 </tbody>

</table>

R6      R4     R6      R4      R5     R4      R5      R4

App A

App B

Channel 1

<table width="66">

 <tbody>

  <tr>

   <td width="66">

    <table width="54">

     <tbody>

      <tr>

       <td width="54">Bank 0</td>

      </tr>

     </tbody>

    </table>Row 1024

    <table width="54">

     <tbody>

      <tr>

       <td width="54">Bank 1</td>

      </tr>

     </tbody>

    </table>Row 10</td>

  </tr>

 </tbody>

</table>

Youngest                                                                         Oldest

R2      R3      R3      R3      R3     R2      R2      R2

For the same above buffer state:

What is the stall time of application A using this scheduler?

What is the stall time of application B using this scheduler?

<ul>

 <li>Can you design a better memory scheduler (i.e., one that provides higher system performance) than</li>

</ul>

X? Circle one:                                                      <strong>YES                              NO</strong>

If yes, answer the questions below. What modifications would you make to scheduler X to design this better scheduler Y? Explain clearly.

<ul>

 <li>Consider a simple channel partitioning scheme where application A’s data is mapped to channel 0 and application B’s data is mapped to channel 1. When data is mapped to a different channel, only the channel number changes; the bank number does not change. For instance, requests of application A that were mapped to bank 1 of channel 1 would now be mapped to bank 1 of channel 0. What is the stall time of application A using this channel partitioning mechanism and an FR-FCFS memory scheduler?</li>

</ul>

What is the stall time of application B using this channel partitioning mechanism and an FR-FCFS memory scheduler?

<h1>6. Emerging Memory Technologies [100 points]</h1>

Computer scientists at ETH developed a new memory technology, ETH-RAM, which is non-volatile. The access latency of ETH-RAM is close to that of DRAM while it provides higher density compared to the latest DRAM technologies. ETH-RAM has one shortcoming, however: it has limited endurance, i.e., a memory cell stops functioning after 10<sup>6 </sup>writes are performed to the cell (known as cell wear-out).

A bright ETH student has built a computer system using 1GB of ETH-RAM as main memory. ETHRAM exploits a perfect wear-leveling mechanism, i.e., a mechanism that equally distributes the writes over all of the cells of the main memory.

<ul>

 <li>This student is worried about the lifetime of the computer system she has built. She executes a test program that runs special instructions to bypass the cache hierarchy and repeatedly writes data into different words until <strong>all </strong>the ETH-RAM cells are worn-out (stop functioning) and the system becomes useless. The student’s measurements show that ETH-RAM stops functioning (i.e., all its cells are worn-out) in one year (365 days). Assume the following:

  <ul>

   <li>The processor is in-order and there is no memory-level parallelism.</li>

   <li>It takes 5ns to send a memory request from the processor to the memory controller and it takes 28ns to send the request from the memory controller to ETH-RAM.</li>

   <li>ETH-RAM is word-addressable. Thus, each write request writes 4 bytes to memory. What is the write latency of ETH-RAM? Show your work.</li>

  </ul></li>

 <li>ETH-RAM works in the multi-level cell (MLC) mode in which each memory cell stores 2 bits. The student decides to improve the lifetime of ETH-RAM cells by using the single-level cell (SLC) mode. When ETH-RAM is used in SLC mode, the lifetime of each cell improves by a factor of 10 and the write latency decreases by 70%. What is the lifetime of the system using the SLC mode, if we repeat the experiment in part (a), with everything else remaining the same in the system? Show your work.</li>

</ul>

<h2>7. BossMem [100 points]</h2>

A researcher has developed a new type of nonvolatile memory, BossMem. He is considering BossMem as a replacement for DRAM. BossMem is 10x faster (all memory timings are 10x faster) than DRAM, but since BossMem is so fast, it has to frequently power-off to cool down. Overheating is only a function of time, not a function of activity. An idle stick of BossMem has to power-off just as frequently as an active stick. When powered-off, BossMem retains its data, but cannot service requests. Both DRAM and BossMem are banked and otherwise architecturally similar. To the researcher’s dismay, he finds that a system with 1GB of DRAM performs considerably better than the same system with 1GB of BossMem.

<ul>

 <li>What can the researcher change or improve in the core (he can’t change BossMem or anything beyond the memory controller) that will make his BossMem perform more favorably compared to DRAM, realizing that he will have to be fair and evaluate DRAM with his enhanced core as well? (15 words or less)</li>

 <li>A colleague proposes he builds a hybrid memory system, with both DRAM and BossMem. He decides to place data that exhibits low row buffer locality in DRAM and data that exhibits high row buffer locality in BossMem. Assume 50% of requests are row buffer hits. Is this a good or bad idea? Show your work.</li>

 <li>Now a colleague suggests trying to improve the last-level cache replacement policy in the system with the hybrid memory system. Like before, he wants to improve the performance of this system relative to one that uses just DRAM and he will have to be fair in his evaluation. Can he design a cache replacement policy that makes the hybrid memory system look more favorable? In 15 words or less, justify NO or describe a cache replacement policy that would improve the performance of the hybrid memory system more than it would DRAM.</li>

 <li>In class we talked about another nonvolatile memory technology, phase-change memory (PCM). Which technology, PCM, BossMem, or DRAM requires the greatest attention to security? What is the vulnerability?</li>

</ul>