This project implements a fully functional Neural Network Accelerator designed at the Register-Transfer Level (RTL) using SystemVerilog. The design targets an 8-16-4 Multi-Layer Perceptron (MLP) architecture and performs inference using Q1.15 Fixed-Point Arithmetic.

Unlike standard GPU implementations, this design uses no vendor-specific IP cores, making it portable across FPGA platforms. The hardware is verified using a Hardware-Software Co-Design approach, where a Python "Golden Model" (running in Google Colab) drives the RTL simulation via the Cocotb framework to ensure 100% bit-true accuracy.

 Key Features

No Vendor IP: Pure RTL implementation of Matrix Multiplication.

Q1.15 Arithmetic: Efficient 16-bit signed fixed-point math with a 48-bit accumulator to prevent overflow.

Serial Processing Element (PE): Optimized area usage by reusing a single MAC unit over 16 clock cycles.

Python Verification: Automated testbench using cocotb that compares RTL outputs against a Python Golden Model.

Google Colab Ready: The simulation environment is configured to run entirely in the cloud using open-source tools.

Repository Structure

├── accelerator_design.sv   # The Verilog RTL (Top Module + Serial PE)
├── test_accelerator.py     # The Python Testbench (Cocotb + Golden Model)
├── Makefile                # Configuration for Icarus Verilog simulation
├── HAC_Report_Final.pdf    # Detailed Technical Report
└── README.md               # Project Documentation


 System Architecture

The accelerator consists of a Serial Multiply-Accumulate (MAC) unit.

Input: Accepts 16-bit Q1.15 inputs and weights sequentially.

Process: Accumulates products into a 48-bit register.

Output: Performs an arithmetic right shift (>>> 15) to return the result to 16-bit precision.

Network Topology:

Input Layer: 8 Neurons (Padded to 16 for hardware compatibility)

Hidden Layer 1: 16 Neurons (ReLU applied in Software)

Output Layer: 4 Neurons

 How to Run on Google Colab

You can simulate this hardware design without installing any software on your local machine.

Step 1: Upload Files

Upload accelerator_design.sv, test_accelerator.py, and Makefile to your Google Colab file system (or clone this repo).

Step 2: Install Prerequisites

Run the following command in a code cell to install the simulator (Icarus Verilog) and the verification framework (Cocotb):

!sudo apt-get update
!sudo apt-get install iverilog -y
!pip install cocotb


Step 3: Run Simulation

Execute the simulation using the make command. This compiles the Verilog and runs the Python testbench.

!make


Step 4: View Results

The console will output the simulation logs. Look for:

TEST PASSED == HARDWARE ACCURACY: 100.00%
