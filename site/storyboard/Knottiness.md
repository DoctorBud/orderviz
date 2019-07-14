---
impress:
  data-x: 1800
  data-y: 0
---

## Knottiness

### And Braids

https://tex.stackexchange.com/questions/455488/illustrating-permutations-as-braided-group-tikz-pgf

$$
\begin{xy}
\xymatrix {
U \ar@/_/[ddr]_y \ar@{.>}[dr]|{\langle x,y \rangle} \ar@/^/[drr]^x \\
 & X \times_Z Y \ar[d]^q \ar[r]_p & X \ar[d]_f \\
 & Y \ar[r]^g & Z
}
\end{xy}
$$


$$
\begin{xy}
0*++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 \ar[r]^3 \ar[d]_3 \POS+/lu 1em/*\txt\tiny{1} &
1 \ar[d]^1 \POS+/ru 1em/*\txt\tiny{2}  \\
3 \POS+/ld 1em/*\txt\tiny{1'} &
1 \POS+/rd 1em/*\txt\tiny{2'}
}}="lu",
"lu"+/r8em/ *++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 \ar[d]_3 \ar[rd]^3 &
1 \ar[l]_3 \\
3 &
1 \ar[u]_1
}}="u",
"u"+/r8em/ *++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 \ar[r]^3 &
1 \ar[ld]^(0.7){3} \ar[d]^2 \\
3 \ar[u]^3 &
1 \ar[lu]_(0.7){3}
}}="ru",
"ru"+/d8em/ *++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 \ar[d]_6 \ar[rd]_(0.7){3} &
1 \ar[l]_3 \\
3 \ar[ru]_(0.7)3 &
1 \ar[u]_2
}}="r",
"r"+/d8em/ *++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 \ar[r]^3 &
1 \ar[ld]^(0.7){3} \ar[d]^1 \\
3 \ar[u]^6 &
1 \ar[lu]_(0.7){3}
}}="rd",
"rd"+/l8em/ *++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 \ar[d]_3 &
1 \ar[l]_3 \\
3 \ar[ru]_3 &
1 \ar[u]_1
}}="d",
"d"+/l8em/ *++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 \ar[r]^3 &
1 \\
3 \ar[u]^3 &
1 \ar[u]_1
}}="ld",
"lu"+/d8em/ *++[c]\xybox{
\xymatrix @=1.5pc @*[F-] @*[o] @*+= {
3 &
1 \ar[l]_3 \ar[d]^1 \\
3 \ar[u]^3 &
1
}}="l",
\POS "lu" \ar "u"^2
\POS "u" \ar "ru"^1
\POS "ru" \ar "r"^2
\POS "r" \ar "rd"^1
\POS "rd" \ar "d"_2
\POS "d" \ar "ld"_1
\POS "lu" \ar "l"_1
\POS "l" \ar "ld"_2
\end{xy}
$$



---

[Home](:@Home)
