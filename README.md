# Go Back  N Protocol

**Go-Back-N protocol**, also called Go-Back-N Automatic Repeat reQuest, is a data link layer protocol that uses a sliding window method
for reliable and sequential delivery of data frames. It is a case of sliding window protocol having to send window size of N and 
receiving window size of 1.

## Working Principle:
Go – Back – N ARQ provides for sending multiple frames before receiving the acknowledgment for the first frame. The frames are sequentially numbered and a finite number of frames. The maximum number of frames that can be sent depends upon the size of the sending window. If the acknowledgment of a frame is not received within an agreed upon time period, all frames starting from that frame are retransmitted.

The size of the sending window determines the sequence number of the outbound frames. If the sequence number of the frames is an n-bit field, then the range of sequence numbers that can be assigned is 0 to 2n−1. Consequently, the size of the sending window is 2n−1. Thus in order to accommodate a sending window size of 2n−1, a n-bit sequence number is chosen.

The sequence numbers are numbered as modulo-n. For example, if the sending window size is 4, then the sequence numbers will be 0, 1, 2, 3, 0, 1, 2, 3, 0, 1, and so on. The number of bits in the sequence number is 2 to generate the binary sequence 00, 01, 10, 11.

The size of the receiving window is 1.

## Sender Site Algorithm of Go-Back-N Protocol
```
begin
   frame s;                      //s denotes frame to be sent
   frame t;                      //t is temporary frame
   S_window = power(2,m) – 1;    //Assign maximum window size
   SeqFirst = 0;       // Sequence number of first frame in window
   SeqN = 0;          // Sequence number of Nth frame window
   while (true)       //check repeatedly
   do
      Wait_For_Event();       //wait for availability of packet
      if ( Event(Request_For_Transfer)) then
         //check if window is full
         if (SeqN–SeqFirst >= S_window) then
            doNothing();
         end if;
         Get_Data_From_Network_Layer();
         s = Make_Frame();
         s.seq = SeqN;
         Store_Copy_Frame(s);
         Send_Frame(s);
         Start_Timer(s);
         SeqN = SeqN + 1;
      end if;
      if ( Event(Frame_Arrival) then
         r = Receive_Acknowledgement();
         if ( AckNo > SeqFirst && AckNo < SeqN ) then
            while ( SeqFirst <= AckNo )
               Remove_copy_frame(s.seq(SeqFirst));
               SeqFirst = SeqFirst + 1;
            end while
            Stop_Timer(s);
         end if
      end if
      // Resend all frames if acknowledgement havn’t been received
      if ( Event(Time_Out)) then
         TempSeq = SeqFirst;
         while ( TempSeq < SeqN )
            t = Retrieve_Copy_Frame(s.seq(SeqFirst));
            Send_Frame(t);
            Start_Timer(t);
            TempSeq = TempSeq + 1;
         end while
      end if
end
```

## Receiver Site Algorithm of Go-Back-N Protocol
```
Begin
   frame f;
   RSeqNo = 0; // Initialise sequence number of expected frame
   while (true) //check repeatedly
   do
      Wait_For_Event(); //wait for arrival of frame
      if ( Event(Frame_Arrival) then
         Receive_Frame_From_Physical_Layer();
         if ( Corrupted ( f.SeqNo )
            doNothing();
         else if ( f.SeqNo = RSeqNo ) then
            Extract_Data();
            Deliver_Data_To_Network_Layer();
            RSeqNo = RSeqNo + 1;
            Send_ACK(RSeqNo);
         end if
      end if
   end while
end
```

- Link for more information: [Click Here](https://en.wikipedia.org/wiki/Go-Back-N_ARQ)
