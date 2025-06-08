# Detailed Answer: Selective Repeat ARQ

Selective Repeat ARQ (Automatic Repeat reQuest), also known as Selective Reject ARQ, is another sliding window protocol designed for reliable data transmission. It is more efficient than Go-Back-N ARQ, especially on noisy links, because it retransmits only those frames that are actually lost or corrupted, rather than retransmitting an entire window of frames.

---

## Core Concepts

*   **Sliding Window:** Both sender and receiver maintain windows of sequence numbers.
*   **Pipelining:** Multiple frames can be in transit simultaneously.
*   **Individual/Selective Acknowledgements (ACKs):** The receiver acknowledges each correctly received frame individually. Some implementations might use Negative Acknowledgements (NAKs or SREJs for Selective Reject) to explicitly request retransmission of a specific missing frame.
*   **Out-of-Order Buffering:** The receiver has a window size greater than 1 and can accept and buffer frames that arrive out of order, as long as they fall within its receiving window. These buffered frames are delivered to the network layer in sequence once all preceding frames have been received.
*   **Sender and Receiver Window Sizes (Ws and Wr):**
    *   Both `Ws` and `Wr` can be greater than 1.
    *   A key constraint for unambiguous operation is that `Ws + Wr <= 2^m`, where `m` is the number of bits used for sequence numbers. Often, `Ws = Wr = 2^(m-1)` is chosen.

---

## Working Principle of Selective Repeat ARQ

1.  **Initialization:** Sender and receiver windows are defined. `Ws > 1`, `Wr > 1` (typically `Ws = Wr`).
2.  **Sending Frames:** The sender can transmit up to `Ws` frames without waiting for individual ACKs for each. Each frame has a unique sequence number.
3.  **Receiving Frames (Receiver Side):**
    *   The receiver maintains a window of acceptable sequence numbers.
    *   If Frame `i` is received correctly and its sequence number falls within the receiver's window:
        *   The receiver sends an ACK for Frame `i` (ACK `i`).
        *   If Frame `i` is the one at the start of its window (i.e., the next expected in-sequence frame), it's passed to the network layer. The window then slides forward past this frame and any other contiguous, already acknowledged frames that were buffered.
        *   If Frame `i` is received correctly but is out of order (i.e., an earlier frame is still missing), it is buffered until all preceding frames arrive. It is still ACKed upon receipt.
    *   If a corrupted frame is received, it is typically discarded. The sender will eventually time out and retransmit it.
4.  **Receiving ACKs (Sender Side):**
    *   When the sender receives an ACK `i`, it marks Frame `i` as successfully received.
    *   If ACK `i` is for the frame at the start of the sender's window, the sender's window can slide forward past this acknowledged frame.
    *   The sender maintains a separate timer for each unacknowledged frame it has sent.
5.  **Timeout and Retransmission (Selective Repeat Behavior):**
    *   If the timer for a specific Frame `i` expires, the sender assumes only Frame `i` was lost or its ACK was lost.
    *   The sender **retransmits only Frame `i`**. It does not retransmit other frames that were sent after Frame `i` (unlike Go-Back-N).

---

## Diagrams of Scenarios

Assume sender window Ws = 3, receiver window Wr = 3, sequence numbers 0, 1, 2, 3, 4, 5...

**Scenario 1: Successful Transmission**
```
Sender                      Receiver
Win: [0,1,2]                Win: [0,1,2]
  |--(F0)-------------------->| Receives F0, Send ACK0, Deliver F0
  |--(F1)-------------------->|<-------(ACK0)----------------| Win slides to [1,2,3]
  |--(F2)-------------------->| Receives F1, Send ACK1, Deliver F1
                              |<-------(ACK1)----------------| Win slides to [2,3,4]
Sender rcvs ACK0, Win: [1,2,3]
                              | Receives F2, Send ACK2, Deliver F2
Sender rcvs ACK1, Win: [2,3,4]|<-------(ACK2)----------------| Win slides to [3,4,5]
Sender rcvs ACK2, Win: [3,4,5]
  |--(F3)-------------------->|
  ...
```

**Scenario 2: Frame Loss (e.g., Frame 1 is Lost)**
```
Sender                      Receiver
Win: [0,1,2]                Win: [0,1,2]
  |--(F0)-------------------->| Rcvs F0, ACK0, Deliver F0. Win: [1,2,3]
  |--(F1)---------X--------->|<-------(ACK0)----------------| (F1 Lost)
  |--(F2)-------------------->|
Sender rcvs ACK0, Win: [1,2,3]
                              | Rcvs F2 (within Win [1,2,3] but F1 missing), Buffer F2, Send ACK2.
                              |<-------(ACK2)----------------|
Sender rcvs ACK2, marks F2 OK.
Timer for F1 expires at Sender.
  Sender SELECTIVELY RETRANSMITS F1:
  |--(F1)'------------------->|
                              | Rcvs F1', Send ACK1. Now F1 & F2 are present.
                              | Deliver F1, then F2. Win slides to [3,4,5].
                              |<-------(ACK1)----------------|
  ...
```

**Scenario 3: Lost Acknowledgement (e.g., ACK1 for Frame 1 is Lost)**
```
Sender                      Receiver
Win: [0,1,2]                Win: [0,1,2]
  |--(F0)-------------------->| Rcvs F0, ACK0, Deliver F0. Win: [1,2,3]
  |--(F1)-------------------->|<-------(ACK0)----------------|
  |--(F2)-------------------->| Rcvs F1, ACK1. Deliver F1. Win: [2,3,4]
Sender rcvs ACK0, Win: [1,2,3]|          ACK1 ----X           (ACK1 for F1 Lost)
                              | Rcvs F2, ACK2. Deliver F2. Win: [3,4,5]
                              |<-------(ACK2)----------------|
Sender rcvs ACK2, marks F2 OK. (F1 still unACKed at sender)
Timer for F1 expires at Sender.
  Sender SELECTIVELY RETRANSMITS F1:
  |--(F1)'------------------->|
                              | Rcvs F1' (duplicate of already processed F1).
                              | Discards F1', Resends ACK1.
                              |<-------(ACK1)----------------|
Sender rcvs ACK1 for F1', marks F1 OK. Win slides.
```

---

## Window Sizes and Sequence Numbers

*   For Selective Repeat, the sum of sender and receiver window sizes must be less than or equal to the total number of available sequence numbers (`2^m`). That is, `Ws + Wr <= 2^m`.
*   A common choice is `Ws = Wr = 2^(m-1)`. For example, if `m=3` bits are used for sequence numbers (0-7), then `Ws = Wr = 2^(3-1) = 2^2 = 4`.
    *   This ensures that there's no ambiguity if an old ACK arrives after the sequence numbers have wrapped around.

## Buffering

*   **Sender:** Must buffer all transmitted but unacknowledged frames (up to `Ws` frames) for potential selective retransmission.
*   **Receiver:** Must be able to buffer out-of-order frames that are correctly received but cannot yet be delivered to the network layer (up to `Wr` frames typically, or `Wr-1` if one space is for the next in-sequence).

---

## Advantages and Disadvantages

**Advantages:**
*   **Most Efficient ARQ:** Generally the most efficient ARQ protocol in terms of channel utilization, especially on noisy links with high error rates, because it minimizes retransmissions (only lost/corrupted frames are resent).
*   **Higher Throughput:** Achieves better throughput than Go-Back-N when errors occur frequently.

**Disadvantages:**
*   **More Complex Receiver Logic:** The receiver needs to manage buffers for out-of-order frames, track individual ACKs, and reorder frames before delivering them to the network layer.
*   **More Complex Sender Logic:** Sender needs to manage individual timers for each unacknowledged frame.
*   **Buffering Requirements:** Both sender and receiver require more buffer space compared to Go-Back-N (receiver) or Stop-and-Wait.

---

## Efficiency of Selective Repeat

The efficiency of Selective Repeat is high. If the window size `N` (Ws) is large enough to keep the pipe full (`N >= 1 + 2a`), the efficiency is approximately `1 - P`, where `P` is the probability of a frame being lost or in error (assuming only lost frames are retransmitted and ACKs are not lost). Each frame is transmitted on average `1/(1-P)` times.

Essentially, only erroneous or lost frames are retransmitted, making it much more robust to errors than Go-Back-N.

---

## Comparison: Go-Back-N vs. Selective Repeat

| Feature             | Go-Back-N ARQ                                  | Selective Repeat ARQ                                |
|---------------------|------------------------------------------------|-----------------------------------------------------|
| **Retransmission**  | Retransmits lost/errored frame and all subsequent frames in the window | Retransmits only the specific lost/errored frame(s) |
| **Receiver Window** | Wr = 1 (no out-of-order buffering)           | Wr > 1 (buffers out-of-order frames)              |
| **ACKs**            | Cumulative ACKs                                | Individual ACKs (typically)                         |
| **Receiver Complexity** | Simpler                                        | More complex (buffering, reordering)              |
| **Sender Timers**   | One timer for oldest unACKed frame (typically)| Individual timer for each unACKed frame           |
| **Efficiency (Errors)** | Lower when errors are frequent                 | Higher when errors are frequent                     |
| **Buffering (Rx)**  | Minimal (for one frame)                        | Larger (for Wr frames)                            |

---

## Exam Angle Summary (Based on `pyqs.md` and `plan.md`)

When preparing for exams on Selective Repeat ARQ:

*   **Working Principle:** "Explain the working principle of ... Selective Repeat protocols" (Dec 2024). Detail sender/receiver windows, individual ACKs, out-of-order buffering at the receiver, and selective retransmission upon timeout.
*   **Diagram Scenarios:** "With the help of suitable diagrams explain: ...Selective Repeat" (May 2024). Practice drawing timelines for successful transmission, frame loss (showing buffering and later delivery), and ACK loss.
*   **Comparison:** Be ready to "Compare Go-Back-N and Selective Repeat Protocols" (May 2019, May 2018 CBGS). This is a very common question. Focus on differences in retransmission strategy, receiver complexity, buffering, and efficiency under different error conditions.
*   **Key Features:** Emphasize `Ws > 1`, `Wr > 1`, individual ACKs, receiver buffering of out-of-order frames, and retransmission of only specific missing/errored frames.
*   **Window Size Relationship:** Understand the `Ws + Wr <= 2^m` or `Ws = Wr = 2^(m-1)` concept.

Selective Repeat's strength is its efficiency in handling errors, which comes at the cost of increased complexity, especially at the receiver. 