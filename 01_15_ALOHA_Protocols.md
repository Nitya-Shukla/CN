# Detailed Answer: ALOHA Protocols

ALOHA is a family of pioneering random access MAC (Media Access Control) protocols developed at the University of Hawaii in the early 1970s for communication over a shared radio broadcast channel. These protocols are historically significant and form the conceptual basis for later contention-based access methods like CSMA. The main idea is that stations transmit whenever they have data, leading to potential collisions, which are then handled.

There are two main versions: Pure ALOHA and Slotted ALOHA.

---

## Pure ALOHA

**Principle:**
*   In Pure ALOHA, stations can transmit a frame whenever they have data ready to send, without checking if the channel is busy or coordinating with other stations.
*   If a station transmits a frame, it then waits for an acknowledgement (ACK) from the receiver.
*   If an ACK is not received within a timeout period, the station assumes its frame collided with a transmission from another station (or was lost/corrupted) and retransmits the frame after waiting for a random backoff period.
*   The random backoff is crucial to prevent repeated collisions between the same stations.

**Vulnerable Period:**
*   A collision occurs if any part of one frame overlaps with any part of another frame on the shared channel.
*   Consider a frame of transmission time `Tt` (Frame Time). If station A starts transmitting Frame A at time `t0` and it finishes at `t0 + Tt`.
*   A collision will occur if another station B starts transmitting:
    1.  Anytime between `t0 - Tt` and `t0` (Station B's frame will overlap with the beginning of Frame A).
    2.  Anytime between `t0` and `t0 + Tt` (Station B's frame will overlap with Frame A while it's being sent).
*   Therefore, the **vulnerable period** for a frame in Pure ALOHA is **`2 * Tt`**. A frame is vulnerable to collision if another transmission begins anytime from `Tt` before its start until `Tt` after its start (i.e., during its own transmission time `Tt` and the preceding `Tt`).

*   **Diagram of Vulnerable Period for Pure ALOHA:**
    ```
    <------------------- Vulnerable Period = 2 * Tt ------------------->
    ______________________                               ______________________
   |                      |                             |                      |
   | Previous Frame Time  |      Target Frame Time      |  Next Frame Time     |
   |      (Tt)            |           (Tt)            |       (Tt)           |
   |______________________|_____________________________|______________________|
   t0-Tt                  t0                          t0+Tt                  t0+2Tt

    If station X starts transmitting its frame at t0...
    A collision occurs if another station Y starts transmitting:
    - anytime in the interval [t0 - Tt, t0] (Y's frame overlaps with start of X's frame)
    - anytime in the interval (t0, t0 + Tt] (Y's frame overlaps with end of X's frame)
    ```

**Maximum Throughput (Smax):**
*   Throughput (S) is the average number of successful frame transmissions per frame time.
*   It can be shown that `S = G * e^(-2G)`, where `G` is the offered load (average number of frames attempted per frame time, including originals and retransmissions).
*   The maximum throughput `Smax` occurs when `G = 0.5`.
*   `Smax = 0.5 * e^(-2*0.5) = 0.5 * e^(-1) ≈ 0.5 * 0.36788 ≈ 0.1839`
*   So, the maximum theoretical throughput of Pure ALOHA is approximately **18.4%**. This means at best, only about 18.4% of the channel's capacity can be utilized for successful transmissions; the rest is lost to collisions or idle time.

**Advantages:**
*   Very simple to implement: stations transmit without coordination.

**Disadvantages:**
*   Low maximum throughput due to the high probability of collisions.
*   Channel can become unstable under high load (more collisions lead to more retransmissions, leading to even more collisions).

---

## Slotted ALOHA

**Principle:**
*   Slotted ALOHA was developed to improve the throughput of Pure ALOHA by reducing the vulnerable period.
*   Time is divided into discrete intervals called **slots**. The duration of each slot is equal to the frame transmission time `Tt`.
*   Stations are synchronized and are only allowed to start transmitting a frame at the **beginning of a time slot**.
*   If a station has data, it waits for the beginning of the next slot to transmit.
*   If two or more stations transmit in the same slot, a collision occurs for that entire slot.
*   Like Pure ALOHA, if an ACK is not received after a timeout, the station assumes a collision and retransmits after a random backoff period (again, choosing a future slot).

**Vulnerable Period:**
*   In Slotted ALOHA, a collision can only occur if two or more stations decide to transmit in the *exact same slot*.
*   If a station decides to transmit in slot `k`, it will only collide with other stations that also decide to transmit in slot `k`.
*   Transmissions starting in slot `k-1` will finish before slot `k` begins. Transmissions starting in slot `k+1` will begin after slot `k` ends.
*   Therefore, the **vulnerable period** for a frame in Slotted ALOHA is **`Tt`** (the duration of one time slot).

*   **Diagram of Vulnerable Period for Slotted ALOHA:**
    ```
    <---- Slot k-1 ----><---- Slot k (Vulnerable Period = Tt) ----><---- Slot k+1 ---->
    |                  |  Collision if >1 station starts here  |                  |
    -------------------|---------------------------------------|-------------------
    Start of Slot k-1  Start of Slot k                       Start of Slot k+1

    If station X decides to transmit in Slot k, a collision occurs only if
    another station Y also decides to transmit in the *same* Slot k.
    ```

**Maximum Throughput (Smax):**
*   The throughput `S = G * e^(-G)`, where `G` is the offered load (average number of frames attempted per slot).
*   The maximum throughput `Smax` occurs when `G = 1`.
*   `Smax = 1 * e^(-1) ≈ 0.36788`
*   So, the maximum theoretical throughput of Slotted ALOHA is approximately **36.8%**. This is double the maximum throughput of Pure ALOHA, achieved by halving the vulnerable period.

**Advantages:**
*   Higher maximum throughput (double that of Pure ALOHA).
*   Still relatively simple compared to more complex MAC protocols.

**Disadvantages:**
*   Requires synchronization among all stations to agree on slot boundaries, which adds some complexity.
*   Throughput still limited and can degrade under high load, though better than Pure ALOHA.

---

## Comparison: Pure ALOHA vs. Slotted ALOHA

| Feature              | Pure ALOHA                                  | Slotted ALOHA                                     |
|----------------------|---------------------------------------------|---------------------------------------------------|
| **Transmission Rule**| Transmit anytime data is ready              | Transmit only at the beginning of a time slot   |
| **Synchronization**  | Not required                                | Required (for slot boundaries)                    |
| **Vulnerable Period**| `2 * Tt` (Tt = Frame transmission time)   | `Tt`                                              |
| **Max. Throughput**  | Approx. 18.4% (`1 / (2e)`)                  | Approx. 36.8% (`1 / e`)                           |
| **Complexity**       | Simpler                                     | Slightly more complex due to synchronization    |
| **Probability of Collision** | Higher                                      | Lower (halved compared to Pure ALOHA)             |

---

## Numerical Problems (Based on `plan.md` and `pyqs.md`)

ALOHA protocols are common sources of numerical problems, often asking to calculate:
*   Maximum throughput (given it's Pure or Slotted ALOHA).
*   Actual throughput `S` given the offered load `G` (using `S = G * e^(-2G)` for Pure, `S = G * e^(-G)` for Slotted).
*   Probability of successful transmission.
*   Number of stations that can be supported or number of transmission attempts.

**Example (May 2023 Q4b - typical structure):**
"A pure ALOHA network transmits 200-bit frames on a shared channel of 200 kbps. What is the throughput if the system (all stations together) produces:
(i) 1000 frames per second
(ii) 500 frames per second
(iii) 250 frames per second."

*   **Steps to Solve Such Problems:**
    1.  **Calculate Frame Transmission Time (Tt):**
        `Tt = Frame Size (bits) / Data Rate (bps)`
        For the example: `Tt = 200 bits / 200,000 bps = 0.001 seconds = 1 ms`.
    2.  **Calculate Offered Load (G):**
        `G = Number of frames produced per second * Tt`
        This `G` is the average number of frames attempted *per frame transmission time*.
        (i) `G = 1000 frames/sec * 0.001 sec/frame = 1`
        (ii) `G = 500 frames/sec * 0.001 sec/frame = 0.5`
        (iii) `G = 250 frames/sec * 0.001 sec/frame = 0.25`
    3.  **Calculate Throughput (S) using the appropriate ALOHA formula:**
        For Pure ALOHA: `S = G * e^(-2G)`
        (i) `S = 1 * e^(-2*1) = e^(-2) ≈ 0.1353`. Throughput in frames/sec = `S / Tt = 0.1353 / 0.001 = 135.3 frames/sec`.
        (ii) `S = 0.5 * e^(-2*0.5) = 0.5 * e^(-1) ≈ 0.5 * 0.36788 ≈ 0.1839` (This is Smax). Throughput = `0.1839 / 0.001 = 183.9 frames/sec`.
        (iii) `S = 0.25 * e^(-2*0.25) = 0.25 * e^(-0.5) ≈ 0.25 * 0.6065 ≈ 0.1516`. Throughput = `0.1516 / 0.001 = 151.6 frames/sec`.

**Key for numericals:**
*   Distinguish between Pure and Slotted ALOHA formulas.
*   Correctly calculate `Tt`.
*   Correctly calculate `G` (load per frame time or per slot time).
*   The throughput `S` calculated from the formula is a dimensionless value (successful frames per frame time/slot). To get frames per second, you often multiply `S` by `1/Tt` (or `S` * (number of slots per second)). Or, if `S` is calculated as `S = (frames/sec) * Tt * e^(-2G_frames_sec*Tt)`, then `S` is already in frames/sec.
    *Alternative `G` definition*: If `G` is defined as frames attempted per second directly, then `S_fps = G_fps * e^(-2 * G_fps * Tt)` for Pure ALOHA. Be consistent with units.
    The `plan.md` indicates practice for: May 2023 Q4b, May 2022 Q2b, May 2018 CBGS Q5b.

---

## Exam Angle Summary

*   **Working Principle:** Explain how Pure ALOHA and Slotted ALOHA operate, focusing on when stations transmit and how collisions are handled (timeout, random backoff).
*   **Vulnerable Period:** Crucial for both. Be able to define it, explain why it's `2*Tt` for Pure and `Tt` for Slotted, and preferably draw simple diagrams.
*   **Maximum Throughput:** Know the values (18.4% for Pure, 36.8% for Slotted) and that they occur at `G=0.5` and `G=1` respectively.
*   **Comparison:** Be ready to compare Pure and Slotted ALOHA (throughput, vulnerable period, synchronization requirement, complexity).
*   **Numerical Problems:** This is a high-probability area for numericals. Practice calculating throughput given offered load, or vice-versa, for both types.
*   **Question Types:** "Explain the working of slotted ALOHA. State how it is an improvement over pure ALOHA" (May 2019). "Explain Pure and Slotted ALOHA protocols" (Dec 2024). "Write a short note on ALOHA" (June 2020).

Understanding the trade-off between simplicity (Pure ALOHA) and improved performance via synchronization (Slotted ALOHA) is key. 