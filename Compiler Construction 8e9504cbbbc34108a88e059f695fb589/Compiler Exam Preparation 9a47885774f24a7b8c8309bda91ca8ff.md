# Compiler Exam Preparation

### Question 8 - Liveness + Data-Flow Analysis

- Algo for backward data-flow analysis - live variables
    
    ![Untitled](Compiler%20Exam%20Preparation%209a47885774f24a7b8c8309bda91ca8ff/Untitled.png)
    
- Define Def(B) for the block transfer function; live variable analysis
    
    The set of variables clearly assigned values (defined) in B before any use of that variable in B
    
    ![Untitled](Compiler%20Exam%20Preparation%209a47885774f24a7b8c8309bda91ca8ff/Untitled%201.png)
    
    ![Untitled](Compiler%20Exam%20Preparation%209a47885774f24a7b8c8309bda91ca8ff/Untitled%202.png)
    
- Define UseB for block transfer function; live variable analysis
    
    The set of variables whose values MAY be used in B BEFORE any definition of that variable in B
    
    B6:
    
    b = a + b
    
    e = c - a
    
    Use(B6) = {a, b, c} ; here, b is defined, but b is used in that definition of b, so it is used before its definition