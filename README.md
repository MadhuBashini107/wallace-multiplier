# Wallace-multiplier

## AIM ##
To design and implement a 4×4 Wallace Tree Multiplier that performs fast binary multiplication by reducing partial products in parallel.

---
## EQUIPMENT USED ##
- Laptop
- Verilog HDL
- Vivado Xilinix

---
## ALGORITHM ##
1. Generate partial products by ANDing each bit of the multiplicand with each bit of the multiplier.
2. Group partial products according to their bit weights.
3. Reduce the grouped bits using Half Adders and Full Adders in parallel.
4. Continue reduction until only two rows of bits remain.
5. Add the final two rows using a carry-propagate adder to obtain the product.

---
## DIAGRAM ##

<img width="478" height="320" alt="image" src="https://github.com/user-attachments/assets/2f20d12f-9f47-4192-80df-a2161435c9ef" />


<img width="644" height="407" alt="image" src="https://github.com/user-attachments/assets/bfc6684f-e0a6-46a1-a6ca-5de3525d93a5" />

---
## PROGRAM ##
### CODE ###
```verilog
module multiplier( input [3:0]a,
input [3:0]b,
output [7:0]p);
wire p00= a[0]&b[0];
wire p01= a[0]&b[1];
wire p02= a[0]&b[2];
wire p03= a[0]&b[3];

wire p10= a[1]&b[0];
wire p11= a[1]&b[1];
wire p12= a[1]&b[2];
wire p13= a[1]&b[3];

wire p20= a[2]&b[0];
wire p21= a[2]&b[1];
wire p22= a[2]&b[2];
wire p23= a[2]&b[3];

wire p30= a[3]&b[0];
wire p31= a[3]&b[1];
wire p32= a[3]&b[2];
wire p33= a[3]&b[3];

wire s1,c1;
assign s1= p01^p10;
assign c1= p01&p10;

wire s2,c2;
assign s2= p02^p11^p20;
assign c2= (p02&p11)|(p11&p20)|(p02&p20);

wire s3,c3;
assign s3= p03^p12^p21;
assign c3= (p03&p12)|(p12&p21)|(p21&p03);

wire s3_2,c3_2;
assign s3_2= s3^p30;
assign c3_2= s3&p30;

wire s4,c4;
assign s4= p13^p22^p31;
assign c4= (p13&p22)|(p22&p31)|(p13&p31);

wire s5,c5;
assign s5= p23^p32;
assign c5= p23&p32;

wire [7:0]r1,r2;
assign r1 = {1'b0,c5,s5,s4,s3_2,s2,s1,p00};
assign r2 = {p33,1'b0,c4,c3_2,c2,c1,1'b0,1'b0};

assign p = r1+r2;
endmodule
```
### TESTBENCH ###
```verilog
`timescale 1ns/1ps
module multiplier_tb;
reg [3:0]a;
reg [3:0]b;
wire [7:0]p;
multiplier uut (a,b,p);
initial 
begin
    a = 4'b1011; b = 4'b1000;#10;
    a = 4'b0001; b = 4'b0010;#10;
    a = 4'b0100; b = 4'b0011;#10;
    a = 4'b0111; b = 4'b1101;#10;
    $finish;
end
endmodule
```
---
## OUTPUT ##

<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/2b430b85-eae0-4e9e-afe2-e48f395f741b" />

---
## RESULT ##
Thus, 4x4 wallace multiplier is designed using Verilog HDL and simulated to verify the outputs for given inputs.
