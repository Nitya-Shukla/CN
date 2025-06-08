# Detailed Answer: Stop-and-Wait ARQ

Stop-and-Wait ARQ (Automatic Repeat reQuest) is the simplest ARQ protocol used for reliable data transmission over a noisy channel. It builds upon the basic Stop-and-Wait flow control mechanism by adding error detection and retransmission capabilities.

---

## Core Concepts

*   **ARQ:** Automatic Repeat reQuest protocols aim to ensure reliable delivery by retransmitting frames that are lost or corrupted.
*   **Stop-and-Wait:** The sender transmits one frame and then waits for an acknowledgement (ACK) from the receiver before sending the next frame.

---

## Working Principle of Stop-and-Wait ARQ

Stop-and-Wait ARQ incorporates the following elements:

1.  **Error Detection:** The receiver uses an error detection mechanism (e.g., CRC, checksum) to check if the received frame is corrupted.
2.  **Acknowledgements (ACKs):** The receiver sends a positive acknowledgement (ACK) if the frame is received correctly and is the expected one.
3.  **Timeouts:** The sender starts a timer when it sends a frame. If an ACK is not received before the timer expires, the sender assumes the frame or its ACK was lost and retransmits the frame.
4.  **Sequence Numbers:** To handle duplicate frames (which can occur if an ACK is lost and the sender retransmits a frame that was already successfully received), frames and ACKs are typically numbered. In basic Stop-and-Wait ARQ, a 1-bit sequence number (0 and 1) is sufficient for frames and ACKs.
    *   Sender sends Frame 0.
    *   Receiver expects Frame 0. If received correctly, sends ACK 0 (or ACK 1, acknowledging Frame 0 and expecting Frame 1, depending on convention).
    *   Sender sends Frame 1.
    *   Receiver expects Frame 1. If received correctly, sends ACK 1 (or ACK 0, acknowledging Frame 1 and expecting Frame 0).

**Operational Scenarios:**

1.  **Normal Operation (Frame Transmitted Successfully):**
    *   Sender sends Frame N.
    *   Receiver receives Frame N correctly, sends ACK N (or ACK for next expected frame).
    *   Sender receives ACK, sends Frame N+1 (modulo 2 for 1-bit sequence number).

2.  **Lost Frame:**
    *   Sender sends Frame N.
    *   Frame N is lost in transit.
    *   Receiver receives nothing, sends nothing.
    *   Sender's timer for Frame N expires.
    *   Sender retransmits Frame N.

3.  **Lost Acknowledgement (ACK):**
    *   Sender sends Frame N.
    *   Receiver receives Frame N correctly, sends ACK N.
    *   ACK N is lost in transit.
    *   Sender's timer for Frame N expires.
    *   Sender retransmits Frame N.
    *   Receiver receives Frame N again. Since it already received Frame N (it knows this from the sequence number), it discards the duplicate and resends ACK N.

4.  **Corrupted Frame:**
    *   Sender sends Frame N.
    *   Frame N arrives at receiver but is corrupted (error detected).
    *   Receiver discards the corrupted Frame N. (Some implementations might send a NAK - Negative Acknowledgement, but often the timeout mechanism handles this).
    *   Sender's timer for Frame N expires.
    *   Sender retransmits Frame N.

5.  **Delayed ACK (Premature Timeout):**
    *   Sender sends Frame N.
    *   Receiver receives Frame N, sends ACK N.
    *   ACK N is delayed but not lost.
    *   Sender's timer for Frame N expires before delayed ACK arrives.
    *   Sender retransmits Frame N.
    *   Receiver receives duplicate Frame N, discards it, resends ACK N.
    *   Sender eventually receives the first (delayed) ACK N, then possibly the second ACK N for the retransmitted frame. It processes the first valid ACK and moves on.

---

## Diagrams of Scenarios

**Scenario 1: Successful Transmission**
```
Sender                      Receiver
  |--(Seq 0) Frame 0--------->| 
  | Start Timer               | Process Frame 0
  |                           | Send ACK 0 (or ACK 1 for next)
  |<--------ACK 0 (or 1)-----| 
  | Stop Timer                |
  |--(Seq 1) Frame 1--------->| 
  ...                         ...
```

**Scenario 2: Lost Frame**
```
Sender                      Receiver
  |--(Seq 0) Frame 0----X     | (Frame Lost)
  | Start Timer               |
  |        (Timeout)          |
  | Retransmit Frame 0 ------>| 
  | Restart Timer             | Process Frame 0
  |                           | Send ACK 0
  |<--------ACK 0------------|
  | Stop Timer                |
```

**Scenario 3: Lost Acknowledgement (ACK)**
```
Sender                      Receiver
  |--(Seq 0) Frame 0--------->| 
  | Start Timer               | Process Frame 0
  |                           | Send ACK 0
  |          ACK 0 ----X      | (ACK Lost)
  |        (Timeout)          |
  | Retransmit Frame 0 ------>| 
  | Restart Timer             | (Duplicate Frame 0, discard)
  |                           | Resend ACK 0
  |<--------ACK 0------------|
  | Stop Timer                |
```

**Scenario 4: Corrupted Frame (Receiver Discards)**
```
Sender                      Receiver
  |--(Seq 0) Frame 0--\ /---->| (Frame Corrupted, discard)
  | Start Timer        X      |
  |                           |
  |        (Timeout)          |
  | Retransmit Frame 0 ------>|
  | Restart Timer             | Process Frame 0
  |                           | Send ACK 0
  |<--------ACK 0------------|
  | Stop Timer                |
```

---

## Efficiency of Stop-and-Wait ARQ

The efficiency of Stop-and-Wait ARQ (also called utilization or throughput) is generally low, especially on links with a long propagation delay or high data rates.

*   **Transmission Time (Tt):** Time taken to transmit one frame. `Tt = Frame Size / Data Rate`.
*   **Propagation Delay (Tp):** Time taken for the first bit of a frame to travel from sender to receiver, or for an ACK to travel from receiver to sender.

In the ideal case (no errors), the sender transmits a frame (Tt), waits for the frame to reach the receiver (Tp), waits for the receiver to process and send an ACK (negligible processing time often assumed), and waits for the ACK to return (Tp). So, the total time for one frame cycle is approximately `Tt + 2*Tp`.

*   **Efficiency (U) = Useful Time / Total Cycle Time = Tt / (Tt + 2*Tp)**

This can be rewritten using the parameter `a = Tp / Tt`:
*   **Efficiency (U) = 1 / (1 + 2a)**

**Analysis:**
*   If `a` is small (propagation delay is much smaller than transmission time, e.g., short, slow link), efficiency is high.
*   If `a` is large (propagation delay is much larger than transmission time, e.g., satellite link or very high-speed short link), `2a` dominates, and efficiency `1/(2a)` becomes very low. The sender spends most of its time waiting.

**Example:**
If Tt = 1 ms and Tp = 20 ms:
`a = 20ms / 1ms = 20`
`U = 1 / (1 + 2*20) = 1 / 41 ≈ 0.024` or `2.4%` (very inefficient).

---

## Recall Cue

*   **"Send 1, Wait 1 (with timer and sequence numbers)."** This emphasizes the single frame transmission, the waiting period, and the additions for reliability (timer for loss, sequence numbers for duplicates).

---

## Advantages and Disadvantages

**Advantages:**
*   **Simplicity:** It is the simplest ARQ protocol to implement.
*   **Guaranteed Delivery:** Ensures that each frame is correctly received at least once (assuming a finite number of errors).

**Disadvantages:**
*   **Inefficient:** Very low channel utilization and throughput, especially for links with a high bandwidth-delay product (where `a` is large).
*   **Slow:** Only one frame can be in transit at a time.

---

## Exam Angle Summary (Based on `pyqs.md` and `plan.md`)

When preparing for exams on Stop-and-Wait ARQ:

*   **Working Principle:** Be able to explain how it handles successful transmission, lost frames, and lost ACKs. Understanding the role of sequence numbers (even if just 0 and 1) and timers is crucial.
*   **Diagrams:** "With the help of suitable diagrams explain: Stop and wait..." (May 2024). Practice drawing the timeline diagrams for different scenarios (successful, lost frame, lost ACK).
*   **Description:** "Describe the stop and wait flow control technique" (May 2022). While this question points to flow control, Stop-and-Wait ARQ is its reliable counterpart and often discussed in this context.
*   **Efficiency:** Understand the concept of its efficiency (`U = 1 / (1 + 2a)`), why it's inefficient, and be able to explain the terms Tt and Tp.
*   **Comparison:** Be ready to "Compare the performance of stop and wait protocol and sliding window protocol" (Dec 2020). This involves contrasting its inefficiency with the better performance of sliding window protocols.

Focus on the mechanism, the handling of errors and losses through timers and sequence numbers, its inherent simplicity, and its significant drawback in terms of efficiency. 