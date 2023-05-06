Download Link: https://assignmentchef.com/product/solved-ee371-lab-3-display-interface
<br>
In this lab we will learn how to display images. We will use the DE1-SoC Computer’s video-out port to display images on a VGA terminal.

Background Information

The DE1-SoC Computer includes a video-out port with a VGA controller that can be connected to a standard VGA monitor. The VGA controller supports a screen resolution of 640 × 480. The image that is displayed by the VGA controller is derived from two sources: a pixel buffer, and a character buffer. Only the pixel buffer will be used in this exercise, hence we will not discuss the character buffer

Pixel Buffer

The pixel buffer for the video-out port holds the data (color) for each pixel that is displayed by the VGA controller. As illustrated in Figure 1, the pixel buffer provides an image resolution of 320 <em> </em>× 240 pixels,  with the coordinate 0,0 being at the top-left corner of the image. Since the VGA controller supports the screen resolution of 640 ×480, each of the pixel values in the pixel buffer is replicated in both the <em>x </em>and <em>y </em>dimensions when it is being displayed on the VGA screen.




x <sup> </sup>

<table width="0">

 <tbody>

  <tr>

   <td rowspan="9" width="30">012</td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td rowspan="7" width="32">   <strong>. . . </strong></td>

   <td width="16"> </td>

  </tr>

  <tr>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

  </tr>

  <tr>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

  </tr>

  <tr>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

  </tr>

  <tr>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

  </tr>

  <tr>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

  </tr>

  <tr>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

  </tr>

  <tr>

   <td colspan="16" width="277"></td>

  </tr>

  <tr>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="16"> </td>

   <td width="32"><strong>. . . </strong></td>

   <td width="16"> </td>

  </tr>

 </tbody>

</table>

0 1 2 3     <strong><sup>. . .                                                         </sup></strong>319







y










239

Figure 1: Pixel buffer coordinates.

Figure 2<em>a </em>shows that each pixel color is represented as a 16-bit halfword, with five bits for the blue and red components, and six bits for green. As depicted in part <em>b </em>of Figure 2,  pixels are addressed in the  pixel buffer by using the combination of a <em>base </em>address and an <em>x,y </em>offset. In the DE1-SoC Computer the default address of the pixel buffer is 0xC8000000, which corresponds to the starting address of the FPGA on-chip memory. Using this scheme, the pixel at location 0,0 has the address 0xC8000000,  the pixel  1,0 has the address <em>base </em>+ (00000000 000000001 0)<sub>2</sub> = 0xC8000002, the pixel 0,1 has the address <em>base </em>

+ (00000001 000000000 0)<sub>2</sub>  = 0xC8000400,  and the pixel at location 319,239 has the address <em>base  </em>+ (11101111 100111111 0)<sub>2</sub> = 0xC803BE7E.




Figure 2: Pixel values and addresses.




You can create an image by writing color values into the pixel addresses as described above. A dedicated <em>pixel buffer controller </em>reads this pixel data from the memory and sends it to the VGA display. The controller reads the pixel data in sequential order, starting with the pixel data corresponding to the upper-left corner of the VGA screen and proceeding to read the whole buffer until it reaches the data for the lower-right corner. This process is then repeated, continuously. You can modify the pixel data at any time, by writing to the pixel addresses. Writes to the pixel buffer are automatically interleaved in the hardware with the read operations that are performed by the pixel buffer controller.




It is also possible to prepare a new image for the VGA display without changing the content of the pixel buffer, by using the concept of <em>double-buffering</em>. In this scheme two pixel buffers are involved, called the <em>front </em>and <em>back </em>buffers, as described below.

<h2>Double Buffering</h2>

As mentioned above, a pixel buffer controller reads data out of the pixel buffer so that it can be displayed on the VGA screen. This pixel buffer controller includes a programming interface in the form of a set of registers, as illustrated in Figure 3. The register at address 0xFF203020 is called the <em>Buffer </em>register, and the register at address 0xFF203024 is the <em>Backbuffer </em>register. Each of these registers stores the starting address of a pixel buffer.  The Buffer register holds the address of the pixel buffer that is displayed on   the VGA screen. As mentioned above, in the default configuration of the DE1-SoC Computer this Buffer register is set to the address 0xC8000000, which points to the start of the FPGA on-chip memory. The default value of the Backbuffer register is also 0xC8000000, which means that there is only one pixel buffer. But software can modify the address stored in the Backbuffer register, thereby creating a second pixel buffer. An image can be drawn into this second buffer by writing to its pixel addresses. This image is not displayed on the VGA monitor until a pixel buffer <em>swap </em>is performed, as explained below.

A pixel buffer swap is caused by writing the value 1 to the Buffer register.  This write operation does   not directly modify the content of the Buffer register, but instead causes the contents of the Buffer and Backbuffer registers to be swapped. The swap operation does not happen right away; it occurs at the end of a VGA screen-drawing cycle, after the last pixel in the bottom-right corner has been displayed. This time instance is referred to as the <em>vertical synchronization </em>time, and occurs every 1/60 seconds. Software can poll the value of the <em>S </em>bit in the <em>Status </em>register, at address 0xFF20302C, to see when the vertical synchronization has happened. Writing the value 1 into the Buffer register causes <em>S </em>to be set to 1. Then, when the swap of the Buffer and Backbuffer registers has been completed <em>S </em>is reset back to 0. The <em>Status </em>register contains additional bits of information, shown in Figure 3, but these bits are not needed for this exercise. Also, the programming interface includes a <em>Resolution </em>register, shown in the figure, that contains the X and Y resolution of the pixel buffer(s).










Figure 3: Pixel buffer controller registers.




In a typical application the pixel buffer controller is used as follows. While the image contained in the pixel buffer that is pointed to by the Buffer register is being displayed, a new image is drawn into the pixel buffer pointed to by the Backbuffer register. When this new image is ready to be displayed, a pixel buffer swap is performed. Then, the pixel buffer that is now pointed to by the Backbuffer register, which was already displayed, is cleared and the next new image is drawn. In this way, the next image to be displayed is always drawn into the “back” pixel buffer, and the “front” and “back” buffer pointers are swapped when the new image is ready to be displayed. Each time a swap is performed software has to synchronize with the VGA controller by waiting until the <em>S </em>bit in the Status register becomes 0.

<h2>Drawing</h2>

In this lab, you will learn how to implement a simple line-drawing algorithm.




Drawing a line on a screen requires coloring pixels between two points (<em>x</em><sub>1</sub><em>, y</em><sub>1</sub>) and (<em>x</em><sub>2</sub><em>, y</em><sub>2</sub>), such that the pixels represent the desired line as closely as possible. Consider the example in Figure 4, where we want to draw a line between points (1<em>, </em>1) and (12<em>, </em>5). The squares in the figure represent the location and size of pixels on the screen. As indicated in the figure, we cannot draw the line precisely—we can only draw  a shape that is similar to the line by coloring the pixels that fall closest to the line’s ideal location on the screen.




We can use algebra to determine which pixels to color. This is done by using the end points and the slope of the line. The slope of our example line is <em>slope </em>= (<em>y</em><sub>2</sub>  <em>y</em><em>−</em><sub>1</sub> )<em>/</em>(<em>x</em><sub>2</sub>  <em>x</em><sub>1</sub>)<em>−</em> =  4<em>/</em>11. Starting at point (1<em>, </em>1) we move along the <em>x </em>axis and compute the <em>y </em>coordinate for the line as follows:







Figure 4: Drawing a line between points (1<em>, </em>1) and (12<em>, </em>5).







<em>y </em>= <em>y</em><sub>1</sub> + <em>slope </em><em>× </em>(<em>x </em><em>− </em><em>x</em><sub>1</sub>)




Thus, for column <em>x </em>= 2, the <em>y </em>location of the pixel is 1+ <u><sup>4</sup></u><sub>11</sub> <em>×</em> (2<em>−</em>1) = 1 <u><sup>4</sup></u><sub>11</sub> . <sub> </sub>Since pixel locations are defined by integer values we round the <em>y </em>coordinate to the nearest integer, and determine that in column <em>x </em>= 2 we should color the pixel at <em>y </em>= 1. For column <em>x </em>= 3 we perform the calculation <em>y </em>= 1 + <sub>11</sub><u><sup>4</sup></u> <em>×</em> <em> </em>(3 <em>− </em>1) = 1 <u><sup>8</sup></u><sub>11</sub> , <sub> </sub>and round the result to <em>y </em>= 2. Similarly, we perform such computations for each column between <em>x</em><sub>1</sub> and <em>x</em><sub>2</sub>.




The approach of moving along the <em>x </em>axis has drawbacks when a line is steep. A steep line spans more rows than it does columns, and hence has a slope with absolute value greater than 1. In this case our calculations will not produce a smooth-looking line. Also, in the case of a vertical line we cannot use the slope to make a calculation. To address this problem, we can alter the algorithm to move along the <em>y  </em>axis when a line  is steep. With this change, we can implement a line-drawing algorithm known as <em>Bresenham’s algorithm</em>. Pseudo-code for this algorithm is given in Figure 5. The first 15 lines of the algorithm make the needed adjustments depending on whether or not the line is steep. Then, in lines 17 to 22 the algorithm increments the <em>x </em>variable 1 step at a time and computes the <em>y </em>value. The <em>y </em>value is incremented when needed to stay as close to the ideal location of the line as possible. Bresenham’s algorithm calculates an <em>error </em>variable  to decide whether or not to increment each <em>y </em>value. The version of the algorithm shown in Figure 5 uses only integers to perform all calculations. To understand how this algorithm works, you can read about Bresenham’s algorithm in a textbook or by searching for it on the internet.







1 draw line(x0, x1, y0, y1) 2

<ul>

 <li>boolean is steep = abs(y1 <em>− </em>y0) <em>&gt; </em>abs(x1 <em>− </em>x0)</li>

 <li>if is steep then</li>

 <li>swap(x0, y0)</li>

 <li>swap(x1, y1)</li>

 <li>if x0 <em>&gt; </em>x1 then</li>

 <li>swap(x0, x1)</li>

 <li>swap(y0, y1)</li>

</ul>

10

<ul>

 <li>int deltax = x1 <em>− </em>x0</li>

 <li>int deltay = abs(y1 <em>− </em>y0)</li>

 <li>int error = <em>−</em>(deltax / 2)</li>

 <li>int y = y0</li>

 <li>if y0 <em>&lt; </em>y1 then y <u></u>step = 1 else y step = <em>− </em>1</li>

</ul>

16

<ul>

 <li>for x from x0 to x1</li>

 <li>if is steep then draw pixel(y,x) else draw pixel(x,y)</li>

 <li>error = error + deltay</li>

 <li>if error <em>≥ </em>0 then</li>

 <li>y = y + y step</li>

 <li>error = error <em>− </em>deltax</li>

</ul>




Figure 5: Pseudo-code for a line-drawing algorithm.

<strong> </strong>

<h1>Task 1</h1>

Your task for lab 3 is to implement Bresenham’s line algorithm in SystemVerilog and compile it onto your

FPGA to draw lines on a computer monitor. Some files have been uploaded to Canvas to make this easier.




Perform the following:




<ol>

 <li>Download the “lab3template.zip” file from Canvas. This is a compressed folder containing a full Quartus project with some files that will help you work with the VGA output of the DE1 board.</li>

 <li>Observe that the project contains three SystemVerilog files:

  <ol>

   <li>sv, a driver for the VGA port of the board. You don’t need to edit or understand this file, but you might notice it uses a 38,400-byte framebuffer register, similar to what was described above. The ternary operator on the last line of this file controls the colors of the lines you’ll be drawing.</li>

   <li>sv, a skeleton file for you to add your code to implement Bresenham’s algorithm.</li>

   <li>sv, a top-level module which instantiates both of the above modules. This should compile and function without any editing on your part, but you are free to do whatever you want with it.</li>

  </ol></li>

 <li>Ensure the project compiles and produces an output on the VGA monitor.

  <ol>

   <li>There are several monitors in both EEB 137 and EEB 361 which have VGA-to-HDMI adaptors installed. They look like this:</li>

  </ol></li>

</ol>




The HDMI cables of the adapters are already connected to the monitors. They also have a 3.5mm audio jack, which you won’t need, and a USB cable, which powers the adapter. Then, the VGA port connects to your board like this:




<ol>

 <li>With the adapter connected and powered, switch the monitor from displaying mDP to HDMI.</li>

 <li>Compile the project and load it onto your DE1 board. If everything is working correctly, the monitor should be black. If there’s a message on it stating “No HDMI/MHL Cable” then something is not connected properly. <strong>Task 2 </strong></li>

</ol>

Implement Bresenham’s line algorithm.




Some notes about the line_drawer.sv file:

<ol>

 <li>The file takes inputs <em>x0, y0, x1, y1</em> corresponding to the coordinate pairs (x0, y0) and (x1, y1) 2. On positive edges of the input clock <em>clk</em>, the outputs <em>x</em> and <em>y</em> are coordinate pairs on the line between (x0, y0) and (x1, y1). On any given clock cycle, <em>x</em> and <em>y</em> should increment at most one pixel.</li>

 <li>As indicated in the file, you’ll need to create some local registers to keep track of things. Notice that the example is declared as <em>signed</em> and is a bit longer than the <em>x</em> and <em>y</em> inputs/ouputs.</li>

</ol>




Bresenham’s algorithm can get complicated. Ultimately, you’ll want to be able to draw a line between any two arbitrary points on the monitor, regardless of whether you’re drawing to the left or right, up or down, or whether the line is steep or gradual. Instead of doing this all at once, you’ll probably want to work in smaller steps.




The following are suggestions on how to approach this problem, but you can complete this task in whatever way makes the most sense to you.

<ol>

 <li>Assume x0 = x1 or y0 = y1 and use the line_drawer.sv file to draw perfectly straight lines</li>

 <li>Assume that (x0, y0) will be (0,0) and x1 = y1. That is, design an algorithm that only draws perfectly diagonal lines from the origin</li>

 <li>Modify your algorithm to draw perfectly diagonal lines from any arbitrary starting point</li>

 <li>Modify your algorithm to handle lines with gradual slopes, such as a line from (0,0) to (100, 20)</li>

</ol>




Demonstrate that your line algorithm can generate a line between any two coordinates on the monitor.




<h1>Task 3</h1>

Modify the DE1_SoC.sv file to implement the following:

<ol>

 <li>Use your line algorithm to draw a line on the monitor and animate it to move around the screen.</li>

 <li>Implement a reset that, when activated, clears the screen by drawing every pixel to be black. You’ll need to modify the arguments being passed to the VGA_framebuffer module to choose between drawing black or white.</li>

</ol>




Demonstrate that you are able to animate an object moving around the screen and that your reset feature clears the monitor.


