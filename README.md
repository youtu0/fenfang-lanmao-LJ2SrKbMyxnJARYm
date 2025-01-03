
### 组合逻辑电路设计方法


* **使用assign语句：** 描述简单的组合逻辑电路
* **使用always块：** 描述复杂的组合逻辑电路
	+ 要点：
		- 只在一个always模块中对某一变量进行赋值；
		- 将所有敏感变量列在敏感变量列表中（使用**always@(\*)**）；
		- 使用**阻塞赋值**；
		- 条件语句的完全描述（敏感变量不全或者条件语句的不完全描述将导致不是纯组合逻辑电路，将引入寄存器或者锁存器）


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102231441383-271030204.png)
### 组合逻辑电路设计实例


* **实例1：多路选择器：**



> 要求描述：使用条件表达式实现4路选择器
> 
> 
> ![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102231857706-1641879599.png)



```


|  | //实现代码 |
| --- | --- |
|  | module mux4_1( |
|  | input a, b, c, d, s1, s0, |
|  | output reg y); |
|  |  |
|  | always@(*)  begin |
|  | if(s1==0) begin |
|  | if(s0==0)   y=a; |
|  | else        y=b; |
|  | end |
|  | else      begin |
|  | if(s0==0)   y=c; |
|  | else        y=d; |
|  | end |
|  | end |
|  | endmodule |
|  |  |
|  | //仿真代码 |
|  | `timescale 1ns/100ps |
|  | module mux4_1_tb(); |
|  | reg a, b, c, d, s1, s0; |
|  | wire out; |
|  |  |
|  | mux4_1 U1(a, b, c, d, s1, s0, out); |
|  | initial begin |
|  | s1=0;s0=0;d=0;c=0;b=0;a=0; |
|  | #10 s1=0;s0=0;d=0;c=0;b=0;a=1; |
|  | #10 s1=0;s0=0;d=0;c=0;b=1;a=0; |
|  | #10 s1=0;s0=0;d=0;c=0;b=1;a=1; |
|  | #10 s1=0;s0=1;d=0;c=1;b=0;a=0; |
|  | #10 s1=0;s0=1;d=0;c=1;b=0;a=1; |
|  | #10 s1=0;s0=1;d=0;c=1;b=1;a=0; |
|  | #10 s1=0;s0=1;d=0;c=1;b=1;a=1; |
|  | #10 s1=1;s0=0;d=1;c=0;b=0;a=0; |
|  | #10 s1=1;s0=0;d=1;c=0;b=0;a=1; |
|  | #10 s1=1;s0=0;d=1;c=0;b=1;a=0; |
|  | #10 s1=1;s0=0;d=1;c=0;b=1;a=1; |
|  | #10 s1=1;s0=1;d=1;c=1;b=0;a=0; |
|  | #10 s1=1;s0=1;d=1;c=1;b=0;a=1; |
|  | #10 s1=1;s0=1;d=1;c=1;b=1;a=0; |
|  | #10 s1=1;s0=1;d=1;c=1;b=1;a=1; |
|  | end |
|  | endmodule |


```

        得到仿真波形如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102232027474-1272220469.png)
        综合出的电路结构如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102232117505-284872071.png)
* **实例2：比较器：**



> 要求描述：实现一位二进制比较器
> 
> 
> ![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102232251706-1463343523.png)



```


|  | //实现代码 |
| --- | --- |
|  | module comp_1( |
|  | input in0, in1, |
|  | output reg gt, eq, lt); |
|  |  |
|  | always@(*)	begin |
|  | gt = 1'b0; |
|  | eq = 1'b0; |
|  | lt = 1'b0; |
|  | if(in0>in1)	    gt=1'b1; |
|  | if(in0==in1)	eq=1'b1; |
|  | if(in01'b1; |
|  | end |
|  | endmodule |
|  |  |
|  | //仿真代码 |
|  | `timescale 1ns/100ps |
|  | module comp_1_tb(); |
|  | reg in0, in1; |
|  | wire gt, eq, lt; |
|  |  |
|  | comp_1 U1(in0, in1, gt, eq, lt); |
|  | initial begin |
|  | in0=0;in1=0; |
|  | #10 in0=0;in1=1; |
|  | #10 in0=1;in1=0; |
|  | #10 in0=1;in1=1; |
|  | #10 $finish; |
|  | end |
|  | endmodule |


```

        在本设计中，always块内if语句之前，我们对gt，eq和lt都赋值为0，这样做的重要性是**保证每个输出都被分配一个值**，如果没有这个初始赋值操作，Verilog会认为你不想让它们的值改变，系统将会自动生成一个锁存器，那么得到的电路就不再是一个纯组合电路了。


        下图为未经数据初始化的波形图


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102232433323-694725901.png)
        下图为经过数据初始化的波形图
![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102232438976-1440518699.png)
* **实例3：8\-3编码器**



> 要求描述：真值表如下：
> 
> 
> ![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102232839394-594409388.png)



```


|  | //实现代码 |
| --- | --- |
|  | module encode_8_3( |
|  | input [7:0] in, |
|  | output reg [2:0] out, |
|  | output reg valid); |
|  |  |
|  | always@(*)	begin |
|  | valid = 1'b1; |
|  | case(in) |
|  | 8'b00000001:	out=3'b000; |
|  | 8'b00000010:	out=3'b001; |
|  | 8'b00000100:	out=3'b010; |
|  | 8'b00001000:	out=3'b011; |
|  | 8'b00010000:	out=3'b100; |
|  | 8'b00100000:	out=3'b101; |
|  | 8'b01000000:	out=3'b110; |
|  | 8'b10000000:	out=3'b111; |
|  | default:	valid = 1'b0; |
|  | endcase |
|  | end |
|  | endmodule |
|  |  |
|  | //仿真代码 |
|  | `timescale 1ns/100ps |
|  | module encode_8_3_tb(); |
|  | reg [7:0] in; |
|  | wire [2:0] out; |
|  | wire valid;//增加鲁棒性，以指示输入了不希望的逻辑组合 |
|  |  |
|  | encode_8_3 U1(in, out, valid); |
|  | initial	begin |
|  | in = 8'b00000000; |
|  | #10 in = 8'b00000001; |
|  | #10 in = 8'b00000010; |
|  | #10 in = 8'b00000100; |
|  | #10 in = 8'b00001000; |
|  | #10 in = 8'b00010000; |
|  | #10 in = 8'b00100000; |
|  | #10 in = 8'b01000000; |
|  | #10 in = 8'b10000000; |
|  | #10 in = 8'b00000101; |
|  | #10 $finish; |
|  | end |
|  | endmodule |


```

        仿真得到的波形图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233024915-705918457.png)
        综合出的电路结构如下：
![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233031645-1531882025.png)
* **实例4：3\-8译码器**



> 要求描述：真值表如下：
> 
> 
> ![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233150254-1174997505.png)



```


|  | //实现代码 |
| --- | --- |
|  | module decode_3_8( |
|  | input [2:0] in, |
|  | output reg [7:0] out); |
|  |  |
|  | always@(*)	begin |
|  | case(in) |
|  | 3'b000:	out = 8'b00000001; |
|  | 3'b001:	out = 8'b00000010; |
|  | 3'b010: out = 8'b00000100; |
|  | 3'b011: out = 8'b00001000; |
|  | 3'b100: out = 8'b00010000; |
|  | 3'b101: out = 8'b00100000; |
|  | 3'b110: out = 8'b01000000; |
|  | 3'b111: out = 8'b10000000; |
|  | endcase |
|  | end |
|  | endmodule |
|  |  |
|  | //仿真代码 |
|  | `timescale 1ns/100ps |
|  | module decode_3_8_tb(); |
|  | reg [2:0] in; |
|  | wire [7:0] out; |
|  |  |
|  | decode_3_8 U1(in, out); |
|  | initial	begin |
|  | in = 3'b000; |
|  | #10 in = 3'b001; |
|  | #10 in = 3'b010; |
|  | #10 in = 3'b011; |
|  | #10 in = 3'b100; |
|  | #10 in = 3'b101; |
|  | #10 in = 3'b110; |
|  | #10 in = 3'b111; |
|  | #10 $finish; |
|  | end |
|  | endmodule |


```

        仿真得到的波形图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233157444-856403211.png)
        综合出的电路结构如下：
![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233203010-454587949.png)
* **实例5：8\-3优先编码器**



> **要求描述：**
>         8\-3编码器，要求在任何时候都只有一个输入为1，如果输入有一个以上的1的时候，编码器只能输出一个无效信号来指示当前的状态；而优先编码器的输入**可以同时包括一个以上的1**，但输出状态会按照输入的优先级来确定
>         真值表如下：
> 
> 
> ![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233210100-908894765.png)



```


|  | //实现代码 |
| --- | --- |
|  | module priority_encode_8_3( |
|  | input EI, |
|  | input [7:0] in, |
|  | output reg [2:0] out, |
|  | output reg GS, EO); |
|  |  |
|  | always@(*)	begin |
|  | if(EI==1)	begin	out=3'b111; GS=1'b1; EO=1'b1;	end |
|  | else if(in[7]==0)	begin	out=3'b000; GS=1'b0; EO=1'b1;	end |
|  | else if(in[6]==0)	begin	out=3'b001; GS=1'b0; EO=1'b1;	end |
|  | else if(in[5]==0)	begin	out=3'b010; GS=1'b0; EO=1'b1;	end |
|  | else if(in[4]==0)	begin	out=3'b011; GS=1'b0; EO=1'b1;	end |
|  | else if(in[3]==0)	begin 	out=3'b100; GS=1'b0; EO=1'b1;	end |
|  | else if(in[2]==0)	begin	out=3'b101; GS=1'b0; EO=1'b1;	end |
|  | else if(in[1]==0)	begin	out=3'b110; GS=1'b0; EO=1'b1;	end |
|  | else if(in[0]==0)	begin	out=3'b111; GS=1'b0; EO=1'b1;	end |
|  | else	begin out=3'b111; GS=1'b1; EO=1'b0;	end |
|  | end |
|  | endmodule |
|  |  |
|  | //仿真代码 |
|  | `timescale 1ns/100ps |
|  | module priority_encode_8_3_tb(); |
|  | reg EI; |
|  | reg [7:0] in; |
|  | wire [2:0] out; |
|  | wire GS, EO; |
|  |  |
|  | priority_encode_8_3 U1(EI, in, out, GS, EO); |
|  | initial	begin |
|  | EI=0; in=8'b00000000; |
|  | #10 in=8'b10000000; |
|  | #10 in=8'b11000000; |
|  | #10 in=8'b11100000; |
|  | #10 in=8'b11110000; |
|  | #10 in=8'b11111000; |
|  | #10 in=8'b11111100; |
|  | #10 in=8'b11111110; |
|  | #10 in=8'b11111111; |
|  | #10 EI=1; |
|  | #10 $finish; |
|  | end |
|  | endmodule |


```

        仿真得到的波形图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233216690-1421603185.png)
        综合出的电路结构如下：
![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233222624-527067245.png)
* **实例6：七段数码管**



> **要求描述：**
>         包括七个LED管和一个圆形LED小数点，按照LED单元连接方式可以分为共阳数码管和共阴数码管
> 
> 
> ![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233229333-164051286.png)
> * **共阳数码管**是指将所有发光二极管的阳极接到一起形成公共阳极（COM）的数码管，COM接到逻辑高电平，当某一字段发光二极管的印记为低电平时，相应字段就点亮；
> * **共阴数码管**是指将所有发光二极管的阴极接到一起形成公共阴极的数码管，COM接到逻辑低电平，当某一字段发光二极管的阳极为高电平时，相应字段就点亮
> 
> 
> ![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233236650-1175051725.png)



```


|  | //实现代码 |
| --- | --- |
|  | module hex_7seg( |
|  | input [3:0] hex, |
|  | input dp, |
|  | output reg [7:0] sseg |
|  | ); |
|  |  |
|  | always@(*)  begin |
|  | case(hex) |
|  | 4'h0: sseg[6:0]=7'b0000001; |
|  | 4'h1: sseg[6:0]=7'b1001111; |
|  | 4'h2: sseg[6:0]=7'b0010010; |
|  | 4'h3: sseg[6:0]=7'b0000110; |
|  | 4'h4: sseg[6:0]=7'b1001100; |
|  | 4'h5: sseg[6:0]=7'b0100100; |
|  | 4'h6: sseg[6:0]=7'b0100000; |
|  | 4'h7: sseg[6:0]=7'b0001111; |
|  | 4'h8: sseg[6:0]=7'b0000000; |
|  | 4'h9: sseg[6:0]=7'b0000100; |
|  | 4'ha: sseg[6:0]=7'b0001000; |
|  | 4'hb: sseg[6:0]=7'b1100000; |
|  | 4'hc: sseg[6:0]=7'b0110001; |
|  | 4'hd: sseg[6:0]=7'b1000010; |
|  | 4'he: sseg[6:0]=7'b0110000; |
|  | 4'hf: sseg[6:0]=7'b0111000; |
|  | endcase |
|  | sseg[7]=dp; |
|  | end |
|  | endmodule |


```

### 组合逻辑电路设计时延优化


* **实例1：** 考虑一个4路选择器



```


|  | module mux4_1d( |
| --- | --- |
|  | input a, b, c, d, |
|  | input [3:0] sel, |
|  | output reg y |
|  | ); |
|  |  |
|  | always@(*)  begin |
|  | y=1'b0; //赋初值，由于组合逻辑没有覆盖所有可能的输入情况，综合工具会综合出一个锁存器来保持输出状态，直到下一个有效的输入出现 |
|  | if(sel[0])  y=a; |
|  | if(sel[1])  y=b; |
|  | if(sel[2])  y=c; |
|  | if(sel[3])  y=d; |
|  | end |
|  | endmodule |


```

        综合出的电路结构如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233915887-735359003.png)
        由于if\-else综合出来的逻辑具有优先级，靠前的逻辑少、路径短，则sel\[3]具有更高的优先级，即可以得到该电路的真值表如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233923716-2024110881.png)
        **时延优化思路：尽可能将延迟较大的分支放在电路后端**（减少等待信号准备好的时间）
        假设b信号的延迟较大，那么电路应该修改如下：



```


|  | module mux4_1d( |
| --- | --- |
|  | input a, b, c, d, |
|  | input [3:0] sel, |
|  | output reg y); |
|  |  |
|  | always@(*)	begin |
|  | y=1'b0; |
|  | if(sel[0])	y=a; |
|  | if(sel[2])	y=c; |
|  | if(sel[3])	y=d; |
|  | if(sel[1] & ~(sel[2] | sel[3]))	y=b; |
|  | end |
|  | endmodule |


```

        相应综合出的电路图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233930128-340428364.png)
* **实例2：**



```


|  | module time_delay_1( |
| --- | --- |
|  | input [6:1] A, |
|  | input [5:1] C, |
|  | input late_ctrl, |
|  | output reg Z); |
|  |  |
|  | always@(*)	begin |
|  | if(C[1])	Z=A[1]; |
|  | else if(~C[2]) Z=A[2]; |
|  | else if(C[3])	Z=A[3]; |
|  | else if(C[4] && ~late_ctrl)	Z=A[4]; |
|  | else if(~C[5])	Z=A[5]; |
|  | else	Z=A[6]; |
|  | end |
|  | endmodule |


```

        综合出的电路图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233937982-2021621466.png)
        相应的真值表如下（没有考虑late\_ctrl）
![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233943968-1960708297.png)
        修改后的代码如下：



```


|  | module time_delay_1( |
| --- | --- |
|  | input [6:1] A, |
|  | input [5:1] C, |
|  | input late_ctrl, |
|  | output reg Z); |
|  |  |
|  | reg Z1; |
|  | wire Z2, prev_cond; |
|  |  |
|  | always@(*)	begin |
|  | if(C[1])	Z1=A[1]; |
|  | else if(~C[2]) Z1=A[2]; |
|  | else if(C[3])	Z1=A[3]; |
|  | else if(~C[5])	Z1=A[5]; |
|  | else	Z1=A[6]; |
|  | end |
|  |  |
|  | assign Z2=A[4]; |
|  | assign prev_cond = C[1] || ~C[2] || C[3]; |
|  |  |
|  | always@(*)	begin |
|  | if(C[4] && ~late_ctrl && ~prev_cond) |
|  | Z=Z2; |
|  | else |
|  | Z=Z1; |
|  | end |
|  | endmodule |


```

        相应综合出的电路图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233951184-1244733081.png)
* **实例3：** 加法器



```


|  | module time_delay_2#(parameter N=8)( |
| --- | --- |
|  | input [N-1:0] A, B, C, D, |
|  | output reg [N-1:0] Z |
|  | ); |
|  |  |
|  | always @(*) begin |
|  | if(A+B<24)  Z=C; |
|  | else  Z=D; |
|  | end |
|  | endmodule |


```

        综合出的电路图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102233956859-1001370069.png)
        假设数据A延迟到来，那么可以调整电路如下：



```


|  | module time_delay_2#(parameter N=8)( |
| --- | --- |
|  | input [N-1:0] A, B, C, D, |
|  | output reg [N-1:0] Z |
|  | ); |
|  |  |
|  | always @(*) begin |
|  | if(A<24-B)  Z=C; |
|  | else  Z=D; |
|  | end |
|  | endmodule |


```

        综合出的电路图如下：


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102234002259-1120753708.png)
### 组合逻辑电路设计面积优化


        随着芯片工艺的进步和生产成本的降低，面积显得没有时序问题重要，但是减少设计面积，将降低功耗和成本
        一般综合过程可以对面积进行优化，但在RTL编码中如果注意节约设计的面积，往往可以达到事半功倍的效果。设计中主要涉及触发器和组合结构，其中触发器的数量由**功能**决定，很难减少，且其面积与功能和工艺相关，故**组合电路是电路面积优化的关键**


* **实例1：操作符**


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102234007689-2084356120.png)
* **实例2：资源共享**


![](https://img2024.cnblogs.com/blog/2326690/202501/2326690-20250102234012929-1974448005.png)
        **好的电路是设计出来的，不是综合出来的！！**


 本博客参考[蓝猫加速器官网](https://baoshidao.com)。转载请注明出处！
