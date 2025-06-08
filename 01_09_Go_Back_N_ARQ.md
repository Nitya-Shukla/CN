# Detailed Answer: Go-Back-N ARQ

Go-Back-N ARQ (Automatic Repeat reQuest) is a sliding window protocol used for reliable data transmission. It improves upon the inefficiency of Stop-and-Wait ARQ by allowing the sender to transmit multiple frames (a "window" of frames) before waiting for an acknowledgement. However, it has a specific way of handling errors: if a frame is lost or corrupted, the sender "goes back" and retransmits that frame and all subsequent frames that were sent after it, even if some of those subsequent frames were received correctly by the receiver.

---

## Core Concepts

*   **Sliding Window:** The sender maintains a window of sequence numbers it is allowed to transmit. As frames are acknowledged, this window slides forward.
*   **Pipelining:** Multiple frames can be in transit simultaneously, improving channel utilization compared to Stop-and-Wait.
*   **Cumulative Acknowledgements:** The receiver typically sends an acknowledgement (ACK N) to indicate that all frames up to sequence number N-1 have been received correctly and it is now expecting frame N.
*   **Receiver Window Size:** In Go-Back-N, the receiver window size (Wr) is always 1. This means the receiver only accepts frames in strict sequential order. It discards any out-of-order frames.
*   **Sender Window Size (Ws):** Can be up to `2^m - 1`, where `m` is the number of bits used for sequence numbers. (A common maximum is `2^m - 1` to avoid ambiguity with sequence number wrap-around).

---

## Working Principle of Go-Back-N ARQ

1.  **Initialization:** Both sender and receiver have a window. Sender window size `Ws > 1`, Receiver window size `Wr = 1`.
2.  **Sending Frames:** The sender can send up to `Ws` frames without receiving an ACK. Each frame has a sequence number.
3.  **Receiving Frames (Receiver Side):**
    *   The receiver expects frames in order. If Frame `i` is received correctly and `i` is the expected sequence number, the receiver accepts it, passes its data to the network layer, and updates its expected sequence number to `i+1`.
    *   It then sends a cumulative ACK (e.g., ACK `i+1`) indicating it has received all frames up to `i` and is expecting `i+1`.
    *   If Frame `i` is received but it is corrupted, or if Frame `j` (where `j > i`) is received while Frame `i` is still expected (i.e., out of order), the receiver discards the out-of-order or corrupted frame and resends the last valid ACK it had sent (e.g., ACK `i`). This signals to the sender that it is still waiting for Frame `i`.
4.  **Receiving ACKs (Sender Side):**
    *   When the sender receives an ACK `k`, it means all frames up to sequence number `k-1` have been successfully received. The sender slides its window forward, starting from `k`.
    *   The sender maintains a timer for the oldest unacknowledged frame. If this timer expires, it means that frame or its ACK (or a subsequent ACK that would have cumulatively acknowledged it) was likely lost.
5.  **Timeout and Retransmission (Go-Back-N Behavior):**
    *   If the timer for Frame `i` expires (oldest unacknowledged frame), the sender assumes Frame `i` (or its ACK) is lost.
    *   The sender **goes back** to sequence number `i` and retransmits Frame `i` **and all subsequent frames** that were in its current sending window (i.e., frames `i+1`, `i+2`, ..., up to the last frame sent within the window before the timeout).
    *   This is because the receiver discards any out-of-order frames. So even if frames `i+1`, `i+2` had reached the receiver correctly, they would have been discarded because Frame `i` was missing.

---

## Diagrams of Scenarios

Assume sender window size Ws = 4, sequence numbers 0, 1, 2, 3, 4, 5, 6, 7...

**Scenario 1: Successful Transmission**
```
Sender                      Receiver
  |--(F0)-------------------->| (Expects F0) Receives F0, Send ACK1
  |--(F1)-------------------->|<-------(ACK1)----------------|
  |--(F2)-------------------->| (Expects F1) Receives F1, Send ACK2
  |--(F3)-------------------->|<-------(ACK2)----------------|
  Window: [0,1,2,3]           | (Expects F2) Receives F2, Send ACK3
                              |<-------(ACK3)----------------|
Sender receives ACK1, Window: [1,2,3,4]
Sender receives ACK2, Window: [2,3,4,5]
Sender receives ACK3, Window: [3,4,5,6]
  |--(F4)-------------------->| (Expects F3) Receives F3, Send ACK4
  ...                         |<-------(ACK4)----------------| ...
```
(ACKs can be cumulative and might not be sent for every frame if processing allows)

**Scenario 2: Frame Loss (e.g., Frame 1 is Lost)**
```
Sender                      Receiver
  |--(F0)-------------------->| (Expects F0) Receives F0, Send ACK1
  |--(F1)---------X--------->|<-------(ACK1)----------------| (Frame 1 Lost)
  |--(F2)-------------------->| (Expects F1) Receives F2 (Out of order), DISCARD, Resend ACK1
  |--(F3)-------------------->|<-------(ACK1)----------------|
  Window [0,1,2,3]            | (Expects F1) Receives F3 (Out of order), DISCARD, Resend ACK1
  (Timer for F1 expires)      |<-------(ACK1)----------------|
  Sender GOES BACK TO F1:
  |--(F1)-------------------->| (Expects F1) Receives F1, Send ACK2
  |--(F2)-------------------->|<-------(ACK2)----------------|
  |--(F3)-------------------->| (Expects F2) Receives F2, Send ACK3
  ...                         |<-------(ACK3)----------------| ...
```
Note: The receiver keeps sending ACK1 because it is still waiting for F1.

**Scenario 3: Lost Acknowledgement (e.g., ACK2 is Lost)**
```
Sender                      Receiver
  |--(F0)-------------------->| (Expects F0) Receives F0, Send ACK1
  |--(F1)-------------------->|<-------(ACK1)----------------|
  |--(F2)-------------------->| (Expects F1) Receives F1, Send ACK2
  Window [0,1,2,3]            |          ACK2 ----X           (ACK2 Lost)
  (Timer for F0 expires as ACK1 was received. Timer for F1 is running)
  (Suppose timer for F1 now expires before any further ACK arrives that would cover F1)
  Sender GOES BACK TO F1:
  |--(F1)'------------------->| (Expects F2 or later if ACK2 was piggybacked)
                               | If expects F2, receives F1' (duplicate if F1 was processed) or out of order.
                               | Receiver action depends on its state. If it had processed F1 and F2, it might send ACK3.
  |--(F2)'------------------->|
  ...
```
If a later ACK (e.g., ACK3 from receiver after processing F2) arrives before F1's timer expires, the sender knows F1 and F2 were received, and no retransmission of F1, F2 is needed.

---

## Window Sizes

*   **Sender Window Size (Ws):** To maximize efficiency, `Ws` should be large enough to keep the pipe full. A common upper bound is `2^m - 1`, where `m` is the number of bits for sequence numbers. For example, with 3 bits, sequence numbers are 0-7 (`m=3`), so `Ws <= 2^3 - 1 = 7`.
*   **Receiver Window Size (Wr):** Always 1 in Go-Back-N.

## Buffering

*   **Sender:** Must buffer all transmitted but unacknowledged frames, as they might need to be retransmitted (up to `Ws` frames).
*   **Receiver:** Only needs buffer space for one frame, as it discards any out-of-order frames.

---

## Advantages and Disadvantages

**Advantages:**
*   **More efficient than Stop-and-Wait:** Allows multiple frames to be in transit, improving channel utilization.
*   **Simpler receiver logic compared to Selective Repeat:** Receiver does not need to buffer out-of-order frames or sort them.

**Disadvantages:**
*   **Inefficient in case of errors on high-latency links:** Retransmitting multiple frames (the entire window from the point of error) can be wasteful if only one frame was actually lost or corrupted, especially if the window size is large and error rates are high.
*   **Can lead to many redundant retransmissions:** Correctly received frames after a lost/corrupted frame are discarded by the receiver and then retransmitted by the sender.

---

## Efficiency of Go-Back-N

The efficiency is better than Stop-and-Wait but can degrade with high error rates.
If the window size `N` (Ws) is large enough to keep the pipe full (`N >= 1 + 2a`, where `a = Tp/Tt`), and there are no errors, the efficiency can approach 100%.

However, if there are errors, the efficiency `U` is approximately:
`U = N / (1 + 2a)` when `N < 1 + 2a` (window too small to fill pipe)
`U = 1` when `N >= 1 + 2a` (pipe is full, ideal case without errors)

If `P` is the probability of a frame being in error (or lost), then for every `1/P` frames, there will be a retransmission of `N` frames on average (simplified view).
The actual efficiency formula considering errors is more complex: `U = (N * (1-P)) / ((1+2a) * (1-P + N*P))` or variations thereof depending on assumptions.

The key takeaway is that Go-Back-N is more efficient than Stop-and-Wait but less efficient than Selective Repeat when errors are frequent because it retransmits more data than necessary.

---

## Exam Angle Summary (Based on `pyqs.md` and `plan.md`)

When preparing for exams on Go-Back-N ARQ:

*   **Working Principle:** "Explain the working principle of Go-Back-N... protocols" (Dec 2024). Clearly explain the sender and receiver window sizes, how ACKs work (cumulative), and the "go-back" retransmission strategy upon timeout or error detection.
*   **Diagram Scenarios:** "With the help of suitable diagrams explain: ...Go back N..." (May 2024). Be prepared to draw timelines for successful transmission, frame loss, and possibly ACK loss.
*   **Description:** "Describe Go Back N... protocol" (May 2022).
*   **Comparison:** Be ready to compare it with Stop-and-Wait (highlighting improved efficiency) and Selective Repeat (highlighting GBN's simpler receiver vs. SR's more efficient retransmissions). (`pyqs.md` asks for GBN and SR explanation together, implying comparison).
*   **Key Features:** Emphasize sender window > 1, receiver window = 1, cumulative ACKs, and retransmission of the entire window from the point of error.

Understanding the retransmission strategy and its implications on efficiency and buffering is key for Go-Back-N. 