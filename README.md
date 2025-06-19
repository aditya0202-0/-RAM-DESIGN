# -RAM-DESIGN

COMPANY : CODTECH IT SOLUTIONS

NAME : ADITYA AVINASH GAIKWAD

INTERN ID :CITS0D81

DOMAIN : VLSI

DURATION : 4 WEEKS

MENTOR : NEELA SANTOSH

DESCRIPTION :

SOFTWARE USED- MODELSIM

CODE-

module sync_ram #(

    parameter DATA_WIDTH = 8,
    parameter ADDR_WIDTH = 4
)(
   
    input wire clk,
    input wire write_enable,
    input wire [ADDR_WIDTH-1:0] address,
    input wire [DATA_WIDTH-1:0] write_data,
    output reg [DATA_WIDTH-1:0] read_data
);

    reg [DATA_WIDTH-1:0] memory [2**ADDR_WIDTH-1:0];

    always @(posedge clk) begin
        if (write_enable)
            memory[address] <= write_data;
        read_data <= memory[address];
    end

endmodule

TESTBENCH CODE-

module tb_sync_ram;

    reg clk;
    reg write_enable;
    reg [3:0] address;
    reg [7:0] write_data;
    wire [7:0] read_data;

    sync_ram uut (
        .clk(clk),
        .write_enable(write_enable),
        .address(address),
        .write_data(write_data),
        .read_data(read_data)
    );

    always #5 clk = ~clk;

    initial begin
        clk = 0;
        write_enable = 0;
        address = 0;
        write_data = 0;

        #10 write_enable = 1; address = 4'h1; write_data = 8'hAA;
        #10 address = 4'h2; write_data = 8'hBB;
        #10 write_enable = 0;
        #10 address = 4'h1;
        #10 address = 4'h2;

        #20 $finish;
    end

endmodule
