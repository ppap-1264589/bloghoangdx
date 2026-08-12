---
layout: default
title: "Primitive Root: Giải mã hai câu hỏi thường bị bỏ qua khi cài thuật toán"
use_math: true
nav_title: "Primitive Root"
nav_order: 5
---

# Primitive Root: Giải mã hai câu hỏi thường bị bỏ qua khi cài thuật toán

## 1. Định nghĩa và ký hiệu

Cho trước:

- $P$: một số nguyên tố
- $g$: một số nguyên thỏa mãn $\gcd(g, P) = 1$
- $\text{ord}(g)$: **bậc** của $g$ theo modulo $P$ — số nguyên dương nhỏ nhất sao cho

$$g^{\text{ord}(g)} \equiv 1 \pmod{P}$$

- $C = \{p_1, p_2, \dots, p_k\}$: tập các ước nguyên tố phân biệt của $P-1$

**Định nghĩa:** $g$ được gọi là **căn nguyên thủy** của $P$ nếu

$$\text{ord}(g) = P - 1$$

tức là $\{g^0, g^1, \dots, g^{P-2}\} \bmod P$ chạy qua đúng tất cả $P-1$ giá trị $\{1, 2, \dots, P-1\}$, không lặp lại.

---

## 2. Vấn đề đặt ra

Cần tìm một giá trị $g$ cụ thể là căn nguyên thủy bậc $P-1$ trên trường đồng dư $P$.

---

## 3. Thuật toán tìm căn nguyên thủy bậc $P-1$

Đi thử **brute force** các giá trị $g = 2, 3, 4, \dots$ với điều kiện $\gcd(g, P) = 1$.

**Thuật toán:** với mỗi $g$, kiểm tra:

> Với mỗi $p_i \in C$:
> - Nếu $g^{(P-1)/p_i} \equiv 1 \pmod{P}$ → $g$ **KHÔNG** phải căn nguyên thủy
>
> Nếu không có $p_i \in C$ nào thỏa điều kiện trên → $g$ **là** căn nguyên thủy cần tìm.

---

## 4. Hai câu hỏi nảy ra

1. Tại sao chỉ cần xét các $p_i \in C$ để kiểm tra $g^{(P-1)/p_i} \bmod P \overset{?}{=} 1$, mà không cần $p_i p_j$, $p_i^2$, hay các ước khác của $P-1$?
2. Tại sao giá trị cần xét là $g^{(P-1)/p_i}$ mà không phải $g^{p_i} \bmod P$?

Ta sẽ chứng minh toán học để trả lời cả hai.

---

## 5. Các bổ đề và hệ quả

### Bổ đề 1

$$g^m \bmod P = 1 \iff \text{ord}(g) \mid m$$

**Chứng minh:** Đặt $d = \text{ord}(g)$.

**($\Leftarrow$)** Nếu $d \mid m$: viết $m = dt$, thì

$$g^m = (g^d)^t = 1^t = 1 \pmod{P}$$

**($\Rightarrow$)** Nếu $g^m \equiv 1 \pmod P$: giả sử $d \nmid m$, viết $m = dt + r$ với $0 < r < d$. Khi đó:

$$g^m = (g^d)^t \cdot g^r = 1^t \cdot g^r = g^r \equiv 1 \pmod P$$

Nhưng $d$ được định nghĩa là số nguyên dương **nhỏ nhất** thỏa $g^d = 1$, mà lại tìm được $r < d$ cũng thỏa $g^r = 1$ — mâu thuẫn. Vậy giả sử $d \nmid m$ là sai. $\blacksquare$

### Hệ quả 1

Với $m = P - 1$, theo định lý Fermat nhỏ: $g^{P-1} \equiv 1 \pmod P$.

$$\Rightarrow \text{ord}(g) \mid (P-1)$$

### Bổ đề 2

> Mọi ước thực sự của $P-1$ đều là ước của ít nhất một trong các số $(P-1)/p_i$ với $p_i \in C$.

**Chứng minh:** Gọi $N = P-1$, và $v$ là một ước thực sự của $N$ ($v \neq N$, $v \mid N$).

- Vì $v$ là ước thực sự nên $N/v > 1$
- $N/v$ có ít nhất một ước nguyên tố $q \in C$
- Viết $N/v = qk$ ($k$ nguyên dương) $\Rightarrow N = vqk \Rightarrow N/q = vk \Rightarrow v \mid (N/q)$

Vậy $v$ là ước của $N/q$, với $q \in C$. $\blacksquare$

---

## 6. Định lý chính

### Định lý 1

$$g \text{ là căn nguyên thủy} \iff \forall p_i \in C: \; g^{(P-1)/p_i} \bmod P \neq 1$$

**Chứng minh:** Đặt $d = \text{ord}(g)$.

**($\Rightarrow$)** Giả sử $g$ là căn nguyên thủy, tức $d = P-1$. Xét $p_i \in C$, đặt $u = (P-1)/p_i$.

Vì $p_i \geq 2$ nên $0 < u < P - 1 = d$, tức $d \nmid u$.

Theo Bổ đề 1: $d \nmid u \Rightarrow g^u \bmod P \neq 1$, tức $g^{(P-1)/p_i} \bmod P \neq 1$. ✓

**($\Leftarrow$)** Phản chứng: giả sử $g^{(P-1)/p_i} \bmod P \neq 1$ với mọi $p_i \in C$, nhưng $d \neq P-1$.

- Vì $d \mid (P-1)$ (Hệ quả 1) và $d \neq P-1$ → $d$ là ước thực sự của $P-1$
- Theo Bổ đề 2: tồn tại $q \in C$ sao cho $d \mid (P-1)/q$
- Theo Bổ đề 1: $\Rightarrow g^{(P-1)/q} \equiv 1 \pmod P$ — **mâu thuẫn** với giả thiết

Vậy giả sử sai, suy ra $d = P - 1$. ✓ $\blacksquare$

### Hệ quả 2 — Trả lời câu hỏi thứ nhất

Nếu $d$ có "khuyết tật" (tức $d < P-1$), thì **chắc chắn** tồn tại ít nhất một ước nguyên tố $q \in C$ làm lộ khuyết tật đó:

$$g^{(P-1)/q} \equiv 1 \pmod{P}$$

→ Chỉ cần thử tất cả $p_i \in C$, **không cần** thử $p_i^2$, $p_i p_j$, ... Nếu không $p_i$ nào "lộ tẩy" → $d = P-1$.

---

## 7. Trả lời câu hỏi thứ hai: vì sao là $g^{(P-1)/p_i}$ chứ không phải $g^{p_i}$?

Nếu kiểm tra $g^{p_i}$ thay vì $g^{(P-1)/p_i}$, theo Bổ đề 1, ta chỉ đang kiểm tra $\text{ord}(g) \mid p_i$ — điều này chỉ loại trừ được trường hợp $\text{ord}(g) \in \{1, p_i\}$.

Nhưng $\text{ord}(g)$ hoàn toàn có thể là một ước thực sự **khác** của $P-1$ (ví dụ $p_i p_j$ hoặc $p_i^2$). Khi đó $g^{p_i} \equiv 1 \pmod P$ vẫn đúng, dù $\text{ord}(g) < P-1$ — dẫn đến kết luận **sai** rằng $g$ là căn nguyên thủy.

**Mấu chốt:** cần một tập "điểm kiểm tra" sao cho *mọi* ước thực sự của $P-1$ đều là ước của ít nhất một điểm trong tập đó.

- Tập $\{p_i\}$ — quá nhỏ, không phủ hết
- Tập $\{(P-1)/p_i\}$ — chính là tập các **ước thực sự lớn nhất** (maximal proper divisor) của $N = P-1$, và mọi ước thực sự khác đều "chui vào" bên trong một trong các số này

**Tóm tắt suy luận:**

$$d \neq N \;\Rightarrow\; \exists\, p_i \in C: d \mid (N/p_i) \;\overset{\text{Bổ đề 1}}{\Longleftrightarrow}\; g^{N/p_i} \equiv 1 \pmod P$$

Lấy phản đảo:

$$g^{N/p_i} \not\equiv 1 \pmod P \; \forall p_i \in C \;\Rightarrow\; d = N$$

---

## 8. Về việc brute force $g$ đến đâu

Trên thực tế, mật độ căn nguyên thủy trong khoảng $[2, P-1]$ khá cao (xấp xỉ $\varphi(P-1)/(P-1)$), nên thường **không mất nhiều thời gian** để tìm được $g$ đầu tiên thỏa mãn — $g$ sẽ "lộ diện" khá nhanh khi duyệt tuần tự.