+++
date = "2020-02-19T21:44:41+06:00"
draft = false
title = "Matrix Exponentiation: O(n²logx) per Query"
tags = ["Matrix Exponentiation"]
index = true
description = "Matrix Exponentiation Optimization"
highlight = true
+++

## 
[এই আর্টিকেল পড়তে অবশ্যই ম্যাট্রিক্স এক্সপোনেনশিয়েশন সম্পর্কে ভালো আইডিয়া থাকতে হবে । ]  

একটা প্রবলেম চিন্তা করা যাক। $100$টি নোডের একটি গ্রাফ দেওয়া আছে । গ্রাফের নোডগুলো বাই-ডিরেকশনাল এজ দিয়ে কানেক্টেড এবং যেকোনো $2$টি এজের মাঝে একাধিক এজ নেই । $1000$টি কুয়েরী থাকবে । প্রত্যেক কুয়েরীতে দুইটি পূর্ণসংখ্যা $u$ এবং $x$ ($x <= 10^7$) দেওয়া থাকবে। বলতে হবে, গ্রাফে নোড $u$ থেকে শুরু হয়ে কতগুলো $x$ লেংথের পথ আছে ।  

এখন আমরা এই প্রবলেমের সলিউশন নিয়ে চিন্তা করব । প্রথমে আমরা এমন একটা গ্রাফের কথা চিন্তা করি, যে গ্রাফে নোডসংখ্যা মাত্র $3$ এবং নোডগুলো হচ্ছে $u, v$  এবং $w$ ।  ধরা যাক, নোড $u$ এর নোড $v$ এবং নোড $w$ এর সাথে এজ আছে । এখান থেকে একটু চিন্তা করলেই দেখা যায়, নোড $u$ থেকে $x$ লেংথের পথ = (নোড $v$ থেকে $x-1$ লেংথের পথ + নোড $w$ থেকে $x-1$ লেংথের পথ) । তাহলে নোড $u$ থেকে $k$ লেংথের পথ $f[u][x]$ হলে আমরা লিখতে পারি, 

$$f[u][x] = f[v][x-1] + f[w][x-1]$$

এই রিকারেন্স রিলেশনের বেইস কেস হচ্ছে $f[u][0]$ । যেহেতু, $u$ থেকে শুরু হয়ে $0$ লেংথের পথ মানে হচ্ছে $u$ থেকে কোথাও না যাওয়া এবং এমন ঘটনা শুধু একভাবেই সম্ভব, সেহেতু $f[u][0] = 1$ ।

গ্রাফ যদি $(w\leftrightarrow u\leftrightarrow v)$ এমন হয়, তাহলে $1$ লেংথের পথের জন্য আমরা লিখতে পারি,

$$\left[\begin{matrix}
  f[u][1] \newline
  f[v][1] \newline
  f[w][1]
\end{matrix}\right] = \left[\begin{matrix}
  1 & 1 & 0 \newline
  0 & 0 & 1 \newline
  0 & 0 & 1
\end{matrix}\right] \times \left[\begin{matrix}
  f[w][0] \newline
  f[v][0] \newline
  f[u][0]
\end{matrix}\right]$$

সুতরাং, $x$ লেংথের পথের জন্যঃ

$$\left[\begin{matrix}
  f[u][x] \newline
  f[v][x] \newline
  f[w][x]
\end{matrix}\right] = \left[\begin{matrix}
  1 & 1 & 0 \newline
  0 & 0 & 1 \newline
  0 & 0 & 1
\end{matrix}\right]^x \times \left[\begin{matrix}
  1 \newline
  1 \newline
  1
\end{matrix}\right]$$

অর্থাৎ, এই ইকুয়েশনের প্রথম, দ্বিতীয় এবং তৃতীয় ম্যাট্রিক্সকে যথাক্রমে $F$, $M$ এবং $B$ দিয়ে রিপ্রেজেন্ট করলে আমরা লিখতে পারিঃ

\begin{align}F = M^x \times B \end{align}

$M^x$ সরাসরি লুপ চালিয়ে গুণ করে বের করলে অবশ্যই টাইম কমপ্লেক্সিটিতে হবে না । আমাদের আরো ভালো উপায় বের করতে হবে ।  লক্ষ্য করি, আমরা $a^p$ ($a, p$ পূর্ণসংখ্যা) কে চাইলে এভাবে লিখতে পারিঃ  
$$ a^p = a^{p_1} \times a^{p_2} \times ...... \times a^{p_n} $$, যেখানে $p_1, p_2, ..., p_n$, এরা 2 এর কোনো পাওয়ার ।

যেমনঃ $a^{11} = a^8\times a^2\times a^1$ ।

সুতরাং, $M$ এর $2$ এর পাওয়ারযুক্ত ম্যাট্রিক্সগুলো প্রিক্যালকুলেট করে রেখে দিলে প্রত্যেক কুয়েরীতে $logx$ টা মাল্টিপ্লিকেশন করেই $M^x$ বের করে ফেলা সম্ভব । যেহেতু, $n\times n$ম্যাট্রিক্স মাল্টিপ্লিকেশনের কমপ্লেক্সিটি $O(n^3)$, তাই টোটাল কমপ্লেক্সিটি হবেঃ $O(Q\times n^3logx)$, যেখানে $Q$ হচ্ছে কুয়েরীর সংখ্যা এবং $n$ হচ্ছে নোড নাম্বার বা ম্যাট্রিক্সের ডাইমেনশন। কিন্তু এটা যথেষ্ট ফাস্ট না । $100$টা নোড, $1000$টা কুয়েরীর জন্যঃ $O(Q \times n^3 logx) > 10^{10}$ ।  

আমাদের আরো অপটিমাইজড এপ্রোচ বের করতে হবে । ধরা যাক, $M^x = M^{x_1}\times M^{x_2} \times...\times  M^{x_n}$, যেখানে ${x_1}, {x_2},...,{x_n}$ এরা $2$ এর কোনো পাওয়ার । 

তাহলে $(1)$ নং ইকুয়েশন থেকে,

$F = M^x \times B$

$\Rightarrow F = (M^{x_1}\times M^{x_2}\times...\times M^{x_n}\times B)$

$\Rightarrow F = (M^{x_1}\times M^{x_2}\times...\times (M^{x_n}\times B))$ 

$\Rightarrow F = (M^{x_1}\times M^{x_2}\times...\times B_1)$

$......................$

$......................$

$\Rightarrow F = (M^{x_1}\times B_{n-1})$

$\Rightarrow F = B_{n} $

যেহেতু, প্রতিবার গুণ হচ্ছে লিনিয়ার ম্যাট্রিক্সের সাথে, সেহেতু গুণফলও হবে আরেকটি লিনিয়ার ম্যাট্রিক্স। বর্গ ম্যাট্রিক্সের সাথে লিনিয়ার ম্যাট্রিক্সের গুণ যেহেতু $O(n^2)$ এ করা যায়, সেহেতু টোটাল কমপ্লেক্সিটি হবেঃ
$$O(Q\times n^2logx)$$ , এটা যথেষ্ট ফাস্ট ।

ফাইনালি আমাদের সেই $3$টা নোডের গ্রাফের জন্য,
$$F = B_{n}$$
$$\Rightarrow \left[\begin{matrix}
  f[u][x] \newline
  f[v][x] \newline
  f[w][x] 
\end{matrix}\right] = \left[\begin{matrix}
  a \newline
  b \newline
  c
\end{matrix}\right]$$

, যেখানে $a,b,c$ পূর্ণসংখ্যা এবং $f[u][x] = a, f[v][x] = b, f[w][x] = c$ । নোডের সংখ্যা আরো বাড়ালেও একই প্রসেস অনুসরণ করে ফলাফল বের করা যাবে। 

ম্যাট্রিক্স এক্সপোনেনশিয়েশনের অনেক প্রবলেমেই সব কুয়েরীর জন্য বেইস ম্যাট্রিক্স ফিক্সড থাকলে, এভাবে প্রথমে $O(n^3logx)$ কমপ্লেক্সিটিতে বেইস ম্যাট্রিক্সের $2$ এর পাওয়ারযুক্ত ম্যাট্রিক্সগুলো প্রিক্যালকুলেট করে রেখে দিলে তারপর প্রত্যেক কুয়েরীতে $O(n^2logx)$ কমপ্লেক্সিটিতে ফলাফল বের করা সম্ভব । 

###  

Some practice problems:

1. [Tribonacci](https://algo.codemarshal.org/contests/gub-iupc-18/problems/E) , (Problem in short: Given Tribonacci relation: $f(n) = f(n-1)+f(n-2)+f(n-3)$. Base cases: $f(0)=1, f(1)=1, f(2)=2$. In each query, you'll be given two integers $n, k$. Print the last $k$ digits of $n^{th}$ Tribonacci number. $0<=n<=10^{18}, 1<=Query<=5\times10^5, 1<=k<=6$.)

2. [Rik, the Space Traveler](https://toph.co/p/rik-the-space-traveller)

My template: [Matrix Exponentiation Optimized](https://github.com/Shefin-CSE16/Competitive-Programming/blob/master/Templates/Matrix%20Expo%20Optimized)

