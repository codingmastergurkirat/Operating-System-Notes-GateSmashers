# Disk Architecture (Magnetic Hard Disk)

## 1. Overview

Disk architecture describes how a magnetic hard disk is organized to store and retrieve data. The topic is relevant to Operating Systems and Computer Organization, especially for questions involving disk capacity and storage structure.

A traditional hard disk drive (HDD) contains rotating platters and read/write heads. The operating system requests data in logical units, while the drive's physical organization is commonly described using platters, surfaces, tracks, and sectors.

## 2. Main Components

### Platters
- Platters are circular disks coated with magnetic material on which data is stored.
- A hard disk can contain multiple platters.
- The platters are mounted on a shared spindle and rotate together.

### Spindle
- The spindle supports and rotates the platters.
- The platters rotate together in the same direction at a given time.

### Surfaces
- A platter normally has two physical surfaces: an upper surface and a lower surface.
- Each surface may be used to store data, although a question may specify that only certain surfaces are active.
- In basic numerical problems, assume both surfaces are used unless the question states otherwise.

### Read/write heads and actuator arm
- A read/write head accesses data on a platter surface.
- The actuator arm moves the heads radially across the platter, from outer tracks toward inner tracks and back.
- In a conventional HDD, each recording surface has a corresponding head. The heads move together with the actuator mechanism.

## 3. Tracks, Sectors, and Data

### Tracks
Tracks are concentric circular paths on a platter surface. Each recording surface contains tracks.

### Sectors
A track is divided into sectors. In introductory capacity problems, the number of sectors per track is often given as a fixed value. Modern disks may use more complex layouts, but the fixed-value assumption is suitable for the numerical examples in this lesson.

### Data
Sectors store data. The amount of data per sector is specified in the problem.

### Structure to remember

**Platters → Surfaces → Tracks → Sectors → Data**

This is a useful hierarchy for introductory disk-capacity calculations.

## 4. Disk-Structure Counting Formulas

Let:
- \(P\) = number of platters
- \(S\) = number of recording surfaces per platter
- \(T\) = number of tracks per surface
- \(Q\) = number of sectors per track
- \(B\) = number of bytes per sector

Then:

\[
\text{Total surfaces} = P \times S
\]

\[
\text{Total tracks} = P \times S \times T
\]

\[
\text{Total sectors} = P \times S \times T \times Q
\]

\[
\text{Disk capacity in bytes} = P \times S \times T \times Q \times B
\]

For the common introductory assumption that both surfaces of every platter are used, \(S=2\). If only one surface per platter is used, set \(S=1\).

## 5. Worked Example: Total Disk Capacity

Suppose a disk has:
- 8 platters
- 2 used surfaces per platter
- 256 tracks per surface
- 512 sectors per track
- 512 KB of data per sector, as stated in the lecture example

### Step 1: Total surfaces

\[
8 \times 2 = 16 \text{ surfaces}
\]

### Step 2: Total tracks

\[
8 \times 2 \times 256 = 4096 \text{ tracks}
\]

### Step 3: Total sectors

\[
8 \times 2 \times 256 \times 512
= 2^3 \times 2^1 \times 2^8 \times 2^9
= 2^{21} \text{ sectors}
\]

### Step 4: Capacity in bytes

Using the lecture's binary-unit convention:
- \(512 = 2^9\)
- \(1\text{ KB} = 2^{10}\) bytes
- Therefore, \(512\text{ KB} = 2^9 \times 2^{10} = 2^{19}\) bytes

So:

\[
\text{Capacity} = 2^{21} \times 2^{19} = 2^{40} \text{ bytes}
\]

\[
2^{40}\text{ bytes} = 1\text{ TiB}
\]

The lecture calls this **1 TB**. Strictly, \(2^{40}\) bytes is 1 tebibyte (TiB); decimal 1 terabyte (TB) is \(10^{12}\) bytes. In many introductory exam questions, the binary convention is used informally for TB.

**Important:** The assumed sector size of 512 KB is unusually large for a conventional disk sector, but it is retained here because it is the value used in the lecture's worked example. If the question gives a different sector size, use that value instead.

## 6. Powers of Two and Storage Units

Useful powers of two:
- \(2^8 = 256\)
- \(2^9 = 512\)
- \(2^{10} = 1024\)

Under the binary convention used in many introductory problems:
- \(1\text{ KB} = 2^{10}\) bytes
- \(1\text{ MB} = 2^{20}\) bytes
- \(1\text{ GB} = 2^{30}\) bytes
- \(1\text{ TB} = 2^{40}\) bytes (more precisely, 1 TiB)

There are 8 bits in one byte:

\[
1\text{ byte} = 8\text{ bits} = 2^3\text{ bits}
\]

To convert a capacity from bytes to bits, multiply by 8. To convert from bits to bytes, divide by 8.

## 7. Number of Bits Required to Represent a Capacity

The lecture's example obtains a capacity of \(2^{40}\) bytes and then asks how many bits are needed to represent the number of possible byte positions/units.

There are \(2^{40}\) distinct values or positions if counting from 0 through \(2^{40}-1\). The number of bits needed to represent that range is:

\[
\log_2(2^{40}) = 40 \text{ bits}
\]

In general, to represent \(N\) distinct values, the minimum number of bits is:

\[
\lceil \log_2 N \rceil
\]

Be careful about the exact wording of a question:
- **Bits needed to encode \(N\) distinct possibilities:** \(\lceil \log_2 N \rceil\).
- **Bits in a storage capacity of \(N\) bytes:** \(8N\) bits.

These are different questions. For example, \(2^{40}\) bytes contains \(8 \times 2^{40}\) bits of storage, while a binary address needs 40 bits to identify one of \(2^{40}\) byte positions.

## 8. Quick Exam Reference

| If asked for… | Calculate |
|---|---|
| Total surfaces | Platters × surfaces per platter |
| Total tracks | Platters × surfaces per platter × tracks per surface |
| Total sectors | Platters × surfaces per platter × tracks per surface × sectors per track |
| Total capacity | Total sectors × bytes per sector |
| Bits to identify one of \(2^n\) locations | \(n\) bits |
| Capacity in bits | Capacity in bytes × 8 |

## 9. Key Points

1. Remember the hierarchy: **platters → surfaces → tracks → sectors → data**.
2. A platter typically has two surfaces, but not every surface must necessarily be used.
3. Heads move radially to select tracks; the platters rotate to bring sectors under the heads.
4. In the lecture's example, the capacity is \(2^{40}\) bytes under the stated assumptions.
5. Read whether the question asks for a number of tracks, sectors, bytes, bits, or address bits before choosing the formula.
