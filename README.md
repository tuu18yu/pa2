# Programming Assignment 2

To run: sudo ./run.sh
(No new packages are added from the starter code)

----------------------------------------------------------

Questions:
1. Why do you see a difference in webpage fetch times with small and large router buffers?

When q=20, average: 0.504805555556 std dev: 0.074230540264
When q=100, average: 1.11019047619 std dev: 0.581217330144
From the results shown above, we can see that in larger router buffer, average fetch time is double the average of smaller router buffer.
Additionally, standard deviation when q=100 is greater, indicating the fetch times were more inconsistent compare to when q=20.

This difference in fetch time happened, as in TCP, window size is keep increased until lost packet is detected when buffer is full.
And when buffer is large, more packets can be queued, which means it takes longer for packet to be lost. Then in meantimes, as no packets are
lost, TCP will keep increase the window size and sending packets at a faster rate. When this happens the rate of packets filling up the queue
becomes higher than packets leaving the queue, increasing the latency. Therefore as the packets have to wait a longer time in the queue,
when buffer is large, fetech time is longer in large buffer router.

2. Bufferbloat can occur in other places such as your network interface card (NIC). Check the output of ifconfig eth0 on your VirtualBox VM.
What is the (maximum) transmit queue length on the network interface reported by ifconfig? For this queue size and a draining rate of 100 Mbps, 
what is the maximum time a packet might wait in the queue before it leaves the NIC?

The output of ifconfig eth0 reports txqueuelen:1000, so transmit queue length is 1000. Then when queue is full it would hold
1000 packets of 1500 bytes. Then it is 1000 * 1500 * 8 = 12,000,000 bits.

Assuming draining rate of 100 Mbps, the maximum time packet has to wait is 12,000,000 / (100 * 10^6) = 0.12 seconds.

3. How does the RTT reported by ping vary with the queue size? Write a symbolic equation to describe the relation between the two (ignore computation overheads in ping that might affect the final result).

Through observartion in graph q.png and rtt.png, RTT and queue have proportional relationship. Then if we let Q to be queue size, and
x to be some value that represents the different between the two, the equation is: RTT = Q + x.

4. Identify and describe two ways to mitigate the bufferbloat problem.

    (1) Smaller buffer size, as shown above. 
    
    (2) Shaping the traffic in a way that buffer in never full, that is rate of packets leaving the buffer is faster than packets
    arriving at the bufefer.
