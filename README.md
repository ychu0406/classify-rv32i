# Assignment 2: Classify

## Part A: Mathematical Functions
In this section, you will implement essential matrix operations used in neural networks. Specifically, you’ll create functions for dot product, matrix multiplication, element-wise ReLU, and argmax.
### Task 1: ReLU
In `relu.s`, implement the ReLU function, which applies the transformation:
ReLU(𝑎)=max(𝑎,0)
Each element of the input array will be individually processed by setting negative values to 0. Since the matrix is stored as a 1D row-major vector, this function operates directly on the flattened array.
```assembly
    loop_start:
    # TODO: Add your own implementation
    beq t1, a1, lend
    
    slli t2, t1, 2       
    add t3, a0, t2      
    lw t4, 0(t3)        
    
    bge t4, zero, lskip  
    sw zero, 0(t3)      
    
    lskip:
    addi t1, t1, 1     
    j loop_start       

    lend:
    jr ra

```

Use `test_relu.s` to set up and run tests on your ReLU function. You can define the matrix values in static memory, and the test will print the matrix before and after applying ReLU.



### Task 2: ArgMax
In `argmax.s`, implement the argmax function, which returns the index of the largest element in a given vector. If multiple elements share the largest value, return the smallest index. This function operates on 1D vectors.
```assembly
    start:
    # TODO: Add your own implementation
    beq t2, a1, comp

    lw t3, 4(a0)       
    ble t3, t0, skip   
    mv t0, t3          
    mv t1, t2          
    skip:
    addi a0, a0, 4     
    addi t2, t2, 1     
    j start

    comp:
    mv a0, t1
    jr ra
```


Use `test_argmax.s` to test the function. You can modify the static vector and its length in the test file. Running the test will print the index of the largest element.

### Task 3.1: Dot Product
In `dot.s`, implement the dot product function, defined as:
dot(𝑎,𝑏)=∑𝑖=0,𝑛−1(𝑎𝑖⋅𝑏𝑖)
You will need to account for stride when accessing the vector elements. No overflow handling is required, so you will not need the mulh instruction.
```assembly
    start:
    bge t1, a2, loop_end
    # TODO: Add your own implementation
    # first array
    mul t2, t1, a3        
    slli t2, t2, 2        # int : 4byte
    add t2, a0, t2        
    lw t3, 0(t2)          
    # second array
    mul t4, t1, a4        
    slli t4, t4, 2       
    add t4, a1, t4        
    lw t5, 0(t4)          
    # product and add to result
    mul t6, t3, t5        
    add t0, t0, t6        
    
    addi t1, t1, 1        
    j start
```
Fill out `test_dot.s` using the provided starter code to test your dot product function. Below is an example:

Vectors:
v0 = [1, 2, 3]  
v1 = [1, 3, 5]  
Result:
dot(v0, v1) = 1 * 1 + 2 * 3 + 3 * 5 = 22

### Task 3.2: Matrix Multiplication
In `matmul.s`, implement matrix multiplication, where:
```assembly
    inner_loop_end:
    # TODO: Add your own implementation
    slli t0, a2, 2        # cols0*4
    add s3, s3, t0        

    addi s0, s0, 1        
    j outer_loop_start
```
## Part B: File Operations and Main
This section focuses on reading and writing matrices to files and building the main function to perform digit classification using the pretrained MNIST weights.
### Task 1: Read Matrix
In `read_matrix.s`, implement the function to read a binary matrix from a file and load it into memory. 
```assembly
    # FIXME: Replace 'mul' with your own implementation
    li s1, 0            # init
    beq t1, zero, done_mul  
    beq t2, zero, done_mul  
    mv t3, t1 

    loop_mul:
    beq t3, zero, done_mul
    add s1, s1, t2
    addi t3, t3, -1
    j loop_mul

    done_mul:
```

### Task 2: Write Matrix
In `write_matrix.s`, implement the function to write a matrix to a binary file. 
```assembly
    # FIXME: Replace 'mul' with your own implementation
    li s4, 0            # init
    beq s2, zero, done_mul
    beq s3, zero, done_mul
    mv t0, s2

    loop_mul:
    beq t0, zero, done_mul
    add s4, s4, s3
    addi t0, t0, -1
    j loop_mul     

    done_mul:
```

### Task 3: Classification
In `classify.s`, bring everything together to classify an input using two weight matrices and the ReLU and ArgMax functions. 
- line 170
```assembly
# FIXME: Replace 'mul' with your own implementation
    li a0, 0          # result
    beqz t0, done_mul_1
    beqz t1, done_mul_1
loop_mul_1:
    andi t2, t1, 1
    beqz t2, skip_mul_1
    add a0, a0, t0
skip_mul_1:
    slli t0, t0, 1
    srli t1, t1, 1
    bnez t1, loop_mul_1

done_mul_1:
```
- line 221
```assembly
 # FIXME: Replace 'mul' with your own implementation
    li a1, 0 
    beqz t0, done_mul_2
    beqz t1, done_mul_2
loop_mul_2:
    andi t2, t1, 1
    beqz t2, skip_mul_2
    add a1, a1, t0
skip_mul_2:
    slli t0, t0, 1
    srli t1, t1, 1
    bnez t1, loop_mul_2
done_mul_2:
```
line 255
```assembly
# FIXME: Replace 'mul' with your own implementation
    li a0, 0
    beqz t0, done_mul_3
    beqz t1, done_mul_3
loop_mul_3:
    andi t2, t1, 1
    beqz t2, skip_mul_3
    add a0, a0, t0
skip_mul_3:
    slli t0, t0, 1
    srli t1, t1, 1
    bnez t1, loop_mul_3
done_mul_3:
```
## Results
% ./test.sh all        
```  
test_abs_minus_one (__main__.TestAbs) ... ok
test_abs_one (__main__.TestAbs) ... ok
test_abs_zero (__main__.TestAbs) ... ok
test_argmax_invalid_n (__main__.TestArgmax) ... ok
test_argmax_length_1 (__main__.TestArgmax) ... ok
test_argmax_standard (__main__.TestArgmax) ... ok
test_chain_1 (__main__.TestChain) ... ok
test_classify_1_silent (__main__.TestClassify) ... ok
test_classify_2_print (__main__.TestClassify) ... ok
test_classify_3_print (__main__.TestClassify) ... ok
test_classify_fail_malloc (__main__.TestClassify) ... ok
test_classify_not_enough_args (__main__.TestClassify) ... ok
test_dot_length_1 (__main__.TestDot) ... ok
test_dot_length_error (__main__.TestDot) ... ok
test_dot_length_error2 (__main__.TestDot) ... ok
test_dot_standard (__main__.TestDot) ... ok
test_dot_stride (__main__.TestDot) ... ok
test_dot_stride_error1 (__main__.TestDot) ... ok
test_dot_stride_error2 (__main__.TestDot) ... ok
test_matmul_incorrect_check (__main__.TestMatmul) ... ok
test_matmul_length_1 (__main__.TestMatmul) ... ok
test_matmul_negative_dim_m0_x (__main__.TestMatmul) ... ok
test_matmul_negative_dim_m0_y (__main__.TestMatmul) ... ok
test_matmul_negative_dim_m1_x (__main__.TestMatmul) ... ok
test_matmul_negative_dim_m1_y (__main__.TestMatmul) ... ok
test_matmul_nonsquare_1 (__main__.TestMatmul) ... ok
test_matmul_nonsquare_2 (__main__.TestMatmul) ... ok
test_matmul_nonsquare_outer_dims (__main__.TestMatmul) ... ok
test_matmul_square (__main__.TestMatmul) ... ok
test_matmul_unmatched_dims (__main__.TestMatmul) ... ok
test_matmul_zero_dim_m0 (__main__.TestMatmul) ... ok
test_matmul_zero_dim_m1 (__main__.TestMatmul) ... ok
test_read_1 (__main__.TestReadMatrix) ... ok
test_read_2 (__main__.TestReadMatrix) ... ok
test_read_3 (__main__.TestReadMatrix) ... ok
test_read_fail_fclose (__main__.TestReadMatrix) ... ok
test_read_fail_fopen (__main__.TestReadMatrix) ... ok
test_read_fail_fread (__main__.TestReadMatrix) ... ok
test_read_fail_malloc (__main__.TestReadMatrix) ... ok
test_relu_invalid_n (__main__.TestRelu) ... ok
test_relu_length_1 (__main__.TestRelu) ... ok
test_relu_standard (__main__.TestRelu) ... ok
test_write_1 (__main__.TestWriteMatrix) ... ok
test_write_fail_fclose (__main__.TestWriteMatrix) ... ok
test_write_fail_fopen (__main__.TestWriteMatrix) ... ok
test_write_fail_fwrite (__main__.TestWriteMatrix) ... ok

----------------------------------------------------------------------
Ran 46 tests in 33.403s

OK
```