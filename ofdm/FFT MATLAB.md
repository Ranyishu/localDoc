## 鰈形算法

![1718702140893](image/FFTMATLAB/1718702140893.png)

![1718702145617](image/FFTMATLAB/1718702145617.png)


MindSpore AI科学计算系列（43）：快速傅里叶变换——“没有最快，只有更快”

[https://zhuanlan.zhihu.com/p/663814551](https://zhuanlan.zhihu.com/p/663814551)

[1] Cooley J W, Tukey J W. An algorithm for the machine calculation of complex Fourier series[J]. Mathematics of computation, 1965, 19(90): 297-301.

[2] Good I J. The interaction algorithm and practical Fourier analysis[J]. Journal of the Royal Statistical Society Series B: Statistical Methodology, 1958, 20(2): 361-372. [https://doi.org/10.1111/j.2517-6161.1958.tb00300.x](https://link.zhihu.com/?target=https%3A//doi.org/10.1111/j.2517-6161.1958.tb00300.x)

[3] Thomas L H. Using a computer to solve problems in physics[J]. Applications of digital computers, 1963: 44-45.

[4] Duhamel P, Hollmann H. ‘Split radix’FFT algorithm[J]. Electronics letters, 1984, 20(1): 14-16.

[5] Rader C M. Discrete Fourier transforms when the number of data samples is prime[J]. Proceedings of the IEEE, 1968, 56(6): 1107-1108. DOI: 10.1109/PROC.1968.6477

[6] 陈暾, 李志豪, 贾海鹏, 等. 基于 ARMv8 平台的多维 FFT 实现与优化研究[J]. 计算机学报, 2019, 42(11): 2384-2402.

[7] 郭金鑫, 张广婷, 张云泉, 等. Cooley-Tukey FFT 算法高性能实现与优化研究[J]. 计算机科学与探索, 2022, 16(6): 1304. DOI: 10.3778/j.issn.1673-9418.2011092



快速理解FFT算法

[https://zhuanlan.zhihu.com/p/407885496](https://zhuanlan.zhihu.com/p/407885496)

C语言系列之FFT算法实现 ***

[https://zhuanlan.zhihu.com/p/135259438](https://zhuanlan.zhihu.com/p/135259438)

[http://www.fftw.org/doc/](http://www.fftw.org/doc/)


## FFTW

[FFTW中文参考-CSDN博客](https://blog.csdn.net/congwulong/article/details/7576012)

```cpp
#include <complex>

...

std::complex<double>* in  = new std::complex<double>[n];
std::complex<double>* out = new std::complex<double>[n];
...
p = fftw_plan_dft_1d(n,
                     reinterpret_cast<fftw_complex*>(in),
                     reinterpret_cast<fftw_complex*>(out),
                     FFTW_FORWARD, FFTW_ESTIMATE);
...
```



[fftw的使用_fftw库的用法-CSDN博客](https://blog.csdn.net/u011913417/article/details/105482834)

[FFTW使用手册（精品PDF） - 豆丁网 (docin.com)](https://www.docin.com/p-1247883763.html)

[FFTW使用指南 - 简书 (jianshu.com)](https://www.jianshu.com/p/fe49fb424fae)

[Top (FFTW 3.3.10)](http://www.fftw.org/fftw3_doc/)

[FFTW_Manual_in_Chinese/FFTW中文手册-by-xjs.bupt.pdf at master · xjsXjtu/FFTW_Manual_in_Chinese · GitHub](https://github.com/xjsXjtu/FFTW_Manual_in_Chinese/blob/master/FFTW%E4%B8%AD%E6%96%87%E6%89%8B%E5%86%8C-by-xjs.bupt.pdf)


## 8 points

```matlab
%FFT8Point.m
% FFT 8-point (C) Bagus Hanindhito (13211007)
% Twiddle Factor
Wn1 = (sqrt(2)/2)-j*(sqrt(2)/2);
Wn2 = 0 - j1;
Wn3 = -(sqrt(2)/2)-j(sqrt(2)/2);
% Input (diisi manual)
xt0 = 0 + j0;
xt1 = 0 + j0;
xt2 = 0 + j0;
xt3 = 0 + j0;
xt4 = 0 + j0;
xt5 = 0 + j0;
xt6 = 0 + j0;
xt7 = 0 + j0;
% Convert Input to Fixed Point
xt0_real = fi(real(xt0), 1, 16, 12);
xt0_imag = fi(imag(xt0), 1, 16, 12);
xt1_real = fi(real(xt1), 1, 16, 12);
xt1_imag = fi(imag(xt1), 1, 16, 12);
xt2_real = fi(real(xt2), 1, 16, 12);
xt2_imag = fi(imag(xt2), 1, 16, 12);
xt3_real = fi(real(xt3), 1, 16, 12);
xt3_imag = fi(imag(xt3), 1, 16, 12);
xt4_real = fi(real(xt4), 1, 16, 12);
xt4_imag = fi(imag(xt4), 1, 16, 12);
xt5_real = fi(real(xt5), 1, 16, 12);
xt5_imag = fi(imag(xt5), 1, 16, 12);
xt6_real = fi(real(xt6), 1, 16, 12);
xt6_imag = fi(imag(xt6), 1, 16, 12);
xt7_real = fi(real(xt7), 1, 16, 12);
xt7_imag = fi(imag(xt7), 1, 16, 12);
% Display Input
xt0_hex = dec2hex(bin2dec(strcat(xt0_real.bin,xt0_imag.bin)))
xt1_hex = dec2hex(bin2dec(strcat(xt1_real.bin,xt1_imag.bin)))
xt2_hex = dec2hex(bin2dec(strcat(xt2_real.bin,xt2_imag.bin)))
xt3_hex = dec2hex(bin2dec(strcat(xt3_real.bin,xt3_imag.bin)))
xt4_hex = dec2hex(bin2dec(strcat(xt4_real.bin,xt4_imag.bin)))
xt5_hex = dec2hex(bin2dec(strcat(xt5_real.bin,xt5_imag.bin)))
xt6_hex = dec2hex(bin2dec(strcat(xt6_real.bin,xt6_imag.bin)))
xt7_hex = dec2hex(bin2dec(strcat(xt7_real.bin,xt7_imag.bin)))
% Output
xf0 = xt0 + xt1     + xt2     + xt3     + xt4 + xt5     + xt6     + xt7;
xf1 = xt0 + Wn1xt1 + Wn2xt2 + Wn3xt3 - xt4 - Wn1xt5 - Wn2xt6 - Wn3xt7;
xf2 = xt0 + Wn2xt1 - xt2     - Wn2xt3 + xt4 + Wn2xt5 - xt6     - Wn2xt7;
xf3 = xt0 + Wn3xt1 - Wn2xt2 + Wn1xt3 - xt4 - Wn3xt5 + Wn2xt6 - Wn1xt7;
xf4 = xt0 - xt1     + xt2     - xt3     + xt4 - xt5     + xt6     - xt7;
xf5 = xt0 - Wn1xt1 + Wn2xt2 - Wn3xt3 - xt4 + Wn1xt5 - Wn2xt6 + Wn3xt7;
xf6 = xt0 - Wn2xt1 - xt2     + Wn2xt3 + xt4 - Wn2xt5 - xt6     + Wn2xt7;
xf7 = xt0 - Wn3xt1 - Wn2xt2 - Wn1xt3 - xt4 + Wn3xt5 + Wn2xt6 + Wn1xt7;
% Convert Output to Fixed Point
xf0_real = fi(real(xf0), 1, 16, 12);
xf0_imag = fi(imag(xf0), 1, 16, 12);
xf1_real = fi(real(xf1), 1, 16, 12);
xf1_imag = fi(imag(xf1), 1, 16, 12);
xf2_real = fi(real(xf2), 1, 16, 12);
xf2_imag = fi(imag(xf2), 1, 16, 12);
xf3_real = fi(real(xf3), 1, 16, 12);
xf3_imag = fi(imag(xf3), 1, 16, 12);
xf4_real = fi(real(xf4), 1, 16, 12);
xf4_imag = fi(imag(xf4), 1, 16, 12);
xf5_real = fi(real(xf5), 1, 16, 12);
xf5_imag = fi(imag(xf5), 1, 16, 12);
xf6_real = fi(real(xf6), 1, 16, 12);
xf6_imag = fi(imag(xf6), 1, 16, 12);
xf7_real = fi(real(xf7), 1, 16, 12);
xf7_imag = fi(imag(xf7), 1, 16, 12);
% Display Result
xf0_hex = dec2hex(bin2dec(strcat(xf0_real.bin,xf0_imag.bin)))
xf1_hex = dec2hex(bin2dec(strcat(xf1_real.bin,xf1_imag.bin)))
xf2_hex = dec2hex(bin2dec(strcat(xf2_real.bin,xf2_imag.bin)))
xf3_hex = dec2hex(bin2dec(strcat(xf3_real.bin,xf3_imag.bin)))
xf4_hex = dec2hex(bin2dec(strcat(xf4_real.bin,xf4_imag.bin)))
xf5_hex = dec2hex(bin2dec(strcat(xf5_real.bin,xf5_imag.bin)))
xf6_hex = dec2hex(bin2dec(strcat(xf6_real.bin,xf6_imag.bin)))
xf7_hex = dec2hex(bin2dec(strcat(xf7_real.bin,xf7_imag.bin)))
```

## FFT64 point

```matlab

% FFT 64-point (C) Bagus Hanindhito (13211007)

% Define Mode %0 for FFT, 1 for IFFT
mode = 0;
% Twiddle Factor for 8 point FFT
Wn8 = zeros(1,64);
for i=1:1:64
    Wn8(i) = cos(-2*pi*(i-1)/8)+j*sin(-2*pi*(i-1)/8);
end;
% Wn8_1 = (sqrt(2)/2)-j*(sqrt(2)/2);
% Wn8_2 = 0 - j*1;
% Wn8_3 = -(sqrt(2)/2)-j*(sqrt(2)/2);

% Twiddle Factor for 64 point FFT
Wn64 = zeros(1,64);
for i=1:1:64
    Wn64(i)=cos(-2*pi*(i-1)/64)+j*sin(-2*pi*(i-1)/64);
end;

% Input Vector (Manual Input)
B64 = zeros(1,64);
B64(1)  = 0.0012207031250 + j*0.0632324218750;
B64(2)  = 0.0021972656250 + j*0.0620117187500;
B64(3)  = 0.0031738281250 + j*0.0610351562500;
B64(4)  = 0.0041503906250 + j*0.0600585937500;
B64(5)  = 0.0051269531250 + j*0.0590820312500;
B64(6)  = 0.0061035156250 + j*0.0581054687500;
B64(7)  = 0.0070800781250 + j*0.0571289062500;
B64(8)  = 0.0080566406250 + j*0.0561523437500;
B64(9)  = 0.0090332031250 + j*0.0551757812500;
B64(10) = 0.0100097656250 + j*0.0541992187500;
B64(11) = 0.0112304687500 + j*0.0532226562500;
B64(12) = 0.0122070312500 + j*0.0520019531250;
B64(14) = 0.0131835937500 + j*0.0510253906250;
B64(15) = 0.0141601562500 + j*0.0500488281250;
B64(16) = 0.0151367187500 + j*0.0490722656250;
B64(17) = 0.0161132812500 + j*0.0480957031250;
B64(18) = 0.0170898437500 + j*0.0471191406250;
B64(19) = 0.0180664062500 + j*0.0461425781250;
B64(20) = 0.0190429687500 + j*0.0451660156250;
B64(21) = 0.0200195312500 + j*0.0441894531250;
B64(22) = 0.0212402343750 + j*0.0432128906250;
B64(23) = 0.0222167968750 + j*0.0422363281250;
B64(24) = 0.0231933593750 + j*0.0410156250000;
B64(25) = 0.0241699218750 + j*0.0400390625000;
B64(26) = 0.0251464843750 + j*0.0390625000000;
B64(27) = 0.0261230468750 + j*0.0380859375000;
B64(28) = 0.0270996093750 + j*0.0371093750000;
B64(29) = 0.0280761718750 + j*0.0361328125000;
B64(30) = 0.0290527343750 + j*0.0351562500000;
B64(31) = 0.0300292968750 + j*0.0341796875000;
B64(32) = 0.0310058593750 + j*0.0332031250000;
B64(33) = 0.0322265625000 + j*0.0322265625000;
B64(34) = 0.0332031250000 + j*0.0310058593750;
B64(35) = 0.0341796875000 + j*0.0300292968750;
B64(36) = 0.0351562500000 + j*0.0290527343750;
B64(37) = 0.0361328125000 + j*0.0280761718750;
B64(38) = 0.0371093750000 + j*0.0270996093750;
B64(39) = 0.0380859375000 + j*0.0261230468750;
B64(40) = 0.0390625000000 + j*0.0251464843750;
B64(41) = 0.0400390625000 + j*0.0241699218750;
B64(42) = 0.0410156250000 + j*0.0231933593750;
B64(43) = 0.0422363281250 + j*0.0222167968750;
B64(44) = 0.0432128906250 + j*0.0212402343750;
B64(45) = 0.0441894531250 + j*0.0200195312500;
B64(46) = 0.0451660156250 + j*0.0190429687500;
B64(47) = 0.0461425781250 + j*0.0180664062500;
B64(48) = 0.0471191406250 + j*0.0170898437500;
B64(49) = 0.0480957031250 + j*0.0161132812500;
B64(50) = 0.0490722656250 + j*0.0151367187500;
B64(51) = 0.0500488281250 + j*0.0141601562500;
B64(52) = 0.0510253906250 + j*0.0131835937500;
B64(53) = 0.0520019531250 + j*0.0122070312500;
B64(54) = 0.0532226562500 + j*0.0112304687500;
B64(55) = 0.0541992187500 + j*0.0100097656250;
B64(56) = 0.0551757812500 + j*0.0090332031250;
B64(57) = 0.0561523437500 + j*0.0080566406250;
B64(58) = 0.0571289062500 + j*0.0070800781250;
B64(59) = 0.0581054687500 + j*0.0061035156250;
B64(60) = 0.0590820312500 + j*0.0051269531250;
B64(61) = 0.0600585937500 + j*0.0041503906250;
B64(62) = 0.0610351562500 + j*0.0031738281250;
B64(63) = 0.0620117187500 + j*0.0021972656250;
B64(64) = 0.0632324218750 + j*0.0012207031250;

%{
B64(1)  = 0.001 + j*0.001;
B64(2)  = 0.002 + j*0.002;
B64(3)  = 0.003 + j*0.003;
B64(4)  = 0.004 + j*0.004;
B64(5)  = 0.005 + j*0.005;
B64(6)  = 0.006 + j*0.006;
B64(7)  = 0.007 + j*0.007;
B64(8)  = 0.008 + j*0.008;
B64(9)  = -0.001 + j*0.001;
B64(10) = -0.002 + j*0.002;
B64(11) = -0.003 + j*0.003;
B64(12) = -0.004 + j*0.004;
B64(13) = -0.005 + j*0.005;
B64(14) = -0.006 + j*0.006;
B64(15) = -0.007 + j*0.007;
B64(16) = -0.008 + j*0.008;
B64(17) = 0.001 - j*0.001;
B64(18) = 0.002 - j*0.002;
B64(19) = 0.003 - j*0.003;
B64(20) = 0.004 - j*0.004;
B64(21) = 0.005 - j*0.005;
B64(22) = 0.006 - j*0.006;
B64(23) = 0.007 - j*0.007;
B64(24) = 0.008 - j*0.008;
B64(25) = -0.001 - j*0.001;
B64(26) = -0.002 - j*0.002;
B64(27) = -0.003 - j*0.003;
B64(28) = -0.004 - j*0.004;
B64(29) = -0.005 - j*0.005;
B64(30) = -0.006 - j*0.006;
B64(31) = -0.007 - j*0.007;
B64(32) = -0.008 - j*0.008;
B64(33) = 0.001 + j*0.001;
B64(34) = 0.002 + j*0.002;
B64(35) = 0.003 + j*0.003;
B64(36) = 0.004 + j*0.004;
B64(37) = 0.005 + j*0.005;
B64(38) = 0.006 + j*0.006;
B64(39) = 0.007 + j*0.007;
B64(40) = 0.008 + j*0.008;
B64(41) = -0.001 + j*0.001;
B64(42) = -0.002 + j*0.002;
B64(43) = -0.003 + j*0.003;
B64(44) = -0.004 + j*0.004;
B64(45) = -0.005 + j*0.005;
B64(46) = -0.006 + j*0.006;
B64(47) = -0.007 + j*0.007;
B64(48) = -0.008 + j*0.008;
B64(49) = 0.001 - j*0.001;
B64(50) = 0.002 - j*0.002;
B64(51) = 0.003 - j*0.003;
B64(52) = 0.004 - j*0.004;
B64(53) = 0.005 - j*0.005;
B64(54) = 0.006 - j*0.006;
B64(55) = 0.007 - j*0.007;
B64(56) = 0.008 - j*0.008;
B64(57) = -0.001 - j*0.001;
B64(58) = -0.002 - j*0.002;
B64(59) = -0.003 - j*0.003;
B64(60) = -0.004 - j*0.004;
B64(61) = -0.005 - j*0.005;
B64(62) = -0.006 - j*0.006;
B64(63) = -0.007 - j*0.007;
B64(64) = -0.008 - j*0.008;
%}

%Swapping Unit for IFFT
if(mode==1)
    for i=1:1:64
        B64(i) = imag(B64(i))+j*real(B64(i));
    end;
end;

% Convert to Fixed Point in Hexadecimal
B64_Fixed = cell(1,64);
for i=1:1:64
    B64_real_temp = fi(real(B64(i)),1,16,12);
    B64_imag_temp = fi(imag(B64(i)),1,16,12);
    B64_Fixed(i)  = cellstr(dec2hex(bin2dec(strcat(B64_real_temp.bin,B64_imag_temp.bin)),8));
    % Overwrite the value of input
    %B64(i) = B64_real_temp.double + j*B64_imag_temp.double;
end;
B64_Fixed = transpose(B64_Fixed);

% Computing Equation (3)
A64 = zeros(1,64);
T64 = zeros(1,64);
T64_1 = zeros(1,64);
for(t=0:1:7)
    for(s=0:1:7)
        temp2 = 0;
        for(l=0:1:7)
            temp =0;
            for(m=0:1:7)
                temp = temp+B64(l+8*m+1)*Wn8(s*m+1);
            end;
            T64(s+8*l+1) = temp;
            T64_1(s+8*l+1) = temp*Wn64(s*l+1);
            temp2 = temp2 + temp*Wn64(s*l+1)*Wn8(l*t+1);
        end;
        A64(s+8*t+1) = temp2;
    end;
end;

% Intermediate Data
T64_Fixed = cell(1,64);
for i=1:1:64
    T64_real_temp = fi(real(T64(i)),1,16,12);
    T64_imag_temp = fi(imag(T64(i)),1,16,12);
    T64_Fixed(i)  = cellstr(dec2hex(bin2dec(strcat(T64_real_temp.bin,T64_imag_temp.bin)),8));
end;
T64_Fixed = transpose(T64_Fixed);

T64_1_Fixed = cell(1,64);
for i=1:1:64
    T64_1_real_temp = fi(real(T64_1(i)),1,16,12);
    T64_1_imag_temp = fi(imag(T64_1(i)),1,16,12);
    T64_1_Fixed(i)  = cellstr(dec2hex(bin2dec(strcat(T64_1_real_temp.bin,T64_1_imag_temp.bin)),8));
end;
T64_1_Fixed = transpose(T64_1_Fixed);

%Swapping Unit for IFFT
if(mode==1)
    for i=1:1:64
        A64(i) = imag(A64(i))+j*real(A64(i));
    end;
end;

% Convert Fixed Point in Hexadecimal
A64_Fixed = cell(1,64);
for i=1:1:64
    A64_real_temp = fi(real(A64(i)),1,16,12);
    A64_imag_temp = fi(imag(A64(i)),1,16,12);
    A64_Fixed(i)  = cellstr(dec2hex(bin2dec(strcat(A64_real_temp.bin,A64_imag_temp.bin)),8));
end;
A64_Fixed = transpose(A64_Fixed);
```

[64pointFFTProcessor/Matlab/FFT64Point.m at master · hibagus/64pointFFTProcessor · GitHub](https://github.com/hibagus/64pointFFTProcessor/blob/master/Matlab/FFT64Point.m)

## matlab plot color

![1718681749042](image/FFTMATLAB/1718681749042.png)
