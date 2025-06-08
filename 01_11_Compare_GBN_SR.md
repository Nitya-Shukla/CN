# Detailed Answer: Comparison of Go-Back-N (GBN) ARQ and Selective Repeat (SR) ARQ

Go-Back-N (GBN) ARQ and Selective Repeat (SR) ARQ are both sliding window protocols designed to provide reliable data transmission by handling errors and losses. However, they differ significantly in their approach to retransmission, acknowledgement, buffering, and overall complexity, leading to different performance characteristics, especially under varying network conditions.

---

## Core Differences at a Glance

| Feature                     | Go-Back-N (GBN) ARQ                                     | Selective Repeat (SR) ARQ                                      |
|-----------------------------|---------------------------------------------------------|----------------------------------------------------------------|
| **Retransmission Strategy** | Retransmits the lost/errored frame AND all subsequent frames in the current sender window (Goes Back N). | Retransmits ONLY the specific lost/errored frame(s).           |
| **Receiver Window Size (Wr)** | Wr = 1                                                  | Wr > 1 (typically Ws = Wr)                                     |
| **Buffering at Receiver**   | Buffers only one in-sequence frame. Discards out-of-order frames. | Buffers correctly received out-of-order frames within its window. |
| **Acknowledgements (ACKs)** | Cumulative (ACK N implies frames up to N-1 are received). | Individual (ACK N implies frame N is received).                |
| **Sender Timers**           | Typically one timer for the oldest unacknowledged frame.  | Individual timer for each unacknowledged frame.              |
| **Complexity (Receiver)**   | Simpler.                                                | More complex (due to out-of-order buffering and reassembly). |
| **Complexity (Sender)**     | Simpler (retransmits a block).                          | More complex (tracks individual frames and timers).          |
| **Efficiency on Noisy Links**| Can be inefficient due to retransmitting correct frames. | More efficient as only lost/errored frames are resent.       |
| **Sequence Number Space**   | Ws can be up to `2^m - 1`.                              | Ws + Wr <= `2^m` (often Ws = Wr = `2^(m-1)`).                  |

---

## Detailed Comparison Points

**1. Retransmission Strategy:**

*   **GBN:** If a frame (say, Frame `i`) is lost or its timer expires, GBN resends Frame `i` and all subsequent frames (`i+1`, `i+2`, ...) that were part of the sender's window at the time of timeout or error detection. This is because the receiver, with a window size of 1, discards any frame that does not arrive in the expected sequence.
*   **SR:** If a frame (Frame `i`) is lost or its timer expires, SR resends only Frame `i`. Other frames, even those sent after Frame `i`, are not retransmitted unless their own timers expire or a NAK is received for them.

**2. Receiver Window Size and Buffering:**

*   **GBN:** The receiver window size is 1. It only accepts the frame it is currently expecting. Any frame arriving out of order is discarded. This simplifies the receiver, as no buffering of out-of-order frames is needed.
*   **SR:** The receiver window size is greater than 1 (typically equal to the sender window size). It can accept and buffer correctly received frames that arrive out of order, as long as they fall within its current window. These frames are delivered to the network layer in sequence once all missing preceding frames arrive.

**3. Acknowledgements:**

*   **GBN:** Uses cumulative acknowledgements. An ACK `N` confirms that all frames up to `N-1` have been received successfully and the receiver is expecting Frame `N`. This means a single ACK can acknowledge multiple frames.
*   **SR:** Typically uses individual acknowledgements. An ACK `N` confirms that Frame `N` itself has been received. This allows for more precise information about which frames have made it through.

**4. Sender and Receiver Complexity:**

*   **GBN:**
    *   *Sender:* Relatively simpler in terms of retransmission logic (just resend a block). Needs to track only one timer (for the oldest outstanding frame) or manage timers such that a timeout triggers a go-back action.
    *   *Receiver:* Very simple. No out-of-order buffering or reassembly logic needed.
*   **SR:**
    *   *Sender:* More complex. Must maintain a timer for each individual unacknowledged frame. Must be able to retransmit specific frames.
    *   *Receiver:* More complex. Must manage buffers for out-of-order frames, handle individual ACKs, and reassemble frames in the correct sequence before passing them to the network layer.

**5. Efficiency and Throughput:**

*   **GBN:**
    *   *Pros:* More efficient than Stop-and-Wait because it allows multiple frames in transit.
    *   *Cons:* Performance degrades significantly as the error rate or bandwidth-delay product increases. Retransmitting multiple frames when only one was lost is wasteful of bandwidth.
*   **SR:**
    *   *Pros:* Generally the most efficient in terms of bandwidth utilization, especially on links with high error rates or long delays. Only retransmitting necessary frames minimizes redundant data.
    *   *Cons:* The overhead of managing individual timers and more complex buffering can add processing overhead.

**6. Window Size and Sequence Numbers:**

*   **GBN:** The sender window size `Ws` can be up to `2^m - 1` (where `m` is the number of bits for sequence numbers). For example, with 3 bits for sequence numbers (0-7), Ws can be up to 7.
*   **SR:** The sum of the sender and receiver window sizes must be less than or equal to `2^m`. A common and safe configuration is `Ws = Wr = 2^(m-1)`. For 3 bits (0-7), `Ws = Wr = 2^(3-1) = 4`. This constraint is crucial to prevent a scenario where old duplicate frames are mistaken for new frames after sequence numbers wrap around.

---

## When to Use Which?

*   **Go-Back-N ARQ:**
    *   Suitable for links with relatively low error rates where the overhead of retransmitting an entire window is infrequent.
    *   When simplicity at the receiver is a priority.
    *   When memory for buffering at the receiver is limited.
    *   Older implementations or systems where processing power is a constraint.

*   **Selective Repeat ARQ:**
    *   Best for links with high error rates and/or high bandwidth-delay products (e.g., satellite links, long-distance wireless).
    *   When maximizing bandwidth utilization is critical.
    *   When sufficient processing power and buffer memory are available at both sender and receiver.
    *   Most modern reliable transport protocols (like TCP, though it's Layer 4) use mechanisms conceptually similar to Selective Repeat (e.g., SACK - Selective Acknowledgement option in TCP).

---

## Diagrammatic Comparison (Illustrative of Retransmission)

**Scenario: Frame 1 is Lost, Window Size = 3**

**Go-Back-N:**
```
Sender                      Receiver
  |--F0--------------------->| Rcvs F0, ACK1
  |--F1----------X--------->|<----ACK1-------|
  |--F2--------------------->| Rcvs F2 (out-of-order, expects F1), Discards F2, Resends ACK1
  (Timer for F1 expires)    |<----ACK1-------|
  Sender GOES BACK TO F1:
  |--F1'-------------------->| Rcvs F1', ACK2
  |--F2'-------------------->|<----ACK2-------|
  |--F3'-------------------->| Rcvs F2', ACK3
                              | Rcvs F3', ACK4
                              ...
(F2 was retransmitted even if it arrived correctly the first time at the physical layer)
```

**Selective Repeat:**
```
Sender                      Receiver
  |--F0--------------------->| Rcvs F0, ACK0, Deliver F0
  |--F1----------X--------->|<----ACK0-------|
  |--F2--------------------->| Rcvs F2 (out-of-order, expects F1), Buffers F2, Sends ACK2
  (Timer for F1 expires)    |<----ACK2-------|
  Sender RETRANSMITS ONLY F1:
  |--F1'-------------------->| Rcvs F1', ACK1. Delivers F1, then buffered F2.
                              |<----ACK1-------|
  ...
(F2 was not retransmitted as it was buffered and ACKed)
```

---

## Exam Angle Summary (Based on `pyqs.md`)

*   **Direct Comparison:** This is a very common exam question. Be ready to "Compare Go-Back-N and Selective Repeat Protocols" (May 2019, May 2018 CBGS). Structure your answer around the key differences highlighted above (retransmission, window sizes, buffering, ACKs, complexity, efficiency).
*   **Individual Explanations Leading to Comparison:** Often, questions might ask to explain GBN and SR separately and then compare: "Explain the working principle of Go-Back-N and Selective Repeat protocols with their relative comparison" (Dec 2024).
*   **Focus on Trade-offs:** Emphasize that Selective Repeat offers better performance in error-prone environments at the cost of increased complexity, while Go-Back-N is simpler but can be inefficient.
*   **Use Cases:** Briefly mentioning typical scenarios where one might be preferred over the other adds depth to the comparison.

A table summarizing the differences is often a good way to start or conclude your comparison, followed by more detailed explanations of each point. 