---
layout: default
title: Primitive Root
use_math: true
nav_title: "2 câu hỏi về thuật toán tìm Primitive Root modulo P"
nav_order: 10
nav_group: "Misc"
---

# 2 câu hỏi về thuật toán tìm Primitive Root modulo P

- [1. Định nghĩa và ký hiệu](#1-định-nghĩa-và-ký-hiệu)
  - [1.1. Bậc của một số nguyên theo modulo $P$](#11-bậc-của-một-số-nguyên-theo-modulo-p)
  - [1.2. Căn nguyên thủy](#12-căn-nguyên-thủy)
  - [1.3. Ký hiệu chung](#13-ký-hiệu-chung)
- [2. Vấn đề đặt ra](#2-vấn-đề-đặt-ra)
- [3. Thuật toán tìm căn nguyên thủy modulo $P$](#3-thuật-toán-tìm-căn-nguyên-thủy-modulo-p)
  - [3.1. Mô tả thuật toán](#31-mô-tả-thuật-toán)
  - [3.2. Mã giả](#32-mã-giả)
- [4. Hai câu hỏi nảy sinh khi phân tích thuật toán](#4-hai-câu-hỏi-nảy-sinh-khi-phân-tích-thuật-toán)
- [5. Các bổ đề](#5-các-bổ-đề)
  - [5.1. Bổ đề 1](#51-bổ-đề-1)
    - [Hệ quả 1](#hệ-quả-1)
  - [5.2. Bổ đề 2](#52-bổ-đề-2)
- [6. Định lý chính](#6-định-lý-chính)
- [7. Trả lời câu hỏi thứ nhất: vì sao chỉ cần xét các $p\_i \\in C$?](#7-trả-lời-câu-hỏi-thứ-nhất-vì-sao-chỉ-cần-xét-các-p_i-in-c)
- [8. Trả lời câu hỏi thứ hai: vì sao là $g^{(P-1)/p\_i}$ chứ không phải $g^{p\_i}$?](#8-trả-lời-câu-hỏi-thứ-hai-vì-sao-là-gp-1p_i-chứ-không-phải-gp_i)
  - [8.1. Test đang thực sự kiểm tra điều gì?](#81-test-đang-thực-sự-kiểm-tra-điều-gì)
  - [8.2. Vì sao ${p\_i}$ không phải là "bẫy" tốt](#82-vì-sao-p_i-không-phải-là-bẫy-tốt)
  - [8.3. Vì sao ${(P-1)/p\_i}$ lại là "bẫy" đúng](#83-vì-sao-p-1p_i-lại-là-bẫy-đúng)
- [9. Brute force đến giá trị $g$ nào thì dừng?](#9-brute-force-đến-giá-trị-g-nào-thì-dừng)
- [10. Một vài fun fact](#10-một-vài-fun-fact)
  - [10.1. Số lượng căn nguyên thủy của một nhóm cyclic](#101-số-lượng-căn-nguyên-thủy-của-một-nhóm-cyclic)
  - [10.2. Không phải modulo nào cũng có căn nguyên thủy](#102-không-phải-modulo-nào-cũng-có-căn-nguyên-thủy)


## 1. Định nghĩa và ký hiệu

### 1.1. Bậc của một số nguyên theo modulo $P$

Cho trước:

- $P$: một số nguyên tố
- $g$: một số nguyên thỏa mãn $\gcd(g, P) = 1$

Theo **Định lý Fermat nhỏ**, vì $P$ nguyên tố và $\gcd(g,P)=1$, ta có:

$$g^{P-1} \equiv 1 \pmod{P}$$

Vậy $P−1$ có phải số mũ nhỏ nhất làm $g^{P-1} \equiv 1$ không? Chưa chắc. Hoàn toàn có thể tồn tại $n < P−1$ cũng thỏa $g^n \equiv 1$

Ví dụ: Với $P=7$ và $g=2$, **Định lý Fermat nhỏ** cho ta biết $2^6 \equiv 1 \pmod 7$, nhưng tất nhiên $2^3 \equiv 1 \pmod 7$ cũng vậy.

Nói cách khác, tập hợp

$$S = \{n \in \mathbb{Z}^+ : g^n \equiv 1 \pmod{P}\}$$

luôn khác rỗng vì $P-1 \in S$. Tuy nhiên, $S$ có thể có nhiều phần tử, và luôn tồn tại ít nhất một phần tử nhỏ nhất. Ta gọi phần tử này là **bậc** của $g$ theo modulo $P$, ký hiệu $\text{ord}(g)$:

$$\text{ord}(g) = \min S$$

tức là số nguyên dương nhỏ nhất sao cho $g^{\text{ord}(g)} \equiv 1 \pmod{P}$.

### 1.2. Căn nguyên thủy

**Định nghĩa:** $g$ được gọi là **căn nguyên thủy** modulo $P$ nếu

$$\text{ord}(g) = P - 1$$

**Tính chất của căn nguyên thủy**: Nếu $g$ là căn nguyên thủy bậc $P-1$ thì $\\{g^0, g^1, \dots, g^{P-2}\\} \bmod P$ chạy qua đúng tất cả $P-1$ giá trị $\\{1, 2, \dots, P-1\\}$, không lặp lại.

**Chứng minh:**

Giả sử tồn tại $0 \le i < j \le P-2$ sao cho

$$g^i \equiv g^j \pmod{P}$$

Vì $\gcd(g, P) = 1$ nên $g$ khả nghịch modulo $P$, ta có thể nhân hai vế với $(g^i)^{-1}$:

$$g^{j-i} \equiv 1 \pmod{P}$$

Đặt $m = j - i$. Do $0 < i < j \le P-2$, ta có

$$0 < m \le P-2 < P - 1 = \text{ord}(g)$$

Điều này mâu thuẫn với định nghĩa $\text{ord}(g)$ là số nguyên dương **nhỏ nhất** thỏa $g^{\text{ord}(g)} \equiv 1 \pmod P$, vì ta vừa tìm được một số mũ dương $m$ nhỏ hơn mà vẫn thỏa $g^m \equiv 1$.

Vậy giả sử sai, tức là $g^0, g^1, \dots, g^{P-2}$ đôi một phân biệt modulo $P$. Ta có đúng $P-1$ giá trị phân biệt.

Có $P-1$ giá trị phân biệt nằm trong một tập đúng $P-1$ phần tử, nên $\\{g^0, g^1, \dots, g^{P-2}\\} \equiv \\{1, 2, \dots, P-1\\} \pmod P$. $\blacksquare$

### 1.3. Ký hiệu chung

Ta định nghĩa $C = \{p_1, p_2, \dots, p_k\}$: tập các ước nguyên tố phân biệt của $P-1$

> Để mọi ví dụ trong bài đều liên kết được với nhau, ta cố định $P = 41$. Khi đó $P - 1 = 40 = 2^3 \cdot 5$, nên tập ước nguyên tố là $C = \\{2, 5\\}$, và hai "điểm kiểm tra" cần tính trong thuật toán là
> $$\frac{P-1}{2} = 20 \qquad \text{và} \qquad \frac{P-1}{5} = 8$$

---

## 2. Vấn đề đặt ra

Khi ta có một số nguyên tố $P$, ta không hề biết số $g$ nào là **căn nguyên thủy** modulo $P$ cả.

Chả hạn, với $P=41$, ta không biết rằng liệu $g=7$ có phải là căn nguyên thủy hay không? Không biết có phải $\\{7^0, 7^1, \dots, 7^{P-2}\\} \bmod P$ chạy qua đúng tất cả $P-1$ giá trị $\\{1, 2, \dots, P-1\\}$ và không lặp lại hay không?

Đó là lý do ta phải đi **tìm căn nguyên thủy modulo $P$**.

---

## 3. Thuật toán tìm căn nguyên thủy modulo $P$

### 3.1. Mô tả thuật toán

Đi thử **brute force** các giá trị $g = 2, 3, 4, \dots, P-1$.

**Thuật toán:** với mỗi $g$, kiểm tra:

> Với mỗi $p_i \in C$:
> - Nếu "điểm kiểm tra" $g^{(P-1)/p_i} \equiv 1 \pmod{P}$ → $g$ **KHÔNG** phải căn nguyên thủy
>
> Nếu không có $p_i \in C$ nào thỏa điều kiện trên → $g$ **là** căn nguyên thủy cần tìm.


### 3.2. Mã giả
---

> **Input:** $P$ — số nguyên tố
> 
> **Output:** $g$ — một căn nguyên thủy của $P$ (tức $\text{ord}(g) = P-1$)
 
---
 
**procedure** $\text{FindPrimitiveRoot}(P)$:
 
&emsp;1.&ensp;$N \leftarrow P - 1$

&emsp;2.&ensp;$C \leftarrow \text{FactorizeDistinctPrimes}(N)$ &emsp;*(tập ước nguyên tố phân biệt của $N$)*
 
&emsp;3.&ensp;**for** $g \leftarrow 2, 3, 4, \dots, P-1$ **do**
 
&emsp;&emsp;4.&ensp;$\textit{isPrimitive} \leftarrow \text{true}$
 
&emsp;&emsp;5.&ensp;**for each** $p_i \in C$ **do**
 
&emsp;&emsp;&emsp;6.&ensp;**if** $g^{\,N/p_i} \bmod P = 1$ **then**
 
&emsp;&emsp;&emsp;&emsp;7.&ensp;$\textit{isPrimitive} \leftarrow \text{false}$
 
&emsp;&emsp;&emsp;&emsp;8.&ensp;**break**
 
&emsp;&emsp;9.&ensp;**if** $\textit{isPrimitive}$ **then**
 
&emsp;&emsp;&emsp;10.&ensp;**return** $g$
 
---
 
**procedure** $\text{FactorizeDistinctPrimes}(N)$:
 
&emsp;1.&ensp;$C \leftarrow \varnothing$
 
&emsp;2.&ensp;$d \leftarrow 2$
 
&emsp;3.&ensp;**while** $d \times d \le N$ **do**
 
&emsp;&emsp;4.&ensp;**if** $N \bmod d = 0$ **then**
 
&emsp;&emsp;&emsp;5.&ensp;$C \leftarrow C \cup \{d\}$
 
&emsp;&emsp;&emsp;6.&ensp;**while** $N \bmod d = 0$ **do** $N \leftarrow N / d$
 
&emsp;&emsp;7.&ensp;$d \leftarrow d + 1$
 
&emsp;8.&ensp;**if** $N > 1$ **then** $C \leftarrow C \cup \{N\}$ &emsp;*(thừa số nguyên tố lớn còn sót lại)*
 
&emsp;9.&ensp;**return** $C$
 
---

## 4. Hai câu hỏi nảy sinh khi phân tích thuật toán

1. Tại sao chỉ cần xét các $p_i \in C$ để kiểm tra $g^{(P-1)/p_i} \bmod P \overset{?}{=} 1$, mà không cần $p_i p_j$, $p_i^2,\dots$ hay các ước khác của $P-1$?
2. Tại sao giá trị cần xét là $g^{(P-1)/p_i}$ mà không phải $g^{p_i} \bmod P$?

Ta sẽ chứng minh toán học để trả lời cả hai.

---

## 5. Các bổ đề

### 5.1. Bổ đề 1

$$g^m \bmod P = 1 \iff \text{ord}(g) \mid m$$

**Chứng minh:** Đặt $d = \text{ord}(g)$.

**($\Leftarrow$)** Nếu $d \mid m$: viết $m = dt$, thì

$$g^m = (g^d)^t = 1^t = 1 \pmod{P}$$

**($\Rightarrow$)** Nếu $g^m \equiv 1 \pmod P$: giả sử $d \nmid m$, viết $m = dt + r$ với $0 < r < d$. Khi đó:

$$g^m = (g^d)^t \cdot g^r = 1^t \cdot g^r = g^r \equiv 1 \pmod P$$

Nhưng $d$ được định nghĩa là số nguyên dương **nhỏ nhất** thỏa $g^d = 1$, mà lại tìm được $r < d$ cũng thỏa $g^r = 1$ — mâu thuẫn. Vậy giả sử $d \nmid m$ là sai. $\blacksquare$

**Ví dụ minh họa:** 

Với $P=41$, lấy $g=2$. Tính tay ta được $\text{ord}(2) = 20$ (không có số mũ dương nào nhỏ hơn $20$ cho $2^{\bullet} \equiv 1$).

- Thử $m = 40$: vì $20 \mid 40$ → Bổ đề 1 dự đoán $2^{40} \equiv 1 \pmod{41}$ ✓. 
- Thử $m = 8$: vì $20 \nmid 8$ → Bổ đề 1 dự đoán $2^8 \not\equiv 1$. 

Kiểm tra trực tiếp: $2^8 = 256 = 6 \times 41 + 10 \equiv 10 \pmod{41}$ — đúng là khác 1, khớp với dự đoán. ✓

#### Hệ quả 1

Với $m = P - 1$, theo định lý Fermat nhỏ: $g^{P-1} \equiv 1 \pmod P$.

$$\Rightarrow \text{ord}(g) \mid (P-1)$$

Điều đáng chú ý là kết luận này đúng với **mọi** $g$, bất kể $g$ có phải căn nguyên thủy hay không. Ta thử hai trường hợp trái ngược nhau với $P=41$ ($P-1=40$):

- $g = 3$: Tính được $\text{ord}(3) = 8$ (vì $8$ là số mũ nhỏ nhất cho $3^\bullet \equiv 1 \pmod P$). 

Ta thấy $3$ **không phải** là căn nguyên thủy (vì $ord(3) \ne 40$). Nhưng vẫn có $8 \mid 40$ ✓.

- $g = 6$: Tính được $\text{ord}(6) = 40$ (vì $40$ là số mũ nhỏ nhất cho $6^\bullet \equiv 1 \pmod P$) 

Ta thấy $6$ **là** căn nguyên thủy (vì $ord(6) = 40$). Hiển nhiên $40 \mid 40$ ✓.

Vậy dù $g=3$ (không phải căn nguyên thủy) hay $g=6$ (là căn nguyên thủy), $\text{ord}(g)$ đều là ước của $P-1=40$ — đúng như Hệ quả 1 khẳng định. 

Điều Hệ quả 1 **không** cho biết là $\text{ord}(g)$ cụ thể bằng bao nhiêu trong các ước của $P-1$, mà mới chỉ cho chúng ta biết $\text{ord}(g) \mid (P-1)$ mà thôi.

### 5.2. Bổ đề 2

> Mọi ước thực sự của $P-1$ đều là ước của ít nhất một trong các số $(P-1)/p_i$ với $p_i \in C$.

**Chứng minh:** Gọi $N = P-1$, và $v$ là một ước thực sự của $N$ ($v \neq N$, $v \mid N$).

- Vì $v$ là ước thực sự nên $N/v > 1$
- $N/v$ có ít nhất một ước nguyên tố $q \in C$
- Viết $N/v = qk$ ($k$ nguyên dương) $\Rightarrow N = vqk \Rightarrow N/q = vk \Rightarrow v \mid (N/q)$

Vậy $v$ là ước của $N/q$, với $q \in C$. $\blacksquare$

**Ví dụ minh họa:** 

Đặt $N = P-1 = 40$, $C=\\{2,5\\}$.
 
*Lấy $v = 8$, một ước thực sự của $40$.*
- Theo Bổ đề 2, phải tồn tại $p_i \in \\{2,5\\}$ sao cho $v \mid N/p_i$.
- Ở đây, $p_i$ đó chính là $p_i = 5$, vì lẽ $8$ là ước của $40/5 = 8$, hay $8 \mid 8$ ✓.
 
*Lấy $v = 10$, một ước thực sự của $40$.*
- Theo Bổ đề 2, phải tồn tại $p_i \in \\{2,5\\}$ sao cho $v \mid N/p_i$.
- Ở đây, $p_i$ đó chính là $p_i = 2$, vì lẽ $10$ là ước của $40/2 = 20$, hay $10 \mid 20$ ✓.

---

## 6. Định lý chính

> $$g \text{ là căn nguyên thủy} \iff \forall p_i \in C: \; g^{(P-1)/p_i} \bmod P \neq 1$$

**Chứng minh:** Đặt $d = \text{ord}(g)$.

**($\Rightarrow$)** Giả sử $g$ là căn nguyên thủy, tức $d = P-1$. Xét $p_i \in C$, đặt $u = (P-1)/p_i$.

Vì $p_i \geq 2$ nên $0 < u < P - 1 = d$, tức $d \nmid u$.

Theo Bổ đề 1: $d \nmid u \Rightarrow g^u \bmod P \neq 1$, tức $g^{(P-1)/p_i} \bmod P \neq 1$. ✓

**($\Leftarrow$)** Phản chứng: giả sử $g^{(P-1)/p_i} \bmod P \neq 1$ với mọi $p_i \in C$, nhưng $d \neq P-1$.

- Vì $d \mid (P-1)$ (Theo Hệ quả 1) và $d \neq P-1$ → $d$ là ước thực sự của $P-1$
- Theo Bổ đề 2: tồn tại $q \in C$ sao cho $d \mid (P-1)/q$
- Theo Bổ đề 1: $\Rightarrow g^{(P-1)/q} \equiv 1 \pmod P$ — **mâu thuẫn** với giả thiết

Vậy giả sử sai, suy ra $d = P - 1$. ✓ $\blacksquare$

**Ví dụ minh họa:** 

Với $P=41$, $C=\\{2,5\\}$, các điểm kiểm tra là $g^{(P-1)/2}$ và $g^{(P-1)/5}$ (tức $g^{20}$ và $g^8$).
 
*Thử $g = 2$ (kỳ vọng: bị loại).*
 
Ta có: 

$$2^2 = 4$$

$$2^4 = 16$$

$$2^8 = 16^2 \equiv 10 \pmod{41}$$
 
Vậy $2^8 \not\equiv 1$ — chưa bị loại. Tính tiếp:
 
$$2^{20} = 2^8 \cdot 2^8 \cdot 2^4 \equiv 10 \cdot 10 \cdot 16 \equiv 1 \pmod{41}$$
 
→ $2^{20} \equiv 1$ nên **$g=2$ bị loại**, chứng tỏ $2$ không phải là căn nguyên thủy modulo $P$
 
*Thử $g = 6$ (kỳ vọng: được chấp nhận).*
 
Ta có:

$$6^2=36$$

$$6^4 = 36^2 = 1296 \equiv 25 \pmod{41}$$

$$6^8 = 25^2 = 625 \equiv 10 \neq 1 \pmod{41}$$
 
$$6^{20} = 6^8 \cdot 6^8 \cdot 6^4 \equiv 10 \cdot 10 \cdot 25 = 2500 \equiv 40 \neq 1 \pmod{41} $$

→ Cả hai điểm kiểm tra đều cho thấy $g^{20} \bmod P \neq 1$ và $g^8 \bmod P \neq 1$, nên **$g=6$ là căn nguyên thủy** modulo $P=41$, tức $\text{ord}(6) = 40$.

## 7. Trả lời câu hỏi thứ nhất: vì sao chỉ cần xét các $p_i \in C$?

Như vậy theo hệ quả của Định lý 1: Nếu $\text{ord}(g)$ có "khuyết tật" (tức $\text{ord}(g) < P-1$ và $g$ không phải là căn nguyên thủy), thì **chắc chắn** tồn tại ít nhất một ước nguyên tố $q \in C$ làm lộ khuyết tật đó:

$$g^{(P-1)/q} \equiv 1 \pmod{P}$$

→ Chỉ cần thử tất cả $p_i \in C$, **không cần** thử $p_i^2$, $p_i p_j$, ... Nếu không $p_i$ nào "lộ tẩy" → $d = P-1$.

**Ví dụ minh họa:** 

$g=2$ có $\text{ord}(2) = 20$, một "khuyết tật" thực sự (khác $40$). Hệ quả 2 khẳng định có $q \in \\{2,5\\}$ làm lộ khuyết tật này. Thật vậy: với $q=2$: 

$$2^{(40/2)} = 2^{20} \equiv 1 \pmod P$$ 

"Khuyết tật" lộ ra ngay ở đây, không cần thử thêm $q=5$ hay các ước hợp số như $4, 8, 10$.

---

## 8. Trả lời câu hỏi thứ hai: vì sao là $g^{(P-1)/p_i}$ chứ không phải $g^{p_i}$?

### 8.1. Test đang thực sự kiểm tra điều gì?

Với số nguyên dương $g$ bất kì, $\text{ord}(g) \mid P-1$ theo hệ quả 1, nên $\text{ord}(g) \ne P-1$ khi và chỉ khi $\text{ord}(g)$ là **ước thực sự** của $P-1$.

Vậy chiến lược đúng là: liệt kê ra một tập số $X$ sao cho *bất kỳ ước thực sự $u$ nào* của $P-1$ cũng "mắc bẫy" ít nhất một số trong tập $X$, tức $u$ là ước của ít nhất một số thuộc tập $X$. Nếu $\text{ord}(g)$ không "mắc bẫy" số nào thuộc tập $X$, thì $\text{ord}(g)$ chỉ còn một khả năng: đúng bằng $P-1$.

### 8.2. Vì sao $\{p_i\}$ không phải là "bẫy" tốt

Nếu ta kiểm tra $g^{p_i} \overset{?}{\equiv} 1$, theo Bổ đề 1 điều này tương đương với hỏi $\text{ord}(g) \mid p_i$ hay không. Vì $p_i$ là số nguyên tố, ước của nó chỉ có $1$ và $p_i$. Nói cách khác, phép kiểm tra này **chỉ bẫy được** hai giá trị rất nhỏ: $\text{ord}(g) = 1$ hoặc $\text{ord}(g) = p_i$.

Trong khi đó, $\text{ord}(g)$ hoàn toàn có thể là một ước thực sự **lớn hơn nhiều**, ví dụ $p_i p_j$ hay $p_i^2$ — những giá trị này không chia hết $p_i$ theo nghĩa "$\text{ord}(g) \mid p_i$", nên test $g^{p_i}$ không bắt được chúng. Kết quả: $g$ có thể trượt qua mọi test $g^{p_i} \equiv 1$ nhưng vẫn không phải căn nguyên thủy.

**Ví dụ minh họa:** 

Vẫn dùng $P=41$, $g=2$, $\text{ord}(2)=20$, $C=\\{2,5\\}$

Nếu lỡ dùng test sai $g^{p_i}$ thay vì $g^{(P-1)/p_i}$:

$$2^2 = 4 \neq 1 \pmod P, \qquad 2^5 = 32 \neq 1 \pmod P$$

→ Test sai này **không loại được** $g=2$ ở bất kỳ $p_i \in \\{2,5\\}$ nào, dẫn tới kết luận **nhầm** rằng $2$ là căn nguyên thủy, trong khi thực tế $g=2$ **không phải** căn nguyên thủy. 

### 8.3. Vì sao $\{(P-1)/p_i\}$ lại là "bẫy" đúng

Tập $\{(P-1)/p_i\}$ chính là tập các **ước thực sự lớn nhất** (maximal proper divisors) của $N = P-1$. Có một sự thật số học đơn giản:

> Với mọi ước thực sự $u$ của $N$, luôn tồn tại một ước nguyên tố $p_i$ của $N$ sao cho $u \mid N/p_i$.

*(Đây thực chất chính là Bổ đề 2 đã chứng minh và minh họa ở mục 5 — được nhắc lại ở đây vì nó là chốt chặn cho toàn bộ lập luận của mục này.)*

Nói cách khác: **mọi** ước thực sự của $P-1$, dù nhỏ hay lớn, đều nằm gọn bên trong ít nhất một trong các số $(P-1)/p_i$. Vì vậy nếu $\text{ord}(g)$ không chia hết bất kỳ số $(P-1)/p_i$ nào (tức $g^{(P-1)/p_i} \ne 1$ với mọi $i$), thì $\text{ord}(g)$ không thể là ước thực sự của $P-1$ → nó buộc phải bằng chính $P-1$.

**Tóm gọn:**

| Tập kiểm tra | Bẫy được ord(g) nào? | Đủ để suy ra ord(g) = P−1? |
|---|---|---|
| $\{p_i\}$ | Chỉ $1$ và $p_i$ | ❌ Không |
| $\{(P-1)/p_i\}$ | Mọi ước thực sự của $P-1$ | ✅ Có |

**Tóm tắt suy luận:**

Đặt $d = \text{ord}(g)$ và $N=P-1$
 
*Bước 1 (Bổ đề 2):* nếu $d \neq N$ thì $d$ là ước thực sự của $N$, nên tồn tại $p_i \in C$ sao cho $d \mid (N/p_i)$:
 
$$d \neq N \;\Rightarrow\; \exists\, p_i \in C: d \mid (N/p_i)$$
 
*Bước 2 (Bổ đề 1):* vì $d = \text{ord}(g)$, nên $d \mid (N/p_i) \Leftrightarrow g^{N/p_i} \equiv 1 \pmod P$. Thay vào Bước 1:
 
$$d \neq N \;\Rightarrow\; \exists\, p_i \in C: g^{N/p_i} \equiv 1 \!\!\pmod P$$
 
*Bước 3 (lấy phản đảo):* phủ định $d \neq N$ thành $d = N$; phủ định $\exists\, p_i: g^{N/p_i} \equiv 1$ thành $\forall\, p_i: g^{N/p_i}\not\equiv 1$; đảo chiều mũi tên:
 
$$\forall\, p_i \in C: g^{N/p_i} \not\equiv 1 \!\!\pmod P \;\Rightarrow\; d = N$$
 
Đây chính là chiều ($\Leftarrow$) của Định lý 1.

---

## 9. Brute force đến giá trị $g$ nào thì dừng?

Trên thực tế, mật độ căn nguyên thủy trong khoảng $[2, P-1]$ khá cao ($\varphi(P-1)/(P-1)$), nên thường **không mất nhiều thời gian** để tìm được $g$ đầu tiên thỏa mãn. $g$ sẽ "lộ diện" khá nhanh khi duyệt tuần tự.

Với ví dụ $P=41$ dùng xuyên suốt bài: $\varphi(40) = \varphi(2^3)\cdot\varphi(5) = 4 \times 4 = 16$, tức có 16 căn nguyên thủy trong khoảng $[1,40]$ — mật độ $16/40 = 40\%$. Thực tế $g=2$ trượt (không phải căn nguyên thủy) nhưng chỉ cần thử tiếp vài giá trị là gặp ngay $g=6$.

---

## 10. Một vài fun fact

**Bậc của nhóm là gì?**

Trước khi nói tới các tính chất của số lượng căn nguyên thủy theo một modulo, ta cần một khái niệm nền:

Xét tập $(\mathbb{Z}/n\mathbb{Z})^{\ast} = \\{x \in [1,n] : \gcd(x,n)=1\\}$ — tức tập các số khả nghịch modulo $n$. Tập này cùng phép nhân modulo $n$ tạo thành một **nhóm** (group): có phần tử đơn vị (số 1), phép nhân kết hợp được, và mọi phần tử đều có nghịch đảo (theo Định lý Bézout — không trình bày chi tiết ở đây).

**Bậc của nhóm** (order of the group) đơn giản là *số lượng phần tử* trong nhóm đó. Theo định nghĩa của hàm **Euler's totient** $\varphi(n)$ (hàm đếm số nguyên trong $[1,n]$ coprime với $n$), ta có ngay:

$$\text{bậc của nhóm } (\mathbb{Z}/n\mathbb{Z})^{\ast} = \varphi(n)$$

Ví dụ với $n=9$: $(\mathbb{Z}/9\mathbb{Z})^{\ast} = \\{1,2,4,5,7,8\\}$, có 6 phần tử → bậc của nhóm là $\varphi(9)=6$.

Lưu ý: "bậc của nhóm" và "$\text{ord}(g)$" (bậc của một phần tử $g$, đã định nghĩa ở mục 1.1) là hai khái niệm khác nhau — nhưng có liên hệ chặt: $\text{ord}(g)$ luôn là ước của bậc nhóm (đây chính là bản chất tổng quát của Hệ quả 1, vốn chỉ là trường hợp riêng khi nhóm là $(\mathbb{Z}/P\mathbb{Z})^{\ast}$).

**Nhóm cyclic và generator**

Một nhóm được gọi là **cyclic** nếu tồn tại một phần tử $g$ trong nhóm sao cho $\text{ord}(g)$ bằng đúng bậc của cả nhóm — tức là các lũy thừa của $g$ "quét" được toàn bộ nhóm. Phần tử $g$ đó được gọi là một **generator** (phần tử sinh) của nhóm.

Khi $\text{ord}(g) = P-1$ (bậc của nhóm $(\mathbb{Z}/P\mathbb{Z})^{\ast}$), thì $\{g^0,\dots,g^{P-2}\}$ quét hết toàn bộ nhóm — nói cách khác, **"căn nguyên thủy" chính là tên gọi khác của "generator"** khi nhóm đang xét là $(\mathbb{Z}/P\mathbb{Z})^{\ast}$.

### 10.1. Số lượng căn nguyên thủy của một nhóm cyclic

Có một định lý (không chứng minh ở đây, xin nhận là sự thật): nếu một nhóm cyclic có bậc $m$, thì số lượng generator của nó luôn là $\varphi(m)$.

Áp dụng định lý trên cho nhóm $(\mathbb{Z}/n\mathbb{Z})^{\ast}$: nhóm này có bậc $m = \varphi(n)$, nên (khi nó là cyclic) số generator là:

$$\text{số căn nguyên thủy modulo } n = \varphi(\varphi(n))$$

**Ví dụ minh họa:**

Với $n=18$:

$$\varphi(18) = 6 \quad\Rightarrow\quad \varphi(\varphi(18)) = \varphi(6) = 2$$

Kiểm tra trực tiếp: $(\mathbb{Z}/18\mathbb{Z})^{\ast} = \\{1,5,7,11,13,17\\}$. Tính tay sẽ thấy $\text{ord}(5)=6$ và $\text{ord}(11)=6$ — đúng bằng bậc nhóm $\varphi(18)=6$, nên $5$ và $11$ là hai căn nguyên thủy duy nhất.

**Trường hợp riêng $n=P$ nguyên tố:** vì mọi số từ $1$ đến $P-1$ đều coprime với $P$, nên $\varphi(P) = P-1$. Thay vào công thức tổng quát:

$$\varphi(\varphi(P)) = \varphi(P-1)$$

### 10.2. Không phải modulo nào cũng có căn nguyên thủy

Với modulo $n$ bất kỳ, nhóm $(\mathbb{Z}/n\mathbb{Z})^{\ast}$ **không phải lúc nào cũng cyclic**, tức không phải lúc nào cũng có generator, vì không phải nhóm nào cũng cyclic.

Modulo $n$ có căn nguyên thủy **khi và chỉ khi** $n$ thuộc một trong các dạng:

$$n \in \{1,\ 2,\ 4,\ p^k,\ 2p^k\}$$

với $p$ là số nguyên tố lẻ, $k \ge 1$.

Đây cũng là lý do vì sao thuật toán NTT thường chọn modulo là số **nguyên tố** (hoặc dạng $2p^k$) để đảm bảo nhóm luôn cyclic và căn nguyên thủy luôn tồn tại.